# 206 - Check For Coaches Adobe Sign Agreement

## 📝 Executive Summary
Daily verification workflow that monitors Adobe Sign signature status for coach contracts. When signatures are detected, downloads the signed PDF and updates SharePoint with confirmation details. Runs daily.

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily
- **Time:** 05:00 AM GMT (approx)
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 5:00 AM]
         ↓
[Load cohort status reference]
         ↓
[Query: Find cohorts with Status=22 ("Sending Out Coaches Contracts")]
         ↓
[FOR EACH cohort with pending contracts:]
         ├─ Get Program info (Adobe Sign credentials)
         ├─ Find all unconfirmed contracts
         │
         └─ FOR EACH unconfirmed contract:
            ├─ Get Adobe Sign AgreementId
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
| 2 | Query Pending Cohorts | Find cohorts with status=22 |
| 3 | Loop Each Cohort | Process pending cohorts |
| 4 | Get Program Info | Retrieve Adobe Sign credentials |
| 5 | Get Unconfirmed Contracts | Find signatures still pending |
| 6 | Loop Each Contract | Process each coach contract |
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
  - Filename: `{Date}-{CoachName}-Contract-Signed.pdf`

**Adobe Sign Authentication:**
- **Connection:** shared_adobesign
- **User:** `Program.EmailAccount`
- **Scope:** Organization

## 📤 SharePoint Updates (When Signed)

**Table: CohortCoaches**
- `ContractConfirmed` → true
- `ContractConfirmedDate` → Current timestamp
- `ContractConfirmedUrl` → Shareable link to signed PDF

**File Saved:**
- **Filename:** `{Date}-{CoachName}-Contract-Signed.pdf`
- **Location:** `/Contracts/{Cohort}/Coaches/`
- **Permission:** View only, Organization scope

## 🔗 Next Workflow
→ **Workflow 207** ("Check For Coaches Contracts Confirmed")
- Verifies when ALL coach contracts are signed
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
| **Agreement ID** | Must be present in SharePoint (set by WF 205) |
| **API Calls** | One per unconfirmed coach (efficient) |
| **PDF Download** | Only when status = SIGNED |
| **Multiple Coaches** | Each processed independently |
| **Failed Signatures** | Remain pending until signed or manually cleared |

## ⏰ Processing Sequence

```
4:30 AM   → WF 204: Send contracts (WF 205 triggered)
            (Status = 22)
            ↓
5:00 AM   → WF 206: Check for signatures
            (Downloads signed PDFs when ready)
            ↓
5:30 AM   → WF 207: Verify all contracts signed
            (Updates status if all done)
```

---

**Status:** ✅ Documented | **Updated:** March 2026
