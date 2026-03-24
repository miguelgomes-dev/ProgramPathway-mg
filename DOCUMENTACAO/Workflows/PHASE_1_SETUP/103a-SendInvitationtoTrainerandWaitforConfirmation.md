# 103a - Send Invitation to Trainer and Wait for Confirmation

## 📝 Executive Summary
Sends an email invitation to a trainer with date options and awaits response. Processes responses (accept, reject, or choose alternative date) and updates the CohortSchedule accordingly. This is the core workflow for trainer coordination.

## ⚙️ Trigger
- **Type:** Manual (called by Workflow 102a)
- **Input:** CohortScheduleId
- **Initiator:** Workflow 102a or rejection handler (102b)

## 🔄 Execution Flow

```
[Receive CohortScheduleId from caller]
         ↓
[1. Fetch CohortSchedule details]
         ↓
[2. Calculate viable dates for trainer]
         ├─ Exclude public holidays
         ├─ Exclude trainer unavailable dates
         └─ Build date options
         ↓
[3. Find available trainer (not rejected, not allocated)]
         ↓
[4. Assign trainer to schedule with initial dates]
         ↓
[5. Build and send invitation email]
         ├─ To: Trainer email
         ├─ Options: Accept | Choose Alt Date | Reject
         └─ Include cohort details
         ↓
[6. Wait for trainer response...]
         ├─ ACCEPT → Mark Confirmed ✓
         ├─ REJECT → Clear trainer, mark unconfirmed
         └─ ALT DATE → Confirm with chosen date
         ↓
[✅ Workflow 103a Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Get CohortSchedule | Fetch schedule details |
| 2 | Calculate Viable Dates | Determine available dates for trainer |
| 3 | Find Available Trainer | Locate trainer not yet assigned |
| 4 | Assign Trainer | Allocate trainer to schedule |
| 5 | Build Invitation Email | Create email with response options |
| 6 | Send Email | Send invitation to trainer |
| 7 | Wait for Response | Listen for email response |
| 8 | Process Response | Handle accept/reject/alternate date |

## 💌 Email Details

**Sent To:** Trainer's registered email address

**Content:** 
- Cohort details (program, dates, location)
- Date options for the trainer to choose
- Three clear action buttons:
  - ✅ **Accept** (confirms current dates)
  - 📅 **Alternative Date** (list of options)
  - ❌ **Reject** (declines invitation)

## 📤 SharePoint Updates

**Table: CohortSchedules**
- `Trainer` → Assigned trainer ID
- `Confirmed` → true (if accepted), false (if rejected)
- `ConfirmedDate` → Date/time when confirmed
- `Status` → User confirmation pending

## 🔗 Next Workflow
→ **Workflow 104** ("Check For Trainers Dates Confirmed")
- Verifies all trainers have confirmed

## ⚠️ Critical Points

| Scenario | Handling |
|----------|----------|
| No available trainers | Sends error alert, pauses process |
| Trainer rejects | Triggered: WF 102b (resend via handler) |
| Trainer doesn't respond | Remains pending until manual action or timeout |
| Alternative date chosen | Confirms with trainer-selected date |

---

**Status:** ✅ Documented | **Updated:** March 2026
