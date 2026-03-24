# 107 - Check For Trainers Adobe Sign Agreement

## 📝 Executive Summary
Daily verification workflow that monitors Adobe Sign signature status. When signatures are detected, downloads the signed PDF and updates SharePoint with confirmation details. Runs at 2:00 AM GMT.

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 02:00 AM GMT
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 2:00 AM]
         ↓
[Load cohort status reference]
         ↓
[Query: Find cohorts with Status=5 ("Sending Out Trainers Contracts")]
         ↓
[FOR EACH cohort with pending contracts:]
         ├─ Get Program info (Adobe Sign credentials)
         ├─ Find all unsent/unconfirmed schedules
         │
         └─ FOR EACH unconfirmed schedule:
            ├─ Get Adobe Sign AgreementId (from SharePoint)
            ├─ Call Adobe Sign API: GetAgreementInfo
            ├─ Check: Status == "SIGNED"?
            │
            ├─ YES → Download signed PDF
            │       └─ Save in SharePoint
            │       └─ Update ContractConfirmed = true
            │       └─ Record confirmation timestamp
            │
            └─ NO → Do nothing, wait for next check
         ↓
[Check complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference status mapping |
| 2 | Query Pending Cohorts | Find cohorts with status=5 |
| 3 | Loop Each Cohort | Process pending cohorts |
| 4 | Get Program Info | Retrieve Adobe Sign credentials |
| 5 | Get Unconfirmed Schedules | Find contracts still pending |
| 6 | Loop Each Schedule | Process each trainer contract |
| 7 | Get Agreement Status | Query Adobe Sign API |
| 8 | Download Signed PDF | If signed, get document |
| 9 | Save & Update | Record confirmation |

## 🔐 Adobe Sign Integration

**API Calls:**
- `GetAgreementInfo_V2` - Check signature status
  - Input: `AgreementId` (stored in SharePoint)
  - Returns: Agreement status
  - Checks for: `status == "SIGNED"`

- `GetCombinedDocument_V2` - Download signed PDF
  - Input: `AgreementId`
  - Returns: PDF file with signatures
  - Filename: `{Date}-{TrainerName}-Contract-Signed.pdf`

**Adobe Sign Authentication:**
- **Connection:** shared_adobesign
- **User:** `Program.EmailAccount`
- **Scope:** Organization

## 📤 SharePoint Updates (When Signed)

**Table: CohortSchedules**
- `ContractConfirmed` → true
- `ContractConfirmedDate` → Current timestamp
- `ContractConfirmedUrl` → Shareable link to signed PDF

**File Saved:**
- **Filename:** `{Date}-{TrainerName}-Contract-Signed.pdf`
- **Location:** `/Contracts/{Cohort}/Trainers/` (ContractFolderPath)
- **Permission:** View only, Organization scope

**Unchanged:** ContractSent, ContractSentDate, ContractSentUrl, AgreementId

## 🔗 Next Workflow
→ **Workflow 108** ("Check For Trainers Contracts Confirmed")
- Verifies when ALL trainer contracts are signed
- Updates cohort status when complete

## ⏰ Signature Detection

| Status | Action | Result |
|--------|--------|--------|
| **SIGNED** | Download PDF & confirm | SharePoint updated |
| **PENDING** | Do nothing | Wait for next check |
| **VOIDED/REJECTED** | (detected as not signed) | Remains pending |

## ⚠️ Critical Details

| Aspect | Behavior |
|--------|----------|
| **Agreement ID** | Must be present in SharePoint (set by WF 106) |
| **API Calls** | One per unconfirmed schedule (efficient) |
| **PDF Download** | Only when status = SIGNED |
| **Multiple Trainers** | Each processed independently |
| **Failed Signatures** | Remain pending until signed or manually cleared |

## ⏰ Processing Sequence

```
1:30 AM   → WF 105: Send contracts (WF 106 triggered)
            (Status = 5)
            ↓
2:00 AM   → WF 107: Check for signatures
            (Downloads signed PDFs when ready)
            ↓
3:00 AM   → WF 108: Verify all contracts signed
            (Updates status to 6 if all done)
```

---

**Status:** ✅ Documented | **Updated:** March 2026
