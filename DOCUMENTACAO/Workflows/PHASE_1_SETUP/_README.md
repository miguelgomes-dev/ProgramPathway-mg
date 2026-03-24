# PHASE 1: Cohort Setup (WF 101-110)

## 📋 Overview

The **Phase 1** is where everything starts. When a new Cohort is created, this set of workflows **prepares the entire structure** needed for following phases.

## 🎯 Objective

Transform a simple Cohort creation into a **complete structure with:**
- ✅ Schedule assembled
- ✅ Trainers invited and confirmed
- ✅ Contracts signed
- ✅ Teams events/meetings created
- ✅ Cohort units/modules structured

## 📊 Phase 1 Workflows

| ID | Name | Typical Duration |
|----|----|--------------|
| 101 | When Cohort is created | Instantaneous |
| 102a | When Schedule is created | Instantaneous |
| 102b | Handle rejected invitations | On-demand |
| 103a | Send trainer invitations | 1-2 days |
| 103b | Calculate dates | On-demand |
| 104 | Check confirmation | 1-3 days |
| 105 | Send contracts | 1-2 days |
| 106 | Adobe Sign | Parallel |
| 107 | Check Adobe signature | 2-3 days |
| 108 | Confirm contracts | Parallel |
| 109 | Create Teams events | Instantaneous |
| 110 | Create units | Instantaneous |

## ⏰ Typical Timeline

```
DAY 1:
  - User creates Cohort
  - WF 101 triggers
  - WF 102a creates Schedule
  - WF 103a sends invitations

DAYS 2-3:
  - Trainers confirm availability
  - WF 104 checks confirmations

DAYS 3-4:
  - WF 105 sends contracts
  - WF 106-108 manages Adobe Sign

DAYS 4-5:
  - WF 107 checks signatures
  - WF 109 creates events
  - WF 110 creates units

RESULT: Setup complete, ready for Coaches & Markers
```

## 🔄 Detailed Flow

```
WF 101 (Event-based)
  "A new Cohort was created"
         ↓
    Status = "Preprocessing Data Validation"
         ↓
WF 102a (Event-based)
  "When Schedule is created for this Cohort"
         ↓
    Status = "Creating Cohort Schedule"
         ↓
┌─ WF 102b (Event-based)
│  "If Trainer rejected invitation"
│  → Resend with rejection logic
│
WF 103a + 103b (Recurrence Daily 1:00 AM)
  "Send invitations to trainers"
  "Calculate reminder dates"
         ↓
    Status = "Inviting Trainers"
         ↓
WF 104 (Recurrence Daily 1:00 AM)
  "Check if ALL confirmed"
  Loop until ALL confirm
         ↓
    Status = "All Trainers Dates Confirmed"
         ↓
WF 105-108 (Recurrence 1:30 AM)
  "Send contracts"
  "Adobe Sign process"
  "Check signature"
         ↓
    Status = "All Trainers Contracts Confirmed"
         ↓
WF 109 (Recurrence 3:00 AM)
  "Create Teams events for each masterclass"
         ↓
    Status = "All Online Events Created"
         ↓
WF 110 (Automatic)
  "Create units/modules of cohort"
         ↓
    Status = "All Cohorts Units Created"
         ↓
    ✅ PHASE 1 COMPLETE
         ↓
    Ready for Phase 2 (Coaches & Markers)
```

## 🎓 Important Details

### Initial Trigger (WF 101)
- **When:** User clicks "Create Cohort" and fills: Program + Start Date
- **Result:** System creates Cohort in SharePoint, status = "Pending"
- **Next action:** WF 102a triggered automatically

### Invitations to Trainers (WF 103a)
- Sends email to ALL trainers of program
- Whose email? Of the invited trainer
- With what info? Proposed dates, confirmation link
- How to confirm? On SharePoint or email reply

### Confirmation Verification (WF 104)
- Simultaneously checks if ALL trainers confirmed
- If not: waits until tomorrow and checks again
- If yes: advances to contract sending
- Optional timeout: after X days, escalates manually

### Adobe Sign Integration (WF 106-108)
- Contract generated via Word template
- Converted to PDF
- Adobe Sign sends for signature
- Actor signs using email
- System validates signature automatically

### Event Creation (WF 109)
- For each masterclass in schedule
- Creates Teams meeting with:
  - Title: Masterclass name
  - Date/Time: Per Schedule
  - Participants: Trainer + Students
  - Recording: Automatic

### Unit Creation (WF 110)
- Creates structure: Cohort → Unit → Masterclass
- Organizes students by coach/marker (used in Phase 3)
- Prepares material repository

## ⚠️ Critical Points

| Critical Point | What we do | Why |
|---|---|---|
| Trainer doesn't confirm | Wait 3 days, then escalate | Don't want to block process |
| Trainer rejects | WF 102b resends | Maybe lost first email |
| Adobe Sign not signed | Reminds 3x, then manual | Contract is legal, we need it |
| Invalid email | Validates before sending | Don't send to wrong email |

## 🚀 Phase 1 Exit State

When Phase 1 completes, system is in state:
- ✅ Cohort fully structured
- ✅ All trainers confirmed
- ✅ All contracts signed
- ✅ All Teams events created
- ✅ Units/modules ready

## 🔗 Next Phases

- **Phase 2 (doesn't exist)** - Trainers already processed in Phase 1
- **Phase 3: Coaches** - Starts in parallel with Phase 4
- **Phase 4: Markers** - Starts in parallel with Phase 3

## 📚 Next File

See complete reference here: [00_WORKFLOW_REFERENCE.md](../00_WORKFLOW_REFERENCE.md)

For specific terms, consult: [GLOSSARY.md](../../GLOSSARY.md)

---

**Status:** ✅ Documented | **Updated:** March 2026
