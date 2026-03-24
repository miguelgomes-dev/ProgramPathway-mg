# 203 - Check For Coaches Capacity Confirmed

## 📝 Executive Summary
Daily verification that all coaches have confirmed their capacity. Runs at 4:00 AM GMT. Compares confirmed coaches vs total assigned. When all confirmations received, updates cohort status to "All Coaches Capacity Confirmed" and triggers contract sending.

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 04:00 AM GMT
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 4:00 AM]
         ↓
[Load cohort status reference]
         ↓
[Query: Find cohorts with Status=20 ("Confirming Coaches Capacity")]
         ↓
[FOR EACH cohort waiting for confirmations:]
         ├─ Count CONFIRMED coaches (CapacityConfirmed=1)
         ├─ Count TOTAL coaches assigned
         └─ Compare: Confirmed == Total?
         ↓
[ALL COACHES CONFIRMED?]
         ├─ YES → Update Status to 21 ("All Coaches Capacity Confirmed")
         │         → Trigger WF 204 (Send Contracts)
         │
         └─ NO → Do nothing, wait for next check
         ↓
[Check complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference status mapping |
| 2 | Query Waiting Cohorts | Find cohorts with status=20 |
| 3 | Loop Each Cohort | Process all pending cohorts |
| 4 | Count Confirmed | Get coaches with capacity confirmed |
| 5 | Count Total | Get total coach count |
| 6 | Check Confirmation | Confirmed == Total? |
| 7 | Update Status (if ready) | Set status to 21 |

## ✅ Completion Logic

```
FOR EACH Cohort in "Confirming Coaches Capacity" status:
  
  ConfirmedCount = COUNT(Coaches WHERE CapacityConfirmed=1)
  TotalCount = COUNT(All Coaches for this cohort)
  
  IF ConfirmedCount == TotalCount:
    → Status = 21 ("All Coaches Capacity Confirmed")
    → Proceed to WF 204 (Send Contracts)
  ELSE:
    → Wait for next daily check
```

## 📤 Status Transition

| Condition | Action | New Status |
|-----------|--------|-----------|
| All coaches confirmed | Update cohort | **21** = "All Coaches Capacity Confirmed" |
| Some pending | No action | **20** = "Confirming Coaches Capacity" |

## 🔗 Next Workflow
→ **Workflow 204** ("Send Contracts to Coaches")
- Triggered when status = 21

## ⏰ Verification Timeline

```
3:45 AM   → WF 201a: Send capacity confirmations
4:00 AM   → WF 203: Check all confirmations
            (If complete, status=21, WF 204 triggered)
4:30 AM   → WF 204: Send coach contracts
```

## ⚠️ Critical Points

| Scenario | Behavior |
|----------|----------|
| All coaches confirmed | Status updates immediately to 21 |
| One coach pending | Remains status 20, waits |
| Coach never responds | Stuck at status 20 (manual intervention may be needed) |
| Multiple cohorts | Each processed independently |

## 🔀 Comparison: Coaches vs Trainers

| Aspect | Trainers (104) | Coaches (203) |
|--------|---|---|
| **Verification Time** | 1:00 AM | 4:00 AM |
| **Status Range** | 3 → 4 | 20 → 21 |
| **Wait For** | Date confirmations | Capacity confirmations |
| **Pattern** | Identical logic |

---

**Status:** ✅ Documented | **Updated:** March 2026
