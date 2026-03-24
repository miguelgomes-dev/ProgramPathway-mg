# 🔄 Reusability Guide & Patterns

## Introduction

This guide documents the **design patterns** used in the Programme Pathway Portal solution to facilitate:
- Future maintenance
- Reuse in new projects
- Scalability
- Consistency

---

## 🎯 Identified Patterns

### Pattern 1: Trigger + Periodic Verification

**Where it's used:**
- Availability invitations (WF 103a, 201a, 301a)
- Confirmation checks (WF 104, 203, 303)
- Contract processing

**Structure:**
```
1. EVENT TRIGGER: Something happens in SharePoint
   ↓
2. CONDITION: Simple conditional
   ├─ YES: Execute action
   └─ NO: Wait for next iteration
   ↓
3. RECURRENCE: Check again in X hours
   ↓
4. LOOP: Continue until condition is met
```

**Benefit:** Enables asynchronous actions without blocking flow

**Example in solution:**
- WF 104 checks during off-peak hours if all trainers confirmed
- If not: continue checking
- If yes: progress to next phase

---

### Pattern 2: Rejection Handler

**Where it's used:**
- Rejected invitations (WF 102b, 201b, 301b)

**Structure:**
```
WHEN: Invitation marked as "Rejected" in SharePoint
   ↓
THEN: 
   ├─ Increment attempt counter
   ├─ Wait time before resending
   └─ Resend invitation with updated message
```

**Benefit:** Automatically provides "second chance"

**Implementation:**
- SharePoint field: "Rejection_Count"
- SharePoint field: "Last_Rejection_Date"
- Logic: If count < 3, resend; If ≥ 3, escalate

---

### Pattern 3: Adobe Sign Integration

**Where it's used:**
- Contract signing (WF 106, 205, 305)
- Signature verification (WF 107, 206, 306)

**Structure:**
```
1. GENERATION: Contract generated (Word → PDF via Content Conversion)
2. SENDING: Adobe Sign connector sends for signing
3. WEBHOOK: Adobe Sign notifies when signed
4. VERIFICATION: System validates signature received
5. STORAGE: Signed contract goes to SharePoint
```

**Parameterization:**
- EMAIL: Of the actor (trainer/coach/marker)
- DOCUMENT: Word template with cohort data
- RECIPIENTS: Always 1 (the specific actor)
- REMINDER: Adobe sends automatic reminders

**Benefit:** Complete traceability + legal validity

---

### Pattern 4: Controlled Parallelization

**Where it's used:**
- Coaches and Markers run simultaneously (WF 201-207 parallel with WF 301-307)

**Structure:**
```
PHASE 2 (TRAINERS) COMPLETES
   ↓
MOMENT X: Triggers WF 201a AND WF 301a simultaneously
   ↓
PARALLEL:
├─ Coaches confirm
└─ Markers confirm
   ↓
BOTH must complete before next phase
```

**Implementation:**
- No `Wait` between WF 110 and WF 201a/301a
- Both workflows check if "Phase 1 complete"
- Status field controls synchronization

**Benefit:** Reduces total time by ~33%

---

### Pattern 5: Status Machine (Sequential States)

**Where it's used:**
- "Status" field in Cohort on SharePoint

**Structure:**
```
States are SEQUENTIAL and IMMUTABLE:
0 → 1 → 2 → 3 → 4 → 5 → 6 → 8 → 10 → (20-23) → (30-33)

Each workflow:
├─ READ current status
├─ VALIDATE if can execute (e.g., cannot be 5 if status still 3)
├─ EXECUTE logic
└─ UPDATE to next status
```

**Benefit:** 
- Prevents out-of-order execution
- Traceability of where cohort is
- Easy to understand status at any time

**States:**
- **0-2:** Initial setup
- **3-6:** Trainer phase
- **8:** Events created
- **10:** Units created
- **20-23:** Coach phase
- **30-33:** Marker phase

---

## 🛠️ Pattern Recipes

### Recipe 1: Create a New "Send" Workflow

**Use case:** You need to send something (email, invitation, etc.)

**Steps:**
1. **Trigger**: Recurrence Daily + Condition "Status = X"
2. **Action**: Send Email (Office 365) or Send Message (Teams)
3. **Update**: Change status to "Sent - Awaiting"
4. **Schedule**: Configure to run during off-peak (night)

**Template:**
```json
{
  "trigger": "Recurrence (Daily, XX:XX)",
  "condition": {
    "field": "Status",
    "equals": "PENDING_SEND"
  },
  "actions": [
    { "send": "email|teams" },
    { "updateStatus": "SENT_WAITING_CONFIRMATION" }
  ],
  "retries": 3,
  "scheduleOffPeak": true
}
```

---

### Recipe 2: Create a New "Verification" Workflow

**Use case:** You need to validate if something was confirmed

**Steps:**
1. **Trigger**: Recurrence Daily
2. **Query**: Check if ALL items have "Confirmed" status
3. **Condition**: 
   - If all: Proceed
   - If not: Continue checking tomorrow
