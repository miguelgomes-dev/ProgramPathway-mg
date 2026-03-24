# 303 - Check For Markers Capacity Confirmed

## 📝 Executive Summary
Daily verification that all markers have confirmed their capacity. Runs at 5:00 AM GMT (parallel with coaches verification). Compares confirmed markers vs total assigned. When all confirmations received, updates cohort status to "All Markers Capacity Confirmed".

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 05:00 AM GMT
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 5:00 AM]
         ↓
[Load cohort status reference]
         ↓
[Query: Find cohorts with Status=30 ("Confirming Markers Capacity")]
         ↓
[FOR EACH cohort waiting for confirmations:]
         ├─ Count CONFIRMED markers (CapacityConfirmed=1)
         ├─ Count TOTAL markers assigned
         └─ Compare: Confirmed == Total?
         ↓
[ALL MARKERS CONFIRMED?]
         ├─ YES → Update Status to 31 ("All Markers Capacity Confirmed")
         │         → Trigger WF 304 (Send Contracts)
         │
         └─ NO → Do nothing, wait for next check
         ↓
[Check complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference status mapping |
| 2 | Query Waiting Cohorts | Find cohorts with status=30 |
| 3 | Loop Each Cohort | Process all pending cohorts |
| 4 | Count Confirmed | Get markers with capacity confirmed |
| 5 | Count Total | Get total marker count |
| 6 | Check Confirmation | Confirmed == Total? |
| 7 | Update Status (if ready) | Set status to 31 |

## ✅ Completion Logic

```
FOR EACH Cohort in "Confirming Markers Capacity" status:
  
  ConfirmedCount = COUNT(Markers WHERE CapacityConfirmed=1)
  TotalCount = COUNT(All Markers for this cohort)
  
  IF ConfirmedCount == TotalCount:
    → Status = 31 ("All Markers Capacity Confirmed")
    → Proceed to WF 304 (Send Contracts)
  ELSE:
    → Wait for next daily check
```

## 📤 Status Transition

| Condition | Action | New Status |
|-----------|--------|-----------|
| All markers confirmed | Update cohort | **31** = "All Markers Capacity Confirmed" |
| Some pending | No action | **30** = "Confirming Markers Capacity" |

## 🔗 Next Workflow
→ **Workflow 304** ("Send Contracts to Markers")
- Triggered when status = 31

## ⏰ Verification Timeline (Parallel with Coaches)

```
3:45 AM   → WF 201a: Send coach confirmations
            WF 301a: Send marker confirmations (PARALLEL)
            
4:00 AM   → WF 203: Check coach confirmations
            
5:00 AM   → WF 303: Check marker confirmations
            (If complete, status=31, WF 304 triggered)
            
5:30 AM   → WF 304: Send marker contracts
```

## ⚠️ Critical Points

| Scenario | Behavior |
|----------|----------|
| All markers confirmed | Status updates immediately to 31 |
| One marker pending | Remains status 30, waits |
| Marker never responds | Stuck at status 30 (manual intervention may be needed) |
| Multiple cohorts | Each processed independently |

---

**Status:** ✅ Documented | **Updated:** March 2026
