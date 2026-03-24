# 105 - Send Contracts to Trainers

## 📝 Executive Summary
Initiates contract sending to all trainers whose dates have been confirmed. Runs daily at 1:30 AM GMT. Acts as orchestrator: finds unique trainers by cohort and triggers Adobe Sign process for each trainer once per cohort.

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 01:30 AM GMT
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 1:30 AM]
         ↓
[Load cohort status reference]
         ↓
[Query: Find cohorts with Status=4 ("All Trainers Dates Confirmed")]
         ↓
[FOR EACH cohort with confirmed trainers:]
         ├─ Find all schedules (where contract NOT sent yet)
         ├─ Extract unique trainer list (remove duplicates)
         ├─ FOR EACH unique trainer:
         │  └─ Call Workflow 106 (Adobe Sign contract)
         │
         └─ Update Cohort Status → 5 ("Sending Out Trainers Contracts")
         ↓
[✅ Workflow 105 Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference status mapping |
| 2 | Query Ready Cohorts | Find cohorts with status=4 |
| 3 | Loop Each Cohort | Process all ready cohorts |
| 4 | Get Unsent Schedules | Identify pending contract sends |
| 5 | Extract Unique Trainers | Remove duplicate trainer names |
| 6 | Send Contracts | Call WF 106 for each trainer |
| 7 | Update Cohort Status | Set status to 5 |

## 💼 Contract Sending Logic

```
FOR EACH Cohort in "All Trainers Dates Confirmed" status:
  
  1. Find all CohortSchedules
  2. Filter where ContractSent ≠ true
  3. Get unique trainer list:
     [Trainer A, Trainer B, Trainer C] (no duplicates)
  
  4. For each unique trainer:
     → Call Workflow 106 (Adobe Sign)
     → Send contract document
     → Mark ContractSent = true
  
  5. Update Cohort Status → 5
```

## 📤 Status Transition

| From | To | Condition |
|------|----|----|
| **4** = "All Trainers Dates Confirmed" | **5** = "Sending Out Trainers Contracts" | When WF 105 executes |

## 🔗 Next Workflow
→ **Workflow 106** ("Send Contract to Trainer using Adobe Sign")
- Called individually for each unique trainer
- Handles digital signature process

## ⚠️ Key Features

| Feature | Behavior |
|---------|----------|
| **Deduplication** | Extracts UNIQUE trainers only (prevents duplicate contracts) |
| **Per Cohort** | Processes one cohort at a time |
| **Per Trainer** | Sends one contract per trainer (not per schedule) |
| **Timing** | 1:30 AM → 30 minutes after trainer date check |

## ⏰ Processing Sequence

```
1:00 AM   → WF 104: Check all trainer dates confirmed
            (Cohort status updates to 4)
            ↓
1:30 AM   → WF 105: Send contracts to trainers
            (Status updates to 5, WF 106 triggered)
            ↓
1:30-2:00 AM → WF 106: Adobe Sign process runs
              (One per trainer)
```

---

**Status:** ✅ Documented | **Updated:** March 2026