4. **Update**: Change Cohort status to next phase

**Template:**
```json
{
  "trigger": "Recurrence (Daily, XX:XX)",
  "query": "SELECT * FROM Confirmations WHERE cohort_id = X AND status != 'Confirmed'",
  "condition": {
    "if": "query.count == 0",
    "then": ["updateCohortStatus", "notifyStakeholders"],
    "else": "requeue_next_iteration"
  }
}
```

---

### Recipe 3: Add Rejection Handling

**Use case:** Someone rejected an invitation, need to resend

**Steps:**
1. **Trigger**: When status = "Rejected"
2. **Action**: Increment "rejection_count"
3. **Condition**:
   - If count < 3: Resend + wait
   - If count ≥ 3: Escalate for manual
4. **Update**: Set last attempt timestamp

**Template:**
```json
{
  "trigger": "When item modified AND status == 'Rejected'",
  "actions": [
    { "increment": "rejection_count" },
    { "condition": {
        "if": "rejection_count < 3",
        "then": ["resend_invitation", "set_next_check_date"],
        "else": ["escalate_to_manual", "notify_admin"]
      }
    }
  ]
}
```

---

## 📋 New Workflow Checklist

When adding a new workflow, validate:

- [ ] **Clear name**: Follows pattern "XXX-Description" (ex: "401-When...")
- [ ] **Trigger defined**: Event-based or Recurrence?
- [ ] **Status control**: Field updated after execution?
- [ ] **Error handling**: Retry logic implemented?
- [ ] **Rejections**: Cases where actor can reject?
- [ ] **Scheduling**: Off-peak if recurrence?
- [ ] **Notifications**: Does anyone need to know the result?
- [ ] **Documentation**: README in Workflows/PHASE_X/_README.md
- [ ] **Testing**: Validated in staging environment?

---

## 📊 Extension Methods

### Add New Phase (ex: Attendance)

**Expected structure:**
```
Workflows/PHASE_5_ATTENDANCE/
├── _README.md (explain this phase)
├── 401-When-attendance-marked.md
├── 402-Check-attendance-completion.md
├── ... (more workflows)
```

**Required updates:**
1. Create PHASE_5 folder
2. Update `00_WORKFLOW_REFERENCE.md` with new workflows
3. Update `ARCHITECTURE.md` with new flow
4. Update `REUSABILITY_GUIDE.md` (this file) if new patterns
5. Update `README.md` with project status

**Expected pattern in Attendance:**
- Probably triggers when attendance is marked
- Checks if quorum was reached
- Attendance reports per trainer/cohort

---

### Integrate New Connector (ex: Power BI, SAP)

**Considerations:**
1. **Authentication**: How does connector authenticate?
2. **Rate Limits**: Any call limits?
3. **Cost**: Affects budget?
4. **Redundancy**: What if connector fails?
5. **Location in flow**: Which workflow uses it?

**Pattern:**
```
NEW_CONNECTOR_ACTION:
├─ Try: Execute connector call
├─ Catch: If fails, log + retry with backoff
└─ Finally: Notify if exceeded max retries
```

---

## 🐛 Common Troubleshooting

### Problem: Workflow "Stuck" at Status X

**Likely cause:**
- Verification (WF 104, 203, 303) waiting for X confirmations but never comes
- A trainer/coach hasn't confirmed

**Solution:**
1. Check SharePoint - who's missing confirmation?
2. Check if email was delivered (Office 365 logs)
3. Manual: Mark as confirmed for test, OR
4. Manual: Resend invitation to person

---

### Problem: Duplicate Emails

**Likely cause:**
- Recurrence triggering multiple times
- No lock/semaphore preventing duplicate action

**Solution:**
1. Add "last_sent_date" field
2. Condition: Only send if difference > 23 hours
3. Prevention: Use "Recurrence" not "Do until"

---

### Problem: Adobe Sign not returning signature

**Likely cause:**
- Wrong email field in contract
- Adobe Sign waiting for signer who didn't receive email
- Webhook not configured

**Solution:**
1. Azure Portal → Verify Adobe Sign Webhook
2. Test manually in Adobe Sign admin console
3. Verify email correct in contract (Word template)

---

## 📚 Additional References

- [ARCHITECTURE.md](ARCHITECTURE.md) - Detailed flows
- [GLOSSARY.md](GLOSSARY.md) - Term definitions
- [Workflows/00_WORKFLOW_REFERENCE.md](Workflows/00_WORKFLOW_REFERENCE.md) - All workflow IDs
- Power Automate Best Practices: [Microsoft Docs](https://docs.microsoft.com)
- Adobe Sign Integration: [Adobe Documentation](https://www.adobe.io)

---

## ✅ Conclusion

The solution is built on **5 main patterns**:
1. ✅ Trigger + Periodic Verification
2. ✅ Rejection Handler
3. ✅ Adobe Sign Integration
4. ✅ Controlled Parallelization
5. ✅ Status Machine (States)

Understanding these patterns facilitates **maintenance, debugging, and extension** of the solution.

**When in doubt, consult this document!**
