# 306 - Check For Markers Adobe Sign Agreement

## 📝 Executive Summary
Daily verification at 6:00 AM that markers have signed their Adobe Sign contracts. Checks agreement status for each pending contract. Downloads signed PDF when signature received and stores in SharePoint. Does not yet update overall cohort status (that happens in Workflow 307).

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 06:00 AM GMT
- **Purpose:** Monitor Adobe Sign signatures

## 🔄 Execution Flow

```
[Daily trigger at 6:00 AM]
         ↓
[Query: Find CohortMarkerContracts with Status="PendingSignature"]
         ↓
[FOR EACH pending contract:]
         ├─ Get AdobeSignAgreementId
         ├─ Check Adobe Sign agreement status
         └─ Is it signed?
         ↓
[IF SIGNED:]
         ├─ Download signed PDF from Adobe Sign
         ├─ Upload to SharePoint (Contracts library)
         ├─ Update ContractStatus = "Signed"
         ├─ Record SignedDate & SignedByEmail
         └─ Continue to next contract
         │
[IF NOT SIGNED:]
         ├─ Check if past deadline (10 days)
         │  ├─ YES → Mark as "Expired"
         │  └─ NO → Leave as "PendingSignature"
         └─ Continue to next contract
         ↓
[Check complete]
         ↓
[✅ Workflow 306 Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Query Pending Signatures | Find contracts awaiting signature |
| 2 | Loop Each Contract | Process all markers' contracts |
| 3 | Get Agreement ID | Retrieve Adobe Sign ID |
| 4 | Check Status | Query Adobe Sign API |
| 5 | Download PDF (if signed) | Retrieve signed document |
| 6 | Upload to SharePoint | Store signed contract |
| 7 | Update Record | Mark as signed |

## ✅ Signature Detection

```
FOR EACH CohortMarkerContract with Status="PendingSignature":
  
  response = GET Adobe Sign agreement status using AdobeSignAgreementId
  
  IF response.Status == "SIGNED":
    → Get PDF from Adobe Sign
    → Upload to SharePoint Contracts library
    → ContractStatus = "Signed"
    → SignedDate = Date from response
    → SignedByEmail = Marker email
  
  ELSE IF response.Status == "EXPIRED":
    → ContractStatus = "Expired"
    → Manual resend may be needed
  
  ELSE:
    → Leave as "PendingSignature"
    → Wait for next daily check
```

## 📤 SharePoint Updates

**CohortMarkerContract Record Updated (when signed):**
- `ContractStatus` → "Signed"
- `SignedDate` → Date signature received
- `SignedByName` → Marker name
- `SignedByEmail` → Marker email address
- `PDFDocumentLocation` → SharePoint URL

**PDF Stored At:**
- Location: Contract Documents library
- Name: `{CohortName}_{MarkerName}_Contract_Signed.pdf`
- Accessible by: Admin + Marker + all roles with contract access

## ⏰ Verification Timeline

```
5:30 AM   → WF 304: Send marker contracts (status 32)
            ├─ Each marker gets Adobe Sign request
            └─ Countdown starts (10 days)
            
6:00 AM   → WF 306: Check for signatures
            ├─ Query Adobe Sign API
            ├─ Download signed PDFs
            └─ Update records
            
Next day: Check again for remaining signatures
```

## 🔗 Next Workflow
→ **Workflow 307** ("Check For Markers Contracts Confirmed")
- Daily at 6:30 AM
- Verifies ALL markers have signed
- Updates cohort status to "All Contracts Signed"

## ⚠️ Critical Points

| Scenario | Behavior |
|----------|----------|
| Marker signs day 1 | PDF downloaded, status="Signed" |
| Marker signs day 5 | PDF downloaded, status="Signed" |
| Marker never signs (10+ days) | Status="Expired", manual resend needed |
| PDF already exists | Overwrites previous version |
| Multiple markers | Each processed independently |

## 🔀 Comparison: Coaches vs Markers

| Aspect | Coaches (206) | Markers (306) |
|--------|---|---|
| **Trigger Time** | 6:00 AM | 6:00 AM |
| **Status Check** | Adobe Sign API query | Adobe Sign API query |
| **PDF Download** | When signed | When signed |
| **Storage Location** | Contracts library | Contracts library |
| **Next Workflow** | WF 207 | WF 307 |
| **Pattern** | Identical |

---

**Status:** ✅ Documented | **Updated:** March 2026
