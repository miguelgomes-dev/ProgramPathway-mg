# 205 - Send Contract to Coach Using Adobe Sign

## 📝 Executive Summary
Core contract workflow for coaches: generates personalized contracts, uploads to SharePoint, and sends to Adobe Sign for digital signature. Includes automatic daily reminders until signed. Supports two template variants (normal and day rate).

## ⚙️ Trigger
- **Type:** Manual (called by Workflow 204)
- **Input Parameters:** 
  - CohortId
  - CoachId
- **Initiator:** Workflow 204 (for each unique coach)

## 🔄 Execution Flow

```
[Receive CohortId & CoachId from WF 204]
         ↓
[PHASE 1: Fetch Data]
├─ Get Cohort details
├─ Get Coach details (including email)
├─ Get Program (for Adobe Sign account)
└─ Get all Coach assignments
         ↓
[PHASE 2: Build Contract Content]
├─ Extract services/duties
├─ Extract payment amount
└─ Sum total payment
         ↓
[PHASE 3: Select Template]
├─ Check: Is coach DayRate?
├─ YES → Use DayRate template
└─ NO → Use Standard template
         ↓
[PHASE 4: Populate & Save Contract]
├─ Write coach name, services, payment
├─ Create folder: /Contracts/{Cohort}/Coaches/
├─ Save Word document in SharePoint
└─ Get shareable link
         ↓
[PHASE 5: Adobe Sign Integration ⭐]
├─ Upload document to Adobe (transient)
├─ Fetch signing message from Settings
├─ Parse coach email(s) → Signer + CCs
├─ Create Agreement (Adobe Sign API)
├─ Agreement ID returned
└─ Daily reminders ACTIVE until signed
         ↓
[PHASE 6: Update SharePoint]
├─ Set ContractSent = true
├─ Set ContractSentDate = now
├─ Set ContractSentUrl = link
├─ Set AdobeSignAgreementId = ID
└─ Update all coach assignments
         ↓
[✅ Workflow 205 Complete]
```

## 📊 Main Actions (6 Phases)

| Phase | Actions | Purpose |
|-------|---------|---------|
| **1: Data** | Get Cohort, Coach, Program, Assignments | Gather all information |
| **2: Build** | Extract services and payment | Prepare contract content |
| **3: Template** | Select template (Normal or DayRate) | Choose document variant |
| **4: Save** | Populate Word, upload to SharePoint | Create contract file |
| **5: Adobe** | Upload, create agreement, set reminders | Send for digital signature |
| **6: Track** | Update all SharePoint fields | Record agreement details |

## 📄 Contract Templates

| Template | Condition | Location |
|----------|-----------|----------|
| **Standard** | `Coach.IsDayRate = false` (default) | Templates/ |
| **Day Rate** | `Coach.IsDayRate = true` | Templates/ |

**Content in Contracts:**
- Coach name (personalized)
- Services/duties
- Payment amount (£ GBP formatted)
- Cohort dates

## 💌 Email & Signing Configuration

**Coach Email Field:** `coach@email.com;cc1@email.com;cc2@email.com`
- **Split by semicolon:**
  - First email → SIGNER (must sign)
  - Remaining → CCs (view only)

**Adobe Sign Settings:**
- **Signature Type:** ESIGN (electronic signature)
- **State:** IN_PROCESS (starts immediately)
- **Reminders:** DAILY_UNTIL_SIGNED (automatic)

## 📤 SharePoint Updates

**Table: CohortCoaches** (for each assignment)
- `ContractSent` → true
- `ContractSentDate` → Current timestamp
- `ContractSentUrl` → Shareable read-only link
- `AdobeSignAgreementId` → **CRITICAL** (for tracking)
- `ContractFolderPath` → /Contracts/{Cohort}/Coaches/

## 🔗 Next Workflow
→ **Workflow 206** ("Check For Coaches Adobe Sign Agreement")
- Monitors Adobe Sign process
- Validates when signed

## ⚠️ Critical Adobe Sign Details

| Aspect | Detail |
|--------|--------|
| **Agreement ID** | Unique identifier for tracking in SharePoint |
| **Document Upload** | Temporary (transientDocumentId) then saved |
| **Reminders** | Automatic daily until signed (no manual intervention) |
| **Authentication** | NONE (coach clicks link, signs) |
| **Template Variants** | Supports DayRate option for flexible billing |

---

**Status:** ✅ Documented | **Updated:** March 2026
