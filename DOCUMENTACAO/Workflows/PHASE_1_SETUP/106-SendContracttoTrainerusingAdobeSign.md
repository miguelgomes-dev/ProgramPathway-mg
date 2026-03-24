# 106 - Send Contract to Trainer Using Adobe Sign

## 📝 Executive Summary
Core contract workflow: generates personalized contracts, uploads to SharePoint, and sends to Adobe Sign for digital signature. Includes automatic daily reminders until signed. Supports two template variants (normal and day rate).

## ⚙️ Trigger
- **Type:** Manual (called by Workflow 105)
- **Input Parameters:** 
  - CohortId
  - TrainerId
- **Initiator:** Workflow 105 (for each unique trainer)

## 🔄 Execution Flow

```
[Receive CohortId & TrainerId from WF 105]
         ↓
[PHASE 1: Fetch Data]
├─ Get Cohort details
├─ Get Trainer details (including email)
├─ Get Program (for Adobe Sign account)
└─ Get all CohortSchedules (masterclasses)
         ↓
[PHASE 2: Build Contract Content]
├─ Loop each schedule/masterclass
├─ Extract: title, date, payment amount
└─ Sum total payment
         ↓
[PHASE 3: Select Template]
├─ Check: Is trainer DayRate?
├─ YES → Use DayRate template
└─ NO → Use Standard template
         ↓
[PHASE 4: Populate & Save Contract]
├─ Write trainer name, dates, masterclasses, payment
├─ Create folder: /Contracts/{Cohort}/Trainers/
├─ Save Word document in SharePoint
└─ Get shareable link
         ↓
[PHASE 5: Adobe Sign Integration ⭐]
├─ Upload document to Adobe (transient)
├─ Fetch signing message from Settings
├─ Parse trainer email(s) → Signer + CCs
├─ Create Agreement (Adobe Sign API)
├─ Agreement ID returned
└─ Daily reminders ACTIVE until signed
         ↓
[PHASE 6: Update SharePoint]
├─ Set ContractSent = true
├─ Set ContractSentDate = now
├─ Set ContractSentUrl = link
├─ Set AdobeSignAgreementId = ID (critical for tracking)
└─ Update all schedules
         ↓
[✅ Workflow 106 Complete]
```

## 📊 Main Actions (6 Phases)

| Phase | Actions | Purpose |
|-------|---------|---------|
| **1: Data** | Get Cohort, Trainer, Program, Schedules | Gather all information |
| **2: Build** | Loop schedules, extract payment | Prepare contract content |
| **3: Template** | Select template (Normal or DayRate) | Choose document variant |
| **4: Save** | Populate Word, upload to SharePoint | Create contract file |
| **5: Adobe** | Upload, create agreement, set reminders | Send for digital signature |
| **6: Track** | Update all SharePoint fields | Record agreement details |

## 📄 Contract Templates

| Template | Condition | Location |
|----------|-----------|----------|
| **Standard** | `Trainer.IsDayRate = false` (default) | Templates/ |
| **Day Rate** | `Trainer.IsDayRate = true` | Templates/ |

**Content in Contracts:**
- Trainer name (personalized)
- Cohort dates  
- Masterclass details (titles, dates)
- Total payment (£ GBP formatted)
- All agreed services

## 💌 Email & Signing Configuration

**Trainer Email Field:** `trainer@email.com;cc1@email.com;cc2@email.com`
- **Split by semicolon:**
  - First email → SIGNER (must sign)
  - Remaining → CCs (view only, no signature required)

**Adobe Sign Settings:**
- **Signature Type:** ESIGN (electronic signature)
- **State:** IN_PROCESS (starts immediately)
- **Reminders:** DAILY_UNTIL_SIGNED (automatic)

## 📤 SharePoint Updates

**Table: CohortSchedules** (for each schedule)
- `ContractSent` → true
- `ContractSentDate` → Current timestamp
- `ContractSentUrl` → Shareable read-only link
- `AdobeSignAgreementId` → **CRITICAL** (for tracking)
- `ContractFolderPath` → /Contracts/{Cohort}/Trainers/

**Unchanged:** WeekNumber, IsDeadlineReminder, StartTime, EndTime, Confirmed, ContractConfirmed

## 🔗 Next Workflow
→ **Workflow 107** ("Check For Trainers Adobe Sign Agreement")
- Monitors Adobe Sign process
- Validates when signed

## ⚠️ Critical Adobe Sign Details

| Aspect | Detail |
|--------|--------|
| **Agreement ID** | Unique identifier for tracking in SharePoint |
| **Document Upload** | Temporary (transientDocumentId) then saved |
| **Reminders** | Automatic daily until signed (no manual intervention) |
| **Authentication** | NONE (trainer clicks link, signs) |
| **Template Variants** | Supports DayRate option for flexible billing |

## 🔑 Key Features

✅ **Dynamic Contract Generation** - Customized with actual data
✅ **Automatic Reminders** - Daily until signed
✅ **Email Variants** - Single signer + multiple CCs supported
✅ **Two Templates** - Normal and day rate options
✅ **Full Tracking** - Agreement ID saved for monitoring

---

**Status:** ✅ Documented | **Updated:** March 2026
