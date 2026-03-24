# 101 - When a Cohort Is Created

## 📝 Executive Summary
Automatically triggered when a new Cohort is created in SharePoint. Validates that the Program and Template exist and are active, generates a unique name for the cohort, and creates multiple schedule records (CohortSchedules) based on the template's masterclasses.

## ⚙️ Trigger
- **Type:** Event-based (automatic monitoring)
- **Monitors:** "Cohorts" table in SharePoint
- **When:** New item created
- **Polling Frequency:** Every 1 minute
- **Initiator:** System (automatic, no human action)

## 🔄 Execution Flow

```
[New Cohort created in SharePoint]
         ↓
[1. Extract Cohort ID]
         ↓
[2. Fetch Program data (is it active?)]
         ↓
[3. Fetch Template data (is it active?)]
         ↓
[4. Data Validation]
         ├─ Program active?
         ├─ Template active?
         ├─ Has Masterclasses?
         └─ Public Holidays configured?
         ↓
[5. Status = 1: "Preprocessing Data Validation"]
         ↓
[6. Generate Unique Name]
         ├─ Format: ProgramName + Month + Year
         └─ If duplicate, increment index
         ↓
[7. Update Cohort Title in SharePoint]
         ↓
[8. Status = 2: "Creating Cohort Schedule"]
         ↓
[9. Create CohortSchedules (one per Masterclass)]
         ↓
[✅ Workflow 101 COMPLETE]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Initialize CohortId | Extract ID from trigger |
| 2 | Get Program By Id | Fetch Program data |
| 3 | Get Template By Id | Fetch Template data |
| 4 | Validate Data | Check Program/Template active status |
| 5 | Generate Name | Create unique cohort name |
| 6 | Update Title | Save name to SharePoint |
| 7 | Create Schedules | Generate CohortSchedules for each Masterclass |

## 📤 SharePoint Updates

**Table: Cohorts**
- `Title` → Example: "Business Program March 2025 Cohort 1"
- `StartDate` → Date provided at trigger
- `Status` → Changes: 0 → 1 → 2

**Table: CohortSchedules** (multiple records created)
- `WeekNumber`, `StartTime`, `EndTime`
- `Cohort` (link to parent Cohort)
- `Template` and `Masterclass` (links)

## 🔗 Next Workflow
→ **Workflow 102a** ("When a Cohort Schedule is created")
- Automatically triggered when CohortSchedules are created

---

**Status:** ✅ Documented | **Updated:** March 2026
