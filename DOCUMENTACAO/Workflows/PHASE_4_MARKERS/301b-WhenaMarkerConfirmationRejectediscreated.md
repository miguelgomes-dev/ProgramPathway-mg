# 301b - When a Marker Confirmation Rejected Is Created

## 📝 Executive Summary
Monitors marker rejection records. When a marker rejects a capacity confirmation, this workflow triggers and resends the confirmation request via Workflow 302, giving the marker another opportunity to respond.

## ⚙️ Trigger
- **Type:** Recurrence-based (polling)
- **Monitors:** "MarkerConfirmationRejected" table in SharePoint
- **When:** New rejection record created
- **Polling Frequency:** Every 1 minute

## 🔄 Execution Flow

```
[Marker rejects capacity confirmation]
         ↓
[MarkerConfirmationRejected record created]
         ↓
[Monitor detects new rejection (1-min polling)]
         ↓
[Extract CohortMarker ID from rejection]
         ↓
[Call Workflow 302: Resend Confirmation to Marker]
         ↓
[Terminate Successfully]
         ↓
[✅ Workflow 301b Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Extract CohortMarker ID | Get rejected marker details |
| 2 | Call Workflow 302 | Resend capacity confirmation |
| 3 | Terminate Successfully | Mark as complete |

## 📤 SharePoint Updates
**None directly.** (Workflow 302 handles updates)

## 🔗 Next Workflow
→ **Workflow 302** ("Send Capacity Confirmation to Marker and Wait for Confirmation")
- Resends confirmation request to marker

## ⚠️ Important Notes

| Feature | Behavior |
|---------|----------|
| **Rejection Handler** | Automatically resends when rejected |
| **Retry Logic** | Can trigger multiple times (up to 3 rejections) |
| **Purpose** | Prevents loss of marker; gives multiple chances |
| **Pattern** | Identical to Coach (201b) and Trainer (102b) |

---

**Status:** ✅ Documented | **Updated:** March 2026
