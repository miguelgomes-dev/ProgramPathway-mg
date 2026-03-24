# 201b - When a Coach Confirmation Rejected Is Created

## 📝 Executive Summary
Monitors coach rejection records. When a coach rejects a capacity confirmation, this workflow triggers and resends the confirmation request via Workflow 202, giving the coach another opportunity to respond.

## ⚙️ Trigger
- **Type:** Recurrence-based (polling)
- **Monitors:** "CoachConfirmationRejected" table in SharePoint
- **When:** New rejection record created
- **Polling Frequency:** Every 1 minute
- **Site:** motasystems.sharepoint.com/sites/PotentialRealised_Dev

## 🔄 Execution Flow

```
[Coach rejects capacity confirmation]
         ↓
[CoachConfirmationRejected record created]
         ↓
[Monitor detects new rejection (1-min polling)]
         ↓
[Extract CohortCoach ID from rejection]
         ↓
[Call Workflow 202: Resend Confirmation to Coach]
         ↓
[Terminate Successfully]
         ↓
[✅ Workflow 201b Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Extract CohortCoach ID | Get rejected coach details |
| 2 | Call Workflow 202 | Resend capacity confirmation |
| 3 | Terminate Successfully | Mark as complete |

## 📤 SharePoint Updates
**None directly.** (Workflow 202 handles updates)

## 🔗 Next Workflow
→ **Workflow 202** ("Send Capacity Confirmation to Coach and Wait for Confirmation")
- Resends confirmation request to coach

## ⚠️ Important Notes

| Feature | Behavior |
|---------|----------|
| **Rejection Handler** | Automatically resends when rejected |
| **Retry Logic** | Can trigger multiple times (up to 3 rejections) |
| **Purpose** | Prevents loss of coach; gives multiple chances |
| **Difference from Trainers** | Same pattern, applies to capacity (not dates) |

---

**Status:** ✅ Documented | **Updated:** March 2026
