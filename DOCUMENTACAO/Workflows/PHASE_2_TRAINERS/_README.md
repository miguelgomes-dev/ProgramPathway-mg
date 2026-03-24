# PHASE 2: Trainers - Details (WF 103-110)

## 📋 Overview

Technically, **Trainers are not a separate "phase"** - they're processed as part of **Phase 1 (Cohort Setup)**. This document separates only to **ease reading** of trainer-related workflows.

## 🎯 Objective

- ✅ Invite trainers to the program
- ✅ Confirm their availability for proposed dates
- ✅ Build the Schedule based on availabilities
- ✅ Send and ensure contract signing
- ✅ Create Teams events for classes

## 📊 Phase 2 Workflows (Trainers)

| ID | Name | Trigger | Function |
|----|------|---------|----------|
| 103a | Send Invitation to Trainer | Recurrence (1:00 AM) | Send initial invitation |
| 103b | Calculate Dates for Deadline | Recurrence | Calculate reminder dates |
| 104 | Check Trainers Dates Confirmed | Recurrence (1:00 AM) | Verify if ALL confirmed |
| 105 | Send Contracts to Trainers | Recurrence (1:30 AM) | Send for signature |
| 106 | Send via Adobe Sign | Auto | Integrate with Adobe Sign |
| 107 | Check Adobe Agreement | Recurrence Daily | Verify signature |
| 108 | Check Contracts Confirmed | Recurrence Daily | Confirm completion |
| 109 | Create Online Events | Recurrence (3:00 AM) | Create Teams meetings |
| 110 | Create Cohorts Units | Auto | Create unit structure |

**Also:**
- WF 102a: When Schedule is created (trigger for this phase)
- WF 102b: Handles invitation rejections

## ⏰ Typical Timeline

```
DAY 1 (When Cohort created):
  - WF 101 triggers
  - WF 102a creates Schedule automatically
  - WF 103a sends invitations to trainers

DAYS 2-3:
  - Trainers receive email and confirm availability
  - If available: mark as "Yes"
  - If not: mark as "No" (WF 102b intervenes)
  - WF 104 checks status

DAYS 3-4:
  - WF 105 sends contracts when ALL confirmed
  - WF 106 integrates with Adobe Sign
  - Trainers sign digitally

DAYS 4-5:
  - WF 107-108 checks signatures
  - WF 109 creates teams meetings
  - WF 110 creates units

RESULT: Phase 1 complete, ready for Coaches & Markers
```

## 🔄 Difference: Confirming "Availability" vs "Capacity"

| Aspect | Trainers | Coaches | Markers |
|--------|----------|---------|---------|
| **Confirm** | DATE availability | GENERAL capacity | EVALUATION capacity |
| **Question** | "Can you on days X, Y, Z?" | "Can you mentor this cohort?" | "Can you evaluate this cohort?" |
| **Decision** | Yes/No per date | Yes/No general | Yes/No general |
| **Impact if No** | Schedule needs reschedule | Find other coach | Find other marker |

## 🎓 Key Points

### WF 103a - Initial Invitation
- Finds all trainers not yet invited
- Sends email with PROPOSED dates
- Email has link to confirm on SharePoint
- Can be sent multiple times (recurrence)

### WF 103b - Date Calculation
- Calculates when "final deadline" is to respond
- Calculates when "last day before" (warning)
- Uses for automatic reminders

### WF 104 - Confirmation Verification
- **Critical**: Checks if ALL trainers confirmed
- If YES: Can move to contracts
- If NO: Wait for next iteration (next midnight)
- Can have timeout (if after 3 days no response, escalate)

### WF 105-110 - Contracts and Events
- WF 105: Send contract (but recurrence controls frequency)
- WF 106: Integrate Adobe Sign (automatic when contract sent)
- WF 107: Check Adobe Sign if was signed
- WF 108: Confirm completion (all signed?)
- WF 109: Create Teams meeting for each class
- WF 110: Create unit structure to organize content

## ⚠️ Special Cases

### If a Trainer Says "No"?

```
Trainer said "No" for some dates
         ↓
WF 102b (Handle Rejection) triggers
         ↓
System needs to:
  1. Remove trainer from those dates
  2. Find alternate trainer
  3. If can't find: Drop that class OR
  4. Propose new Schedule dates for entire cohort
```

**Decision is manual** or automatic per configuration.

### If Trainer Rejects Contract?

```
Trainer doesn't want to sign
         ↓
System tries to resend 3x
         ↓
If still not signed: Manual intervention
```

## 🔗 Exact Sequence

```
WF 101: Cohort created
    ↓
WF 102a: Schedule created automatically
    ↓
WF 103a: Invitations sent (1:00 AM, recurrence)
    ↓
WF 103b: Date calculation (parallel)
    ↓
WF 104: Confirmation verification (1:00 AM, loop)
    ├─ While not ALL confirmed: wait
    └─ When ALL confirmed: advance
    ↓
WF 105: Contracts sent (1:30 AM)
    ↓
WF 106: Adobe Sign integrated (automatic)
    ↓
WF 107: Adobe verification (daily, loop)
    ├─ While not ALL signed: wait
    └─ When ALL signed: advance
    ↓
WF 108: Final confirmation (daily)
    ↓
WF 109: Teams meetings created (3:00 AM)
    ↓
WF 110: Units created (automatic)
    ↓
✅ TRAINERS PHASE COMPLETE
    ↓
Phase 3 and 4 can start (Coaches & Markers)
```

## 🚀 Phase 2 Exit State

When trainers are ready:
- ✅ ALL trainers confirmed dates
- ✅ ALL contracts signed
- ✅ ALL Teams meetings created
- ✅ Schedule is definitive (won't change)

## 📚 Related File

Also read:
- [PHASE_1_SETUP/_README.md](PHASE_1_SETUP/_README.md) - Bigger context
- [00_WORKFLOW_REFERENCE.md](00_WORKFLOW_REFERENCE.md) - All IDs

---

**Note:** Technically, no "PHASE 2" exists as separate phase. Trainers are part of PHASE 1. This document only separates the logic for clarity.

**Status:** ✅ Documented | **Updated:** March 2026
