# 104 - Check For Trainers Dates Confirmed

## 📝 Executive Summary
Periodic verification workflow that checks if all trainers have confirmed their dates for cohorts in "Inviting Trainers" status. Runs daily at 1:00 AM GMT. When all confirmations are received, updates cohort status to "All Trainers Dates Confirmed".

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 01:00 AM GMT
- **Initiator:** System scheduler
- **Start Date:** January 24, 2026

## 🔄 Execution Flow

```
[Daily trigger at 1:00 AM]
         ↓
[Load all cohort statuses reference]
         ↓
[Query: Find cohorts waiting for trainer confirmations]
         ├─ Status = "3" (Inviting Trainers)
         └─ Cohorts still pending
         ↓
[FOR EACH cohort found:]
         ├─ Count CONFIRMED schedules (Confirmed=1)
         ├─ Count TOTAL schedules
         └─ Compare: Confirmed == Total?
         ↓
[ALL TRAINERS CONFIRMED?]
         ├─ YES → Update Status to 4 ("All Trainers Dates Confirmed")
         │         → Trigger WF 105 (Send Contracts)
         │
         └─ NO → Do nothing, wait for next check
         ↓
[Check complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference data for status values |
| 2 | Query Waiting Cohorts | Find cohorts with status=3 |
| 3 | Loop Through Each | Process all pending cohorts |
| 4 | Get Confirmed Schedules | Count trainer confirmations |
| 5 | Get All Schedules | Count total expected |
| 6 | Check Confirmation | Compare: confirmed == total? |
| 7 | Update Status (if ready) | Set status to 4 |

## 🔍 Verification Logic

```
FOR EACH Cohort in "Inviting Trainers" status:
  
  ConfirmedCount = COUNT(Schedules WHERE Confirmed=1)
  TotalCount = COUNT(All Schedules)
  
  IF ConfirmedCount == TotalCount:
    → Status = 4 ("All Trainers Dates Confirmed")
    → Proceed to WF 105
  ELSE:
    → Wait for next daily check
```

## 📤 Status Transition

| Condition | Action | New Status |
|-----------|--------|-----------|
| All trainers confirmed | Update cohort | **4** = "All Trainers Dates Confirmed" |
| Some pending | No action | **3** = "Inviting Trainers" (unchanged) |

## 🔗 Next Workflow
→ **Workflow 105** ("Send Contracts to Trainers")
- Triggered automatically when status = 4

## ⏰ Schedule & Timing

| Aspect | Value |
|--------|-------|
| **Frequency** | Daily |
| **Execution Time** | 01:00 AM GMT |
| **Timezone** | GMT (Standard) |
| **Duration** | Typically 1-3 minutes |

## ⚠️ Critical Points

| Scenario | Behavior |
|----------|----------|
| Only 1 trainer pending | Waits until that trainer confirms |
| Trainer never responds | Remains in Inviting Trainers (stuck) - manual intervention needed |
| Multiple cohorts | Processes all in single run (efficient) |

---

**Status:** ✅ Documented | **Updated:** March 2026
