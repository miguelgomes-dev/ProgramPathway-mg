# 110 - Create Cohorts Units

## 📝 Executive Summary
Final Phase 1 workflow. Creates CohortUnit records (representing each educational unit assigned to a cohort). Runs daily at 3:30 AM GMT. Marks completion of trainer setup phase - next phase (Coaches) begins separately.

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 03:30 AM GMT
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 3:30 AM]
         ↓
[Load cohort status reference]
         ↓
[Query: Find cohorts with Status=8 ("All Online Events Created")]
         ↓
[FOR EACH cohort ready for unit creation:]
         ├─ Update Status → 9 ("Creating Cohorts Units")
         │
         ├─ FOR EACH Unit in this cohort:
         │  └─ Create CohortUnit record
         │     ├─ Link to Cohort
         │     ├─ Link to Unit
         │     ├─ Set StartDate
         │     └─ Set Status = Pending
         │
         └─ Update Status → 10 ("All Cohorts Units Created")
         ↓
[✅ Workflow 110 Complete - PHASE 1 FINISHED]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference status mapping |
| 2 | Query Ready Cohorts | Find cohorts with status=8 |
| 3 | Loop Each Cohort | Process each cohort |
| 4 | Update Status to 9 | Mark as creating units |
| 5 | Get Cohort Schedules | Fetch all schedules for units |
| 6 | Extract Unit IDs | Identify all units |
| 7 | Loop Each Unit | Process each unit |
| 8 | Create CohortUnit | Generate unit record |
| 9 | Update Status to 10 | Mark units creation complete |

## 📝 CohortUnit Entity

**What is a CohortUnit?**
- Represents the assignment of an educational Unit/Module to a specific Cohort
- Links Cohort ↔ Unit relationship
- Each Cohort has multiple Units (subjects/modules)
- Marks the start of unit-level tracking

**Created For:**
- One CohortUnit per Unit in the Cohort
- Extracted from CohortSchedules (which define units/masterclasses)

## 📤 SharePoint Updates

**Table: CohortUnits** (new records)
- `Cohort` → Link to parent Cohort
- `Unit` → Link to educational Unit
- `StartDate` → From Cohort
- `Status` → "Pending" (initial state)

**Table: Cohorts**
- `Status` → 8 → 9 → 10 (transition states)

## 🔗 Next Phase
→ **Phase 2: Coaches** (separate orchestration)
- **Workflow 201a** ("Send Capacity Confirmation to Coaches")
- Runs in parallel with Phase 1 completion
- Independent status tracking

## ⏰ Phase 1 Completion Timeline

```
01:00 AM   → WF 104: Check trainer confirmations (status→4)
01:30 AM   → WF 105: Send contracts (status→5)
02:00 AM   → WF 107: Check signatures (download PDFs)
02:30 AM   → WF 108: Verify all signed (status→6)
03:00 AM   → WF 109: Create Teams meetings (status→8)
03:30 AM   → WF 110: Create units (status→10)
            ↓
    🎉 PHASE 1 COMPLETE 🎉
```

## ⏹️ Status Progression (Full Phase 1)

```
Status 0:  "Pending"
    ↓ (WF 101)
Status 1:  "Preprocessing Data Validation"
    ↓ (WF 101-102a/b)
Status 2:  "Creating Cohort Schedule"
    ↓ (WF 103a sends invites)
Status 3:  "Inviting Trainers"
    ↓ (WF 104 verifies)
Status 4:  "All Trainers Dates Confirmed"
    ↓ (WF 105 sends contracts)
Status 5:  "Sending Out Trainers Contracts"
    ↓ (WF 107-108 verify signatures)
Status 6:  "All Trainers Contracts Confirmed"
    ↓ (WF 109 creates teams)
Status 8:  "All Online Events Created"
    ↓ (WF 110 creates units)
Status 10: "All Cohorts Units Created" ✅
```

## ✅ What's Achieved at This Point

- ✅ Cohort structure created
- ✅ All trainers confirmed and contracted
- ✅ Teams meetings scheduled
- ✅ Educational units mapped to cohort
- ✅ Ready for Coach/Marker coordination

## ⚠️ Key Notes

| Aspect | Detail |
|--------|--------|
| Phase 1 Duration | ~2.5-4 days (WF 101 trigger to WF 110 completion) |
| Trainer Actions | Respond to invitations, sign contracts |
| Automatic Process | No human intervention after trigger |
| Next Phase | Coaches/Markers run in parallel separately |

---

**Status:** ✅ Documented | **PHASE 1 COMPLETE** | **Updated:** March 2026
