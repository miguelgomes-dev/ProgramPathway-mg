# 301a - Send Capacity Confirmation to Markers

## 📝 Executive Summary
Initiates marker recruitment for cohorts. Runs daily at 4:30 AM GMT (parallel with coaches). Creates CohortMarker records for available markers and sends capacity confirmation invitations. Marks beginning of marker coordination phase.

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 04:30 AM GMT (parallel with coaches)
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 4:30 AM]
         ↓
[Load cohort status reference]
         ↓
[Fetch MaxNumberOfMarkersPerCohort setting]
         ↓
[Query: Find cohorts with Status=10 ("All Cohorts Units Created")]
         ↓
[FOR EACH ready cohort:]
         ├─ PARALLEL with Coaches (WF 201a)
         ├─ Create CohortMarker records
         │  ├─ CapacityConfirmed = false
         │  ├─ ContractSent = false
         │  └─ ContractConfirmed = false
         │
         ├─ FOR EACH marker slot (up to MaxNumberOfMarkers):
         │  └─ Call Workflow 302 (Send invite to marker)
         │
         └─ Update Status to 30 ("Confirming Markers Capacity")
         ↓
[✅ Workflow 301a Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference status mapping |
| 2 | Get MaxMarkers Setting | Fetch marker limit (typically 2-3) |
| 3 | Query Ready Cohorts | Find cohorts with status=10 |
| 4 | Loop Each Cohort | Process each ready cohort |
| 5 | Create CohortMarker | Generate marker slots |
| 6 | Call WF 302 | Send capacity confirmation email |
| 7 | Update Status | Set status to 30 |

## 📋 CohortMarker Records

**Created For:** Each marker needed (up to MaxNumberOfMarkersPerCohort)

**Initial Values:**
- `Cohort` → Link to parent Cohort
- `CapacityConfirmed` → false (awaiting response)
- `ContractSent` → false
- `ContractConfirmed` → false

## 💌 Invitation Details

**What is "Capacity Confirmation" for Markers?**
- About evaluation capacity and resources
- Marker must confirm: "I can evaluate this cohort"
- Different from coaches (who mentor) and trainers (who teach)

**Sent Via:** Workflow 302 (child flow)
- One invitation per marker slot

## 📤 SharePoint Updates

**Table: Cohorts**
- `Status` → 10 → **30** ("Confirming Markers Capacity")

**Table: CohortMarker** (new records created)
- `Cohort` → Link to cohort
- `CapacityConfirmed` → false
- `ContractSent` → false
- `ContractConfirmed` → false

## 🔗 Next Workflow
→ **Workflow 302** ("Send Capacity Confirmation to Marker and Wait for Confirmation")
- Sends individual invitations
- Waits for marker responses

## ⚙️ Configuration

**Setting:** `MaxNumberOfMarkersPerCohort`
- Determines how many marker positions to create
- Typically 2-3 markers per cohort
- Retrieved from SharePoint Settings

## ⏰ Status Transition

```
Status 10: "All Cohorts Units Created" (Phase 1 complete)
    ↓ (WF 301a runs PARALLEL with WF 201a)
Status 30: "Confirming Markers Capacity"
    ↓ (WF 303 verifies confirmations)
Status 31: "All Markers Capacity Confirmed"
```

## 🎓 What is a Marker?

**Marker** = Evaluator/Reviewer
- Responsible for evaluating student performance
- Provides feedback and grading
- Validates learning outcomes
- Essential for program completion

## ⚠️ Key Differences: Markers vs Coaches vs Trainers

| Aspect | Trainers | Coaches | Markers |
|--------|----------|---------|---------|
| **Role** | Teach classes | Mentor students | Evaluate work |
| **Confirmation** | Date availability | General capacity | Evaluation capacity |
| **Status Field** | DatesConfirmed | CapacityConfirmed | CapacityConfirmed |
| **Execution** | Sequential | Parallel with markers | Parallel with coaches |

---

**Status:** ✅ Documented | **Updated:** March 2026
