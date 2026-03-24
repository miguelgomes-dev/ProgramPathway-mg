# PHASE 4: Markers (WF 301-307)

## ✅ Status: COMPLETE
*All 7 workflows (301a-307) fully documented and ready for implementation.*

## 📋 Overview

**Phase 4** is **identical in pattern to Phase 3** (Coaches), but focused on **Markers** (evaluators). Both run **in parallel** after Phase 1 completes.

## 🎯 Objective

- ✅ Contact available markers
- ✅ Confirm they have capacity to evaluate this cohort
- ✅ Send them contracts
- ✅ Ensure signature via Adobe Sign

## 📊 Phase 4 Workflows

| ID | Name | Trigger | Status |
|----|------|---------|--------|
| 301a | Send Capacity Confirmation | Recurrence (~4:30 AM) | "Marker Capacity Pending" |
| 301b | Handle Rejected Invitations | Event | - |
| 302 | Wait for Confirmation | Recurrence (~4:30 AM) | - |
| 303 | Check Confirmed | Recurrence (5:00 AM) | "Marker Capacity Confirmed" |
| 304 | Send Contracts | Recurrence (5:30 AM) | "Sending Contracts" |
| 305 | Send via Adobe Sign | Auto | - |
| 306 | Check Adobe Agreement | Recurrence Daily | - |
| 307 | Check Contracts Confirmed | Recurrence Daily | "Markers Confirmed" |

## ⏰ Typical Timeline

```
AFTER PHASE 1 COMPLETES (day X):
(In parallel with Phase 3 - Coaches)

DAY X + 1, ~4:30 AM:
  - WF 301a sends invitations to markers
  - WF 302 awaits confirmation

DAYS X+1 to X+3:
  - Markers respond confirming availability
  - WF 303 checks if ALL confirmed

DAY X+3, 5:30 AM:
  - WF 304 sends contracts
  - WF 305-306 manages Adobe Sign

DAYS X+4 to X+7:
  - Markers sign contracts
  - WF 307 confirms completion

RESULT: All markers ready
```

## 🔄 Detailed Flow

```
[AFTER PHASE 1 COMPLETED]
         ↓
    Status = "All Trainers Contracts Confirmed"
         ↓
    ┌─────────────────────────────┐
    │ PHASE 3 (Coaches)           │ (Parallel)
    │ WF 201a-207                 │ (Parallel)
    │                             │ (Parallel)
    └─────────────────────────────┘
         ↓
WF 301a (Recurrence Daily ~4:30 AM)
  "Send confirmations to markers"
  Email: "We need your evaluation capacity confirmation"
         ↓
    Status = "Marker Capacity Pending"
         ↓
┌─ WF 301b (Event-based)
│  "If Marker rejected"
│  → Resend invitation
│
WF 302 (Recurrence Daily ~4:30 AM)
  "Await capacity confirmation"
         ↓
WF 303 (Recurrence Daily 5:00 AM)
  "Check if ALL markers confirmed"
  Loop until ALL confirm
         ↓
    Status = "Marker Capacity Confirmed"
         ↓
WF 304-307 (Recurrence 5:30 AM+)
  "Send contracts"
  "Adobe Sign process"
  "Check signatures"
         ↓
    Status = "Markers Contracts Confirmed"
         ↓
    ✅ PHASE 4 COMPLETE
```

## 🎓 Important Details

### What is a Marker?
**Marker** = Evaluator/Reviewer
- Responsible for evaluating student performance
- Provides feedback
- Calculates grades/scores
- Essential for program completion

### Capacity Confirmation (WF 301a-303)
- Marker needs to confirm they have TIME and RESOURCES to evaluate
- Not just "be present"
- It's having educational capacity to assess if student learned

### Marker Contracts (WF 304-307)
- Same pattern as Coaches and Trainers
- Adobe Sign for signing
- Legal documentation
- Complete traceability

## ⚠️ Critical Points

| Critical Point | What we do |
|---|---|
| Marker doesn't respond | Wait 2 days, resend, wait 2 more |
| Marker rejects | WF 301b resends, up to 3 attempts |
| Many markers reject | Manual: find substitutes |
| Time zone difference | Emails sent at night, response expected same day |

## 🔁 Parallel Execution with Phase 3

```
WHEN PHASE 1 ENDS:
         ↓
    ┌──────────────────────┬──────────────────────┐
    │                      │                      │
    │ WF 201a (Coaches)    ↓ WF 301a (Markers)   │
    │ 3:45 AM              │ ~4:30 AM            │
    │                      │                     │
    └──────────────────────┴──────────────────────┘
    
BOTH run SIMULTANEOUSLY
Neither depends on other
Neither blocks the other
Slightly different times to avoid system overload
```

## 🚀 Phase 4 Exit State

When Phase 4 completes:
- ✅ ALL markers confirmed
- ✅ ALL contracts signed
- ✅ System ready for Attendance

## 📊 Comparison: Coaches vs Markers

| Aspect | Coaches | Markers |
|--------|---------|---------|
| Responsibility | Mentoring/Support | Evaluation/Feedback |
| Phase | 3 | 4 |
| Times | 3:45-4:30 AM | ~4:30-5:30 AM |
| Process | Identical | Identical |
| Dependency | None | None (parallel) |

## 🔗 Relationships

- **Parallel with:** Phase 3 (Coaches)
- **Depends on:** Phase 1 (Trainers)
- **Result:** Marker preparation for Attendance

## 📚 Pattern Comparison

See **Phase 3 (Coaches)** - the pattern is exactly the same, only:
- Workflow IDs (201→301)
- Times (3:45→~4:30)
- Actor type (Coach→Marker)
- Status field (Coach_Capacity→Marker_Capacity)

---

**Status:** ✅ Documented | **Updated:** March 2026
