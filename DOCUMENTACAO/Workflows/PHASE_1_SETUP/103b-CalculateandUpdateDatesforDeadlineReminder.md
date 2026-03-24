# 103b - Calculate and Update Dates for Deadline Reminder

## 📝 Executive Summary
Calculates reminder deadline dates for a CohortSchedule and updates the SharePoint record. Triggered when a CohortSchedule is marked as a deadline reminder. Performs pure date calculation (no trainer communication).

## ⚙️ Trigger
- **Type:** Manual (called by Workflow 102a)
- **Input:** CohortScheduleId
- **Initiator:** Workflow 102a (when `IsDeadlineReminder = TRUE`)

## 🔄 Execution Flow

```
[Receive CohortScheduleId from Workflow 102a]
         ↓
[1. Fetch CohortSchedule and Cohort details]
         ↓
[2. Extract start date and week number]
         ↓
[3. Calculate reminder deadline]
         ├─ Formula: CohortStartDate + (WeekNumber × 7 days)
         ├─ Adjust to Sunday (end of week)
         └─ Store calculated date
         ↓
[4. Update CohortSchedule with deadline]
         ├─ StartDate = Calculated deadline
         ├─ EndDate = Calculated deadline
         └─ Other fields unchanged
         ↓
[Respond to caller (Done)]
         ↓
[✅ Workflow 103b Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Respond (Initial) | Acknowledge receipt |
| 2 | Get CohortSchedule | Fetch schedule details |
| 3 | Get Cohort | Fetch cohort start date |
| 4 | Extract CohortStartDate | Store start date in variable |
| 5 | Extract WeekNumber | Store week number in variable |
| 6 | Calculate Initial Deadline | Apply formula |
| 7 | Adjust to Sunday | Loop until date = Sunday |
| 8 | Update Deadline Fields | Write to SharePoint |

## 🗓️ Date Calculation Logic

```
Deadline = CohortStartDate + (WeekNumber × 7 days)

Then adjust to SUNDAY (dayOfWeek = 0)

Example:
  CohortStartDate = 2026-04-01 (Wednesday)
  WeekNumber = 2
  Initial calculation = 2026-04-15 (Wednesday)
  Adjusted to Sunday = 2026-04-19 ✓
```

## 📤 SharePoint Updates

**Table: CohortSchedules**
- `StartDate` → Calculated deadline reminder date
- `EndDate` → Same as StartDate (marks end of deadline week)
- **No other fields modified** (status, confirmations, etc. remain unchanged)

## 🔗 Next Workflow
**None.** Workflow terminates after update.

## ⚠️ Key Differences from 103a

| Aspect | 103a (Invitations) | 103b (Reminders) |
|--------|-------------------|-----------------|
| **Purpose** | Communicate with trainer | Calculate dates only |
| **Email Sent?** | ✅ Yes (invitation) | ❌ No |
| **Status Updated?** | ✅ Yes (Inviting Trainers) | ❌ No |
| **Complexity** | High (13+ actions) | Low (8 actions) |
| **Next Step** | Await confirmation (WF 104) | None (terminates) |

---

**Status:** ✅ Documented | **Updated:** March 2026
