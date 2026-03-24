# 201a - Send Capacity Confirmation to Coaches

## 📝 Executive Summary
Initiates coach recruitment for cohorts. Runs daily at 3:45 AM GMT. Creates CohortCoach records for available coaches (based on MaxNumberOfCoachesPerCohort setting) and sends capacity confirmation invitations. Marks beginning of coach coordination phase.

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 03:45 AM GMT
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 3:45 AM]
         ↓
[Load cohort status reference]
         ↓
[Fetch MaxNumberOfCoachesPerCohort setting]
         ↓
[Query: Find cohorts with Status=10 ("All Cohorts Units Created")]
         ↓
[FOR EACH ready cohort:]
         ├─ Update Status → 20 ("Confirming Coaches Capacity")
         │
         ├─ FOR EACH coach slot (up to MaxNumberOfCoaches):
         │  ├─ Create CohortCoach record
         │  │  ├─ CapacityConfirmed = false
         │  │  ├─ ContractSent = false
         │  │  └─ ContractConfirmed = false
         │  │
         │  └─ Call Workflow 202 (Send invite to coach)
         │
         └─ Update Status to 20 (now confirming)
         ↓
[✅ Workflow 201a Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference status mapping |
| 2 | Get MaxCoaches Setting | Fetch coach limit (typically 2-3) |
| 3 | Query Ready Cohorts | Find cohorts with status=10 |
| 4 | Loop Each Cohort | Process each ready cohort |
| 5 | Create CohortCoach | Generate coach slots |
| 6 | Call WF 202 | Send capacity confirmation email |
| 7 | Update Status | Set status to 20 |

## 📋 CohortCoach Records

**Created For:** Each coach needed (up to MaxNumberOfCoachesPerCohort)

**Initial Values:**
- `Cohort` → Link to parent Cohort
- `CapacityConfirmed` → false (awaiting response)
- `ContractSent` → false
- `ContractConfirmed` → false

## 💌 Invitation Details

**What is "Capacity Confirmation"?**
- Not about specific dates (like trainers)
- About general availability and resources to mentor cohort
- Coach must confirm: "I have capacity to coach this cohort"

**Sent Via:** Workflow 202 (child flow)
- One invitation per coach slot

## 📤 SharePoint Updates

**Table: Cohorts**
- `Status` → 10 → **20** ("Confirming Coaches Capacity")

**Table: CohortCoach** (new records created)
- `Cohort` → Link to cohort
- `CapacityConfirmed` → false
- `ContractSent` → false
- `ContractConfirmed` → false

## 🔗 Next Workflow
→ **Workflow 202** ("Send Capacity Confirmation to Coach and Wait for Confirmation")
- Sends individual invitations
- Waits for coach responses

## ⚙️ Configuration

**Setting:** `MaxNumberOfCoachesPerCohort`
- Determines how many coach positions to create
- Typically 2-3 coaches per cohort
- Retrieved from SharePoint Settings

## ⏰ Status Transition

```
Status 10: "All Cohorts Units Created" (Phase 1 complete)
    ↓ (WF 201a runs)
Status 20: "Confirming Coaches Capacity" (Phase 2 begins)
    ↓ (WF 203 verifies confirmations)
Status 21: "All Coaches Capacity Confirmed"
```

## ⚠️ Key Differences: Coaches vs Trainers

| Aspect | Trainers (Phase 1) | Coaches (Phase 2) |
|--------|-------------------|-------------------|
| **Confirmation Type** | Specific date availability | General capacity |
| **Status Field** | DatesConfirmed | CapacityConfirmed |
| **Parallel Execution** | Sequential after Phase 1 | Parallel with Markers |
| **Number Per Cohort** | Typically 1-2 | Typically 2-3 |
| **Contract Template** | Trainer contract | Coach contract |

---

**Status:** ✅ Documented | **Updated:** March 2026
