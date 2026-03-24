# 202 - Send Capacity Confirmation to Coach and Wait for Confirmation

## 📝 Executive Summary
Sends capacity confirmation request to coach and awaits response. Processes responses (yes = confirmed, no = rejected). Updates CohortCoach records based on coach decision. Core workflow for coach recruitment.

## ⚙️ Trigger
- **Type:** Manual (called by Workflow 201a/201b)
- **Input:** CohortCoachId
- **Initiator:** Workflow 201a or 201b (rejection handler)

## 🔄 Execution Flow

```
[Receive CohortCoachId from caller]
         ↓
[1. Fetch CohortCoach and Coach details]
         ↓
[2. Fetch Coach email address]
         ↓
[3. Fetch email settings (subject, message template)]
         ↓
[4. Personalize message]
         ├─ {CoachName} → Coach full name
         ├─ {ProgramName} → Program name
         └─ {CohortStartDate} → Formatted date
         ↓
[5. Build and send confirmation email]
         ├─ To: Coach email
         ├─ Subject: from Settings
         ├─ Body: Personalized message
         └─ Options: Yes | No
         ↓
[6. Wait for coach response...]
         ├─ YES → Confirm capacity ✓
         │        ├─ Set CapacityConfirmed = true
         │        └─ Record confirmation timestamp
         │
         └─ NO → Reject capacity ✗
                  ├─ Set CapacityConfirmed = false
                  ├─ Clear CoachId
                  └─ Create CoachConfirmationRejected record
         ↓
[✅ Workflow 202 Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Get CohortCoach | Fetch coach assignment |
| 2 | Get Coach Details | Retrieve coach info |
| 3 | Fetch Email Settings | Load subject/message templates |
| 4 | Personalize Message | Replace placeholders |
| 5 | Build Email | Create confirmation request |
| 6 | Send Email | Send to coach |
| 7 | Wait for Response | Listen for yes/no |
| 8 | Process Response | Update records |

## 💌 Email Details

**Sent To:** Coach's registered email address

**Subject:** From Settings (`SETTING_CoachCapacityConfirmation_EmailSubject`)

**Message:** From Settings (`SETTING_CoachCapacityConfirmation_EmailMessage`)

**Placeholders Replaced:**
- `{CoachName}` → Coach full name
- `{ProgramName}` → Program name
- `{CohortStartDate}` → Date in dd/MM/yyyy format

**Action Options:**
- ✅ **Yes** (Coach can mentor this cohort)
- ❌ **No** (Coach cannot participate)

## 📤 SharePoint Updates

**If YES (Confirmed):**
- `CapacityConfirmed` → true
- `CapacityConfirmedDate` → Current timestamp

**If NO (Rejected):**
- `CapacityConfirmed` → false
- `CoachId` → null (cleared)
- Create record in `CoachConfirmationRejected` list
  - Triggers Workflow 201b (resend)

## 🔗 Next Workflow
→ **Workflow 203** ("Check For Coaches Capacity Confirmed")
- Verifies all coaches have confirmed
- Triggers contract sending

## ⚠️ Critical Points

| Scenario | Handling |
|----------|----------|
| Coach confirms | Record persists, moves to contract phase |
| Coach rejects | Triggers WF 201b (resend) up to 3 times |
| No response | Remains pending until manual action |
| Multiple coaches | Each processed independently |

## 🔀 Comparison: Coaches vs Trainers

| Aspect | Trainers (103a) | Coaches (202) |
|--------|---|---|
| **Confirmation Type** | Specific date availability | General capacity |
| **Field Updated** | DatesConfirmed | CapacityConfirmed |
| **Email Options** | Accept / Alt Date / Reject | Yes / No |
| **Next Step** | Contract sending (106) | Contract sending (204) |
| **Pattern** | Identical structure, different content |

---

**Status:** ✅ Documented | **Updated:** March 2026
