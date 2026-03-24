# 307 - Check For Markers Contracts Confirmed

## 📝 Executive Summary
Daily verification at 6:30 AM that all markers have signed their contracts. Compares signed contracts vs total markers. When all signatures received, updates cohort status to "All Markers Contracts Confirmed" — marking Phase 4 (Markers) as COMPLETE. Transition to Phase 5 (Cohort Operational).

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 06:30 AM GMT
- **Purpose:** Final gate for Phase 4 completion

## 🔄 Execution Flow

```
[Daily trigger at 6:30 AM]
         ↓
[Query: Find CohortMarkerContracts for each cohort]
         ↓
[FOR EACH COHORT in "Sending Contracts" or "Verifying Signatures" status:]
         ├─ Count markers with ContractStatus="Signed"
         ├─ Count TOTAL markers assigned
         └─ Compare: All Signed?
         ↓
[ALL MARKERS SIGNED?]
         ├─ YES → Update Cohort Status = 33 ("All Markers Contracts Confirmed")
         │         → PHASE 4 COMPLETE
         │         → Ready for Cohort Operational Activities
         │
         └─ NO → Wait for next daily check
         ↓
[Check complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Query Cohorts in Progress | Find cohorts with pending markers |
| 2 | Loop Each Cohort | Process all waiting cohorts |
| 3 | Count Signed Contracts | Contracts with status="Signed" |
| 4 | Count Total Markers | All markers for cohort |
| 5 | Compare Counts | Signed == Total? |
| 6 | Update Status (if complete) | Set to status 33 |

## ✅ Completion Logic

```
FOR EACH Cohort in "Sending Contracts" or "Verifying Signatures":
  
  SignedCount = COUNT(CohortMarkerContracts WHERE ContractStatus="Signed")
  TotalCount = COUNT(All Markers for this cohort)
  
  IF SignedCount == TotalCount:
    → Cohort Status = 33 ("All Markers Contracts Confirmed")
    → PHASE 4 COMPLETE
    → Proceed to Phase 5 logic
  ELSE:
    → Wait for next daily check
```

## 📤 Status Transition

| From Status | To Status | Condition |
|---|---|---|
| **32** ("Sending Contracts") | **33** ("All Markers Confirmed") | All signed |
| **32** | **32** | Some pending | Wait for next check |

## ⏰ Phase 4 Timeline (Summary)

```
Day 1, 3:45 AM   → WF 301a: Send capacity to markers (status 10→30)
Day 1, 4:30 AM   → WF 301b: Resend if rejected

Day 1, 5:00 AM   → WF 303: Check capacity confirmed (status 30→31)
Day 1, 5:30 AM   → WF 304: Send contracts (status 31→32)
               → For each marker: WF 305 (Adobe Sign)

Day 1, 6:00 AM   → WF 306: Check Adobe Sign signatures
                   Download PDFs when signed

Day 1, 6:30 AM   → WF 307: Check ALL contracts signed
                   IF complete: status 32→33
                   → PHASE 4 COMPLETE ✅

Days 2-10: Marker signing window
         → If markers sign: WF 306 downloads, WF 307 monitors
         → If all sign: status 33, ready for operations
```

## 🏁 Phase 4 Completion

When Workflow 307 sets status = 33:

| Achievement | Details |
|---|---|
| **Phase Complete** | All Markers signed contracts ✅ |
| **Cohort Ready** | Evaluation team assembled |
| **Next Activities** | Phase 5 (Cohort Operational) - attendance, submissions, assessments |
| **Status Immutable** | Status 33 = final state for Markers phase |

## 🔗 Next Workflow
→ **Phase 5** (Cohort Operational Activities)
- Attendance tracking
- Submission management
- Evaluation/assessment workflows
- Certificates and completion
- *(Currently placeholder - to be implemented)*

## 📊 Phase Comparison

| Phase | Actor | Roles | Trigger Status | Complete Status | Next Phase |
|---|---|---|---|---|---|
| **1** | Trainer | Teaching | 0 | 10 | 2 |
| **2** | Coach | Mentoring | 10 | 23 | 3 |
| **3** | Marker | **Evaluation** | 10 | **33** | 5 |
| **4** | — | Cohort Ops | 33 | TBD | — |

## ⚠️ Critical Points

| Scenario | Behavior |
|-----------|----------|
| All markers signed by day 5 | Status updates to 33 on day 5 |
| One marker never signs (10+ days) | Stuck at status 32, manual resend or removal |
| Marker signs after WF 306 runs | Updates in CohortMarkerContract, WF 307 detects next run |
| Multiple cohorts | Each processed independently |

## 🎯 Key Metrics

**Phase 4 Cycle:**
- **Duration:** 3.5 hours (3:45 AM to 6:30 AM)
- **Peak Activity:** Capacity & contract sending (5:30 AM)
- **Verification Points:** 3 (capacity + Adobe Sign + contracts)
- **Success Criterion:** All markers signed within 10 days

---

**Status:** ✅ Documented | **Updated:** March 2026

**🏁 PHASE 4 COMPLETE** ✅ All Markers workflows documented (301a-307)
