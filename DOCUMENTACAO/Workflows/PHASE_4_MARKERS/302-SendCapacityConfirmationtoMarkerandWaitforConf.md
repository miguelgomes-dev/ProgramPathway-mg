# 302 - Send Capacity Confirmation to Marker and Wait for Confirmation

## 📝 Executive Summary
Sends capacity confirmation request to marker and awaits response. Processes responses (yes = confirmed, no = rejected). Updates CohortMarker records based on marker decision. Core workflow for marker recruitment.

## ⚙️ Trigger
- **Type:** Manual (called by Workflow 301a/301b)
- **Input:** CohortMarkerId
- **Initiator:** Workflow 301a or 301b (rejection handler)

## 🔄 Execution Flow

```
[Receive CohortMarkerId from caller]
         ↓
[1. Fetch CohortMarker and Marker details]
         ↓
[2. Fetch Marker email address]
         ↓
[3. Fetch email settings (subject, message template)]
         ↓
[4. Personalize message]
         ├─ {MarkerName} → Marker full name
         ├─ {ProgramName} → Program name
         └─ {CohortStartDate} → Formatted date
         ↓
[5. Build and send confirmation email]
         ├─ To: Marker email
         ├─ Subject: from Settings
         ├─ Body: Personalized message
         └─ Options: Yes | No
         ↓
[6. Wait for marker response...]
         ├─ YES → Confirm capacity ✓
         │        ├─ Set CapacityConfirmed = true
         │        └─ Record confirmation timestamp
         │
         └─ NO → Reject capacity ✗
                  ├─ Set CapacityConfirmed = false
                  ├─ Clear MarkerId
                  └─ Create MarkerConfirmationRejected record
         ↓
[✅ Workflow 302 Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Get CohortMarker | Fetch marker assignment |
| 2 | Get Marker Details | Retrieve marker info |
| 3 | Fetch Email Settings | Load subject/message templates |
| 4 | Personalize Message | Replace placeholders |
| 5 | Build Email | Create confirmation request |
| 6 | Send Email | Send to marker |
| 7 | Wait for Response | Listen for yes/no |
| 8 | Process Response | Update records |

## 💌 Email Details

**Sent To:** Marker's registered email address

**Subject:** From Settings (`SETTING_MarkerCapacityConfirmation_EmailSubject`)

**Message:** From Settings (`SETTING_MarkerCapacityConfirmation_EmailMessage`)

**Placeholders Replaced:**
- `{MarkerName}` → Marker full name
- `{ProgramName}` → Program name
- `{CohortStartDate}` → Date in dd/MM/yyyy format

**Action Options:**
- ✅ **Yes** (Marker can evaluate this cohort)
- ❌ **No** (Marker cannot participate)

## 📤 SharePoint Updates

**If YES (Confirmed):**
- `CapacityConfirmed` → true
- `CapacityConfirmedDate` → Current timestamp

**If NO (Rejected):**
- `CapacityConfirmed` → false
- `MarkerId` → null (cleared)
- Create record in `MarkerConfirmationRejected` list
  - Triggers Workflow 301b (resend)

## 🔗 Next Workflow
→ **Workflow 303** ("Check For Markers Capacity Confirmed")
- Verifies all markers have confirmed
- Triggers contract sending

## ⚠️ Critical Points

| Scenario | Handling |
|----------|----------|
| Marker confirms | Record persists, moves to contract phase |
| Marker rejects | Triggers WF 301b (resend) up to 3 times |
| No response | Remains pending until manual action |
| Multiple markers | Each processed independently |

## 🔀 Comparison: Markers vs Coaches

| Aspect | Coaches (202) | Markers (302) |
|--------|---|---|
| **Confirmation Type** | General capacity to mentor | Evaluation capacity |
| **Field Updated** | CapacityConfirmed | CapacityConfirmed |
| **Email Options** | Yes / No | Yes / No |
| **Next Step** | Contract sending (204) | Contract sending (304) |
| **Pattern** | Identical logic |

---

**Status:** ✅ Documented | **Updated:** March 2026
