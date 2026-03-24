# 102a - When a Cohort Schedule Is Created

## 📝 Executive Summary
Triggers every minute to detect new CohortSchedule records. Acts as a routing orchestrator: if the schedule is a deadline reminder, it calculates and updates reminder dates; otherwise, it sends invitation to the trainer for approval.

## ⚙️ Trigger
- **Type:** Recurrence-based (polling)
- **Monitors:** "CohortSchedules" table in SharePoint
- **When:** New item created
- **Polling Frequency:** Every 1 minute
- **Site:** motasystems.sharepoint.com/sites/PotentialRealised_Dev

## 🔄 Execution Flow

```
[New CohortSchedule created]
         ↓
[Check: IsDeadlineReminder?]
         ├─ YES → Call WF 103b (Calculate Reminder Dates)
         │         └─ Updates deadline dates in system
         │
         └─ NO → Call WF 103a (Send Trainer Invitation)
                  └─ Sends invitation email to trainer
         ↓
[✅ Workflow 102a COMPLETE]
```

## 🔀 Conditional Logic

| Condition | Action | Next Workflow |
|-----------|--------|---------------|
| `IsDeadlineReminder = TRUE` | Calculate and update deadline reminder dates | **WF 103b** |
| `IsDeadlineReminder = FALSE` | Send approval invitation to trainer | **WF 103a** |

## 📊 Key Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Evaluate IsDeadlineReminder | Check if this is a reminder schedule |
| 2a | Call Workflow 103b | (If reminder) Calculate dates |
| 2b | Call Workflow 103a | (If normal) Send trainer invitation |
| 3 | Terminate Successfully | Mark workflow as complete |

## 📤 SharePoint Updates
**None directly.** (Child workflows handle updates)

## 🔗 Next Workflows (Conditional Branching)

- **If Deadline Reminder:** → **Workflow 103b** ("Calculate and Update Dates for Deadline Reminder")
- **If Normal Schedule:** → **Workflow 103a** ("Send Invitation to Trainer and Wait for Confirmation")

## ⚠️ Critical Points

| Issue | Impact |
|-------|--------|
| Schedule marked as reminder but trainer still needs invite | Workflow branches correctly based on flag |
| Multiple schedules created simultaneously | Each triggers separately (1-min polling) |

---

**Status:** ✅ Documented | **Updated:** March 2026
