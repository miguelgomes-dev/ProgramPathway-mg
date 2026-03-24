# 207 - Check For Coaches Contracts Confirmed

## 📝 Executive Summary
Final verification that all coach contracts have been signed digitally. Runs daily. Compares total coaches vs confirmed signatories. When all contracts are signed, updates cohort status and prepares for Markers phase.

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily
- **Time:** 05:30 AM GMT (approx)
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 5:30 AM]
         ↓
[Load cohort status reference]
         ↓
[Query: Find cohorts with Status=22 ("Sending Out Coaches Contracts")]
         ↓
[FOR EACH cohort with pending signatures:]
         ├─ Count CONFIRMED contracts (ContractConfirmed=1)
         ├─ Count TOTAL coaches assigned
         └─ Compare: Confirmed == Total?
         ↓
[ALL CONTRACTS SIGNED?]
         ├─ YES → Update Status to 23 ("All Coaches Contracts Confirmed")
         │         → Ready for Phase 4 (Markers)
         │
         └─ NO → Do nothing, wait for next check
         ↓
[Check complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference status mapping |
| 2 | Query Pending Cohorts | Find cohorts with status=22 |
| 3 | Loop Each Cohort | Process all pending cohorts |
| 4 | Count Confirmed | Get contracts with signature |
| 5 | Count Total | Get total coach count |
| 6 | Check Completion | Confirmed == Total? |
| 7 | Update Status (if ready) | Set status to 23 |

## ✅ Completion Logic

```
FOR EACH Cohort in "Sending Out Coaches Contracts" status:
  
  ConfirmedCount = COUNT(CohortCoaches WHERE ContractConfirmed=1)
  TotalCount = COUNT(All CohortCoaches)
  
  IF ConfirmedCount == TotalCount:
    → Status = 23 ("All Coaches Contracts Confirmed")
    → Phase 3 (Coaches) COMPLETE
  ELSE:
    → Wait for next daily check
```

## 📤 Status Transition

| Condition | Action | New Status |
|-----------|--------|-----------|
| All contracts signed | Update cohort | **23** = "All Coaches Contracts Confirmed" |
| Some pending signature | No action | **22** = "Sending Out Coaches Contracts" |

## 🔗 Next Phase
→ **Phase 4: Markers** (runs in parallel)
- **Workflow 301a** ("Send Capacity Confirmation to Markers")
- Independent orchestration
- Same pattern as Coaches

## ⏰ Verification Timeline

```
4:30 AM   → WF 204: Send contracts
5:00 AM   → WF 206: Check signatures (downloads PDFs)
5:30 AM   → WF 207: Verify ALL signed
            (If complete, status=23, Phase 3 DONE)
```

## ⏹️ Phase 3 (Coaches) Exit State

When Phase 3 completes (Status 23):
- ✅ ALL coaches confirmed
- ✅ ALL contracts signed
- ✅ Coaches ready for Attendance
- → Phase 4 (Markers) runs in parallel

## ⚠️ Critical Points

| Scenario | Behavior |
|----------|----------|
| All coaches signed | Status updates immediately to 23 |
| One coach pending | Remains status 22, waits |
| Coach never signs | Stuck at status 22 (manual intervention may be needed) |
| Multiple cohorts | Each processed independently |

## 🔀 Comparison: Phase 2 vs Phase 1

| Aspect | Trainers (108) | Coaches (207) |
|--------|---|---|
| **Start Status** | 5 | 22 |
| **End Status** | 6 | 23 |
| **Verification Time** | 2:30 AM | 5:30 AM |
| **Pattern** | Identical logic |

---

**Status:** ✅ Documented | **PHASE 2 COMPLETE** | **Updated:** March 2026
