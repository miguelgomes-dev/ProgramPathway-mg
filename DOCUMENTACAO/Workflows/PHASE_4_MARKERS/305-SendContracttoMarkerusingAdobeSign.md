# 305 - Send Contract to Marker Using Adobe Sign

## 📝 Executive Summary
Sends marker evaluation contract via Adobe Sign. Creates temporary upload, generates Adobe Sign agreement with dynamic recipient list, configures reminders, and waits for signature. Critical integration point with Adobe Sign API and webhook system.

## ⚙️ Trigger
- **Type:** Manual (called by Workflow 304)
- **Input:** CohortMarkerId, CohortMarkerContractId
- **Initiator:** Workflow 304 (contract orchestrator)

## 🔄 Execution Flow

```
[Receive CohortMarkerId and ContractId]
         ↓
[1. Fetch CohortMarker details]
         ├─ Marker email, name
         ├─ Cohort info
         └─ Contract settings
         ↓
[2. Fetch template Word document]
         ├─ File name from Settings
         └─ From SharePoint Document Library
         ↓
[3. Convert document to PDF]
         ├─ Use Content Conversion
         └─ Creates base64 PDF content
         ↓
[4. Create temporary upload in Adobe Sign]
         ├─ Upload PDF to Adobe Sign transient storage
         ├─ Receive transient document ID
         └─ Document valid for 7 days
         ↓
[5. Prepare agreement data]
         ├─ Recipient: Marker email
         ├─ CC: Program admin (optional)
         ├─ Message: Personalized request
         └─ Merge field values (standard)
         ↓
[6. Create Adobe Sign Agreement]
         ├─ Use transient document ID
         ├─ Set agreement name: "{MarkerName} - {ProgramName} - {CohortDate}"
         ├─ Configure signing order (Marker first)
         ├─ Set email delivery method
         ├─ Enable reminder emails
         └─ Receive Agreement ID + signing link
         ↓
[7. Configure reminder schedule]
         ├─ First: 3 days after signing request
         ├─ Second: 7 days (final reminder)
         └─ Frequency: Daily during reminder period
         ↓
[8. Update CohortMarkerContract record]
         ├─ AdobeSignAgreementId = returned ID
         ├─ SigningLink = signing URL
         ├─ ContractStatus = "PendingSignature"
         └─ ContractSentDate = Now
         ↓
[9. Send signing link to Marker]
         ├─ Include personalized message
         ├─ Direct link to sign
         └─ Deadline: 10 days
         ↓
[✅ Workflow 305 Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Get CohortMarker Details | Fetch recipient info |
| 2 | Fetch Contract Template | Retrieve Word document |
| 3 | Convert to PDF | Word → PDF |
| 4 | Create Transient Upload | Temporary file in Adobe Sign |
| 5 | Prepare Agreement Data | Build signing request |
| 6 | Create Agreement | Adobe Sign agreement |
| 7. ConfigureReminders | Automatic follow-ups |
| 8 | Update Contract Record | Store agreement ID and URL |
| 9 | Send Signing Link | Marker receives instructions |

## 📋 Adobe Sign Configuration

**Agreement Name:** `{MarkerName} - {ProgramName} - {CohortStartDate}`

**Recipients:**
- **Primary:** Marker email (must sign)
- **CC:** Admin email (optional, notification only)

**Signing Method:**
- *Email delivery* (marker receives link via email)
- *E-sign required* (legally binding digital signature)
- *Signing order:* Marker only (marker signs, admin reviews)

**Reminders:**
- **First Reminder:** 3 days after sending
- **Second Reminder:** 7 days (final notice)
- **Frequency:** Once per day during reminder period

**Expiration:**
- Agreement valid for **10 days**
- After 10 days: Contract expires, can resend if needed

## 📤 SharePoint Updates

**CohortMarkerContract Record Updated:**
- `AdobeSignAgreementId` → Unique ID from Adobe Sign
- `SigningLink` → Direct URL for marker to sign
- `ContractStatus` → "PendingSignature"
- `ContractSentDate` → Current timestamp
- `ContractDeadline` → 10 days from today

## 🔗 Next Workflow
→ **Workflow 306** ("Check For Markers Adobe Sign Agreement")
- Daily verification at 6:00 AM
- Monitors signing status
- Downloads signed PDF when complete

## 🪝 Webhook Integration

When marker signs the agreement in Adobe Sign:
- Webhook fires automatically
- Sends signed PDF to SharePoint
- Stores in Contract Documents library
- Triggers status updates

## ⚠️ Critical Points

| Aspect | Details |
|--------|---------|
| **Transient Upload** | PDF valid in Adobe Sign for 7 days |
| **Agreement ID** | Essential for monitoring in WF 306 |
| **Signing Link** | Sent to marker via email |
| **Reminders** | Automatic in Adobe Sign, no Power Automate needed |
| **PDF Download** | Webhook handles automatic PDF retrieval |
| **Expiration** | 10-day window to sign (resend if needed) |

## 🔀 Comparison: Coaches vs Markers

| Aspect | Coaches (205) | Markers (305) |
|--------|---|---|
| **Template** | Coach contract template | Marker contract template |
| **Recipient** | Coach email | Marker email |
| **Agreement Name** | Coach + Program + Date | Marker + Program + Date |
| **Reminders** | 3 days + 7 days | 3 days + 7 days |
| **Next Workflow** | WF 206 | WF 306 |
| **Pattern** | Identical implementation |

---

**Status:** ✅ Documented | **Updated:** March 2026
