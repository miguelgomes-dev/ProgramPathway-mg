# 102b - When a Trainer Invitation Rejected Is Created

## 📝 Executive Summary
Monitors the "TrainerInvitationRejected" table. When a trainer rejects an invitation, this workflow triggers and resends the invitation, giving the trainer another opportunity to confirm.

## ⚙️ Trigger
- **Type:** Recurrence-based (polling)
- **Monitors:** "TrainerInvitationRejected" table in SharePoint
- **When:** New rejection record created
- **Polling Frequency:** Every 1 minute
- **Site:** motasystems.sharepoint.com/sites/PotentialRealised_Dev

## 🔄 Execution Flow

```
[Trainer rejects invitation]
         ↓
[TrainerInvitationRejected record created]
         ↓
[Monitor detects new rejection (1-min polling)]
         ↓
[Extract CohortSchedule ID from rejection]
         ↓
[Call Workflow 103a: Resend Invitation to Trainer]
         ↓
[Terminate Successfully]
         ↓
[✅ Workflow 102b COMPLETE]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Extract CohortSchedule ID | Get the rejected schedule details |
| 2 | Call Workflow 103a | Resend invitation to trainer |
| 3 | Terminate Successfully | Mark as complete |

## 📤 SharePoint Updates
**None directly.** (Workflow 103a handles updates)

## 🔗 Next Workflow
→ **Workflow 103a** ("Send Invitation to Trainer and Wait for Confirmation")
- Resends invitation to the trainer

## ⚠️ Important Notes

| Feature | Behavior |
|---------|----------|
| **Rejection Handler** | Automatically resends invitation when rejected |
| **Retry Logic** | Can be triggered multiple times (up to 3 rejections) |
| **Purpose** | Prevents invitation loss; gives trainer multiple chances |

---

**Status:** ✅ Documented | **Updated:** March 2026
