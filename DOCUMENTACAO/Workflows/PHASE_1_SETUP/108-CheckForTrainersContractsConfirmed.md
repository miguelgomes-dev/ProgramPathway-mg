# 108 - Check For Trainers Contracts Confirmed

## 📝 Executive Summary
Final verification that all trainer contracts have been signed digitally. Runs daily at 2:30 AM GMT. Compares total trainers vs confirmed signatories. When all contracts are signed, updates cohort status to "All Trainers Contracts Confirmed" and triggers next phase.

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 02:30 AM GMT
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 2:30 AM]
         ↓
[Load cohort status reference]
         ↓
[Query: Find cohorts with Status=5 ("Sending Out Trainers Contracts")]
         ↓
[FOR EACH cohort with pending signatures:]
         ├─ Count CONFIRMED contracts (ContractConfirmed=1)
         ├─ Count TOTAL trainers (all schedules, excludes reminders)
         └─ Compare: Confirmed == Total?
         ↓
[ALL CONTRACTS SIGNED?]
         ├─ YES → Update Status to 6 ("All Trainers Contracts Confirmed")
         │         → Trigger WF 109 (Create Online Events)
         │
         └─ NO → Do nothing, wait for next check
         ↓
[Check complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference status mapping |
| 2 | Query Pending Cohorts | Find cohorts with status=5 |
| 3 | Loop Each Cohort | Process all pending cohorts |
| 4 | Count Confirmed | Get contracts with signature |
| 5 | Count Total | Get total trainer count |
| 6 | Check Completion | Confirmed == Total? |
| 7 | Update Status (if ready) | Set status to 6 |

## ✅ Completion Logic

```
FOR EACH Cohort in "Sending Out Trainers Contracts" status:
  
  ConfirmedCount = COUNT(Schedules WHERE ContractConfirmed=1)
  TotalCount = COUNT(All Schedules, excludes deadline reminders)
  
  IF ConfirmedCount == TotalCount:
    → Status = 6 ("All Trainers Contracts Confirmed")
    → Proceed to WF 109 (Create Events)
  ELSE:
    → Wait for next daily check
```

## 📤 Status Transition

| Condition | Action | New Status |
|-----------|--------|-----------|
| All contracts signed | Update cohort | **6** = "All Trainers Contracts Confirmed" |
| Some pending signature | No action | **5** = "Sending Out Trainers Contracts" |

## 🔗 Next Workflow
→ **Workflow 109** ("Create Online Events")
- Creates Teams meetings and events
- Triggered when status = 6

## ⏰ Verification Timeline

```
1:30 AM   → WF 105: Send contracts
2:00 AM   → WF 107: Check signatures (downloads PDFs)
2:30 AM   → WF 108: Verify ALL signed
            (If complete, status=6, WF 109 triggered)
3:00 AM   → WF 109: Create Teams events
```

## ⚠️ Critical Points

| Scenario | Behavior |
|----------|----------|
| All trainers signed | Status updates immediately to 6 |
| One trainer pending | Remains status 5, waits | 
| Trainer never signs | Stuck at status 5 (manual intervention may be needed) |
| Multiple cohorts | Each processed independently |

---

**Status:** ✅ Documented | **Updated:** March 2026
