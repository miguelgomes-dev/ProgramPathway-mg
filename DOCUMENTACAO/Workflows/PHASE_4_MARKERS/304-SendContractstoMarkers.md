# 304 - Send Contracts to Markers

## 📝 Executive Summary
Orchestrator workflow triggered daily at 5:30 AM when all markers have confirmed capacity. Iterates through all markers assigned to the cohort and triggers Workflow 305 for each marker (Adobe Sign contract sending). Updates cohort status to "Sending Contracts to Markers".

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 05:30 AM GMT
- **Or Manual:** Can be triggered manually if needed

## 🔄 Execution Flow

```
[Recurrence trigger at 5:30 AM]
         ↓
[Query: Find cohorts with Status=31 (All Markers Capacity Confirmed)]
         ↓
[FOR EACH COHORT:]
         ├─ Set Status = 32 ("Sending Contracts to Markers")
         ├─ Filter: Only markers with CapacityConfirmed=1
         └─ FOR EACH MARKER:
              ├─ Create CohortMarkerContract record
              ├─ Call Workflow 305
              │  (Send contract via Adobe Sign)
              └─ Continue to next marker
         ↓
[All markers processed]
         ↓
[✅ Workflow 304 Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohorts Waiting | Find status=31 cohorts |
| 2 | Set Cohort Status 32 | "Sending Contracts to Markers" |
| 3 | Loop Confirmed Markers | For each marker with capacity confirmed |
| 4 | Create Contract Record | Initialize marker contract |
| 5 | Call Workflow 305 | Send via Adobe Sign |
| 6 | Continue Loop | Process all markers |

## 📤 SharePoint Updates

**Cohort Level:**
- `CohortStatus` → **32** ("Sending Contracts to Markers")

**Marker Contract Level (created):**
- `CohortId` → Cohort identifier
- `MarkerId` → Marker identifier
- `ContractStatus` → "PendingSigning"
- `ContractSentDate` → Current timestamp

## ⏰ Execution Timeline

```
5:00 AM   → WF 303: Check all markers confirmed
            
5:30 AM   → WF 304: Send contracts to markers
            ├─ Status 31 → 32
            ├─ Create contract records
            └─ For EACH marker, call WF 305
            
(WF 305 executes in parallel for each marker)
```

## 🔗 Next Workflow
→ **Workflow 305** ("Send Contract to Marker Using Adobe Sign")
- Called once per marker
- Creates agreement, sends for signature

## ⚠️ Key Notes

| Feature | Behavior |
|---------|----------|
| **Orchestrator Pattern** | Calls WF 305 multiple times (once per marker) |
| **Parallelization** | All marker contracts sent ~simultaneously |
| **Status Tracking** | Cohort moves to "Sending Contracts" immediately |
| **Idempotency** | Re-running creates duplicate contracts |

## 🔀 Comparison: Coaches vs Markers

| Aspect | Coaches (204) | Markers (304) |
|--------|---|---|
| **Trigger Status** | 21 (All confirmed) | 31 (All confirmed) |
| **New Status** | 22 | 32 |
| **Contract Actor** | Coach | Marker |
| **Called Workflow** | WF 205 | WF 305 |
| **Pattern** | Identical |

---

**Status:** ✅ Documented | **Updated:** March 2026
