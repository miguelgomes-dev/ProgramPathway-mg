# PHASE 3: Coaches (WF 201-207)

## 📋 Overview

**Phase 3** starts **in parallel with Phase 4** (Markers). Its objective is **confirming Coach availability** and ensuring all **contracts were signed**.

## 🎯 Objective

- ✅ Contact available coaches
- ✅ Confirm they have capacity to serve this cohort
- ✅ Send them contracts
- ✅ Ensure signature via Adobe Sign

## 📊 Phase 3 Workflows

| ID | Name | Trigger | Status |
|----|------|---------|--------|
| 201a | Send Capacity Confirmation | Recurrence (3:45 AM) | "Coach Capacity Pending" |
| 201b | Handle Rejected Invitations | Event | - |
| 202 | Wait for Confirmation | Recurrence (3:45 AM) | - |
| 203 | Check Confirmed | Recurrence (4:00 AM) | "Coach Capacity Confirmed" |
| 204 | Send Contracts | Recurrence (4:30 AM) | "Sending Contracts" |
| 205 | Send via Adobe Sign | Auto | - |
| 206 | Check Adobe Agreement | Recurrence Daily | - |
| 207 | Check Contracts Confirmed | Recurrence Daily | "Coaches Confirmed" |

## ⏰ Typical Timeline

```
AFTER PHASE 1 COMPLETES (day X):

DAY X + 1, 3:45 AM:
  - WF 201a sends invitations to coaches
  - WF 202 awaits confirmation

DAYS X+1 to X+3:
  - Coaches respond confirming availability
  - WF 203 checks if ALL confirmed

DAY X+3, 4:30 AM:
  - WF 204 sends contracts
  - WF 205-206 manages Adobe Sign

DAYS X+4 to X+7:
  - Coaches sign contracts
  - WF 207 confirms completion

RESULT: All coaches ready
```

## 🔄 Detailed Flow

```
[AFTER PHASE 1 COMPLETED]
         ↓
    Status = "All Trainers Contracts Confirmed"
         ↓
WF 201a (Recurrence Daily 3:45 AM)
  "Send confirmations to coaches"
  Email: "We need your availability confirmation"
         ↓
    Status = "Coach Capacity Pending"
         ↓
┌─ WF 201b (Event-based)
│  "If Coach rejected"
│  → Resend invitation
│
WF 202 (Recurrence Daily 3:45 AM)
  "Await capacity confirmation"
         ↓
WF 203 (Recurrence Daily 4:00 AM)
  "Check if ALL coaches confirmed"
  Loop until ALL confirm
         ↓
    Status = "Coach Capacity Confirmed"
         ↓
WF 204-207 (Recurrence 4:30 AM+)
  "Send contracts"
  "Adobe Sign process"
  "Check signatures"
         ↓
    Status = "Coaches Contracts Confirmed"
         ↓
    ✅ PHASE 3 COMPLETE
```

## 🎓 Important Details

### What is "Capacity"?
- Not just "I can be there"
- It's "I have resources/time/availability" to do the work
- Coaches need to assess if they can mentor this cohort

### Invitations to Coaches (WF 201a)
- Finds coaches not yet invited
- Sends email to each one
- Email contains: Cohort dates, number of students, contact info

### Coach Rejection (WF 201b)
- If coach marks "Cannot serve", WF 201b triggers
- Resends in 2 days after rejection
- If reject 3x: escalates (finds another coach)

### Coach Contracts (WF 204-207)
- Same process as Trainers
- Adobe Sign sends contract
- Coach receives email
- Signs digitally
- System validates automatically

## ⚠️ Critical Points

| Critical Point | What we do |
|---|---|
| Coach doesn't respond | Wait 2 days, resend, wait 2 more |
| Coach rejects | WF 201b resends, up to 3 attempts |
| Many coaches reject | Manual: find substitutes |
| Adobe Sign delay | Reminder 2x, then manual |

## 🔁 Parallel Execution with Phase 4

```
WHEN PHASE 1 ENDS:
         ↓
    ┌──────────────────────┬──────────────────────┐
    │                      │                      │
    ↓ WF 201a (Coaches)    ↓ WF 301a (Markers)   │
    │ Recurrence 3:45 AM   │ Recurrence ~4:30 AM │
    │ (Parallel)           │ (Parallel)          │
    │                      │                     │
    └──────────────────────┴──────────────────────┘
    
BOTH run SIMULTANEOUSLY
Neither waits for other
Both must finish before Phase 5
```

## 🚀 Phase 3 Exit State

When Phase 3 completes:
- ✅ ALL coaches confirmed
- ✅ ALL contracts signed
- ✅ Coaches ready to begin

## 📊 Comparison: Trainers vs Coaches

| Aspect | Trainers (Phase 1) | Coaches (Phase 3) |
|--------|-------------------|-------------------|
| Trigger | When Schedule created | When Phase 1 completed |
| Confirmation | DATE availability | GENERAL capacity |
| Responsibility | Teach classes | Mentor students |
| Execution | Phase 1, sequential | Parallel with Markers |

## 🔗 Relationships

- **Depends on:** Phase 1 (Trainers)
- **Parallel with:** Phase 4 (Markers)
- **Result:** Coach preparation for Attendance

## 📚 Next Phase

See **Phase 4 (Markers)** - follows exact same pattern!

---

**Status:** ✅ Documented | **Updated:** March 2026
