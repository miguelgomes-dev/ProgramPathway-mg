# 204 - Send Contracts to Coaches

## 📝 Executive Summary
Initiates contract sending to all coaches whose capacity has been confirmed. Runs daily at 4:30 AM GMT. Acts as orchestrator: finds unique coaches by cohort and triggers Adobe Sign process for each coach once per cohort.

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 04:30 AM GMT
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 4:30 AM]
         ↓
[Load cohort status reference]
         ↓
[Query: Find cohorts with Status=21 ("All Coaches Capacity Confirmed")]
         ↓
[FOR EACH cohort with confirmed coaches:]
         ├─ Find all coach assignments (where contract NOT sent yet)
         ├─ Extract unique coach list (remove duplicates)
         ├─ FOR EACH unique coach:
         │  └─ Call Workflow 205 (Adobe Sign contract)
         │
         └─ Update Cohort Status → 22 ("Sending Out Coaches Contracts")
         ↓
[✅ Workflow 204 Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference status mapping |
| 2 | Query Ready Cohorts | Find cohorts with status=21 |
| 3 | Loop Each Cohort | Process all ready cohorts |
| 4 | Get Unsent Contracts | Identify pending sends |
| 5 | Extract Unique Coaches | Remove duplicates |
| 6 | Send Contracts | Call WF 205 for each coach |
| 7 | Update Cohort Status | Set status to 22 |

## 📤 Status Transition

| From | To | Condition |
|------|----|----|
| **21** = "All Coaches Capacity Confirmed" | **22** = "Sending Out Coaches Contracts" | When WF 204 executes |

## 🔗 Next Workflow
→ **Workflow 205** ("Send Contract to Coach using Adobe Sign")
- Called individually for each unique coach
- Handles digital signature process

## ⚠️ Key Features

| Feature | Behavior |
|---------|----------|
| **Deduplication** | Extracts UNIQUE coaches only (prevents duplicate contracts) |
| **Per Cohort** | Processes one cohort at a time |
| **Per Coach** | Sends one contract per coach |
| **Timing** | 4:30 AM → 30 minutes after capacity check |

## ⏰ Processing Sequence

```
4:00 AM   → WF 203: Check all coach confirmations
            (Cohort status updates to 21)
            ↓
4:30 AM   → WF 204: Send contracts to coaches
            (Status updates to 22, WF 205 triggered)
            ↓
4:30-5:00 AM → WF 205: Adobe Sign process runs
              (One per coach)
```

## 🔀 Comparison: Coaches vs Trainers

| Aspect | Trainers (105) | Coaches (204) |
|--------|---|---|
| **Trigger Time** | 1:30 AM | 4:30 AM |
| **Status Range** | 4 → 5 | 21 → 22 |
| **Check Time** | 1:00 AM | 4:00 AM |
| **Pattern** | Identical logic |

---

**Status:** ✅ Documented | **Updated:** March 2026
