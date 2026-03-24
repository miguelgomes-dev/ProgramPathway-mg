# 📋 Complete Reference Table - All Workflows

## Summary

| Total | Phases | Status |
|-------|--------|--------|
| 28 Workflows | 4 Phases | ✅ In Production |

---

## 📊 Complete Table

### PHASE 1: COHORT SETUP (WF 101-110)

| ID | Name | Type | Trigger | Objective | File |
|---|---|---|---|---|---|
| **101** | When a Cohort is created | Setup | Event | Initiates the process when a new Cohort is created | [Link](PHASE_1_SETUP/101-WhenaCohortiscreated.md) |
| **102a** | When a Cohort Schedule is created | Setup | Event | Triggers workflow when a schedule is created | [Link](PHASE_1_SETUP/102a-WhenaCohortScheduleiscreated.md) |
| **102b** | When a Trainer Invitation Rejected is created | Rejection | Event | Handles trainer invitation rejection | [Link](PHASE_1_SETUP/102b-WhenaTrainerInvitationRejectediscreated.md) |
| **103a** | Send Invitation to Trainer and Wait for Confirmation | Invitation | Recurrence (1:00 AM) | Sends initial invitation to trainers | [Link](PHASE_1_SETUP/103a-SendInvitationtoTrainerandWaitforConfirmation.md) |
| **103b** | Calculate and Update Dates for Deadline Reminder | Calculation | Recurrence | Calculates dates for deadline reminders | [Link](PHASE_1_SETUP/103b-CalculateandUpdateDatesforDeadlineReminder.md) |
| **104** | Check For Trainers Dates Confirmed | Verification | Recurrence (1:00 AM) | Verifies if ALL trainers have confirmed dates | [Link](PHASE_1_SETUP/104-CheckForTrainersDatesConfirmed.md) |
| **105** | Send Contracts to Trainers | Document | Recurrence (1:30 AM) | Sends contract documents via email | [Link](PHASE_1_SETUP/105-SendContractstoTrainers.md) |
| **106** | Send Contract to Trainer using Adobe Sign | Signature | Auto | Integrates with Adobe Sign for digital signature | [Link](PHASE_1_SETUP/106-SendContracttoTrainerusingAdobeSign.md) |
| **107** | Check For Trainers Adobe Sign Agreement | Verification | Recurrence Daily | Checks signature status in Adobe Sign | [Link](PHASE_1_SETUP/107-CheckForTrainersAdobeSignAgreement.md) |
| **108** | Check For Trainers Contracts Confirmed | Verification | Recurrence Daily | Validates if contracts have been signed | [Link](PHASE_1_SETUP/108-CheckForTrainersContractsConfirmed.md) |
| **109** | Create Online Events | Meeting | Recurrence (3:00 AM) | Creates meetings in Microsoft Teams | [Link](PHASE_1_SETUP/109-CreateOnlineEvents.md) |
| **110** | Create Cohorts Units | Structure | Auto | Creates cohort units/modules | [Link](PHASE_1_SETUP/110-CreateCohortsUnits.md) |

**Phase 1 Status:** ✅ Complete | **Typical Duration:** 3-5 days

---

### PHASE 2: TRAINERS (WF 102-110 detailed)

See the table above - WF 102-110 cover the entire trainers phase.

**Sequence:**
1. Initial invitation (103a)
2. Date calculation (103b)
3. Confirmation check (104)
4. Contract sending (105)
5. Adobe Sign (106)
6. Adobe check (107)
7. Contract confirmation (108)
8. Event creation (109)
9. Unit creation (110)

**Expected status at the end:** "All Trainers Contracts Confirmed" + "All Online Events Created"

---

### PHASE 3: COACHES (WF 201-207)

| ID | Name | Type | Trigger | Objective | File |
|---|---|---|---|---|---|
| **201a** | Send Capacity Confirmation to Coaches | Invitation | Recurrence (3:45 AM) | Sends invitation for coaches to confirm capacity | [Link](PHASE_3_COACHES/201a-SendCapacityConfirmationtoCoaches.md) |
| **201b** | When a Coach Confirmation Rejected is created | Rejection | Event | Handles coach rejection | [Link](PHASE_3_COACHES/201b-WhenaCoachConfirmationRejectediscreated.md) |
| **202** | Send Capacity Confirmation to Coach and Wait for Confirmation | Confirmation | Recurrence (3:45 AM) | Waits for capacity confirmation | [Link](PHASE_3_COACHES/202-SendCapacityConfirmationtoCoachandWaitforConfi.md) |
| **203** | Check For Coaches Capacity Confirmed | Verification | Recurrence (4:00 AM) | Verifies if ALL coaches have confirmed | [Link](PHASE_3_COACHES/203-CheckForCoachesCapacityConfirmed.md) |
| **204** | Send Contracts to Coaches | Document | Recurrence (4:30 AM) | Sends contracts for signature | [Link](PHASE_3_COACHES/204-SendContractstoCoaches.md) |
| **205** | Send Contract to Coach using Adobe Sign | Signature | Auto | Integrates Adobe Sign for coaches | [Link](PHASE_3_COACHES/205-SendContracttoCoachusingAdobeSign.md) |
| **206** | Check For Coaches Adobe Sign Agreement | Verification | Recurrence Daily | Checks signature in Adobe Sign | [Link](PHASE_3_COACHES/206-CheckForCoachesAdobeSignAgreement.md) |
| **207** | Check For Coaches Contracts Confirmed | Verification | Recurrence Daily | Confirms contract completion | [Link](PHASE_3_COACHES/207-CheckForCoachesContractsConfirmed.md) |

**Expected status at the end:** "Coaches Contracts Confirmed"

**Execution:** ⏳ Parallel with Phase 4 (Markers)

---

### PHASE 4: MARKERS (WF 301-307)

| ID | Name | Type | Trigger | Objective | File |
|---|---|---|---|---|---|
| **301a** | Send Capacity Confirmation to Markers | Invitation | Recurrence (~4:30 AM) | Sends invitation for markers to confirm capacity | [Link](PHASE_4_MARKERS/301a-SendCapacityConfirmationtoMarkers.md) |
| **301b** | When a Marker Confirmation Rejected is created | Rejection | Event | Handles marker rejection | [Link](PHASE_4_MARKERS/301b-WhenaMarkerConfirmationRejectediscreated.md) |
| **302** | Send Capacity Confirmation to Marker and Wait for Confirmation | Confirmation | Recurrence (~4:30 AM) | Waits for capacity confirmation | [Link](PHASE_4_MARKERS/302-SendCapacityConfirmationtoMarkerandWaitforConf.md) |
| **303** | Check For Markers Capacity Confirmed | Verification | Recurrence (5:00 AM) | Verifies if ALL markers have confirmed | [Link](PHASE_4_MARKERS/303-CheckForMarkersCapacityConfirmed.md) |
| **304** | Send Contracts to Markers | Document | Recurrence (5:30 AM) | Sends contracts for signature | [Link](PHASE_4_MARKERS/304-SendContractstoMarkers.md) |
| **305** | Send Contract to Marker using Adobe Sign | Signature | Auto | Integrates Adobe Sign for markers | [Link](PHASE_4_MARKERS/305-SendContracttoMarkerusingAdobeSign.md) |
| **306** | Check For Markers Adobe Sign Agreement | Verification | Recurrence Daily | Checks signature in Adobe Sign | [Link](PHASE_4_MARKERS/306-CheckForMarkersAdobeSignAgreement.md) |
| **307** | Check For Markers Contracts Confirmed | Verification | Recurrence Daily | Confirms contract completion | [Link](PHASE_4_MARKERS/307-CheckForMarkersContractsConfirmed.md) |

**Expected status at the end:** "Markers Contracts Confirmed"

**Execution:** ⏳ Parallel with Phase 3 (Coaches)

---

## 🚀 Execution Sequence

```text
    WF 101: Cohort Created (TRIGGER)
       ↓
    WF 102a: Schedule Created
       ↓
    WF 102b: Handle Rejections (if any)
       ↓
    WF 103a-b: Send Invites + Calculate Dates (Trainers)
       ↓
    WF 104: Check All Trainers Confirmed
       ↓
    WF 105-108: Send & Verify Contracts (Trainers)
       ↓
    WF 109: Create Events
       ↓
    WF 110: Create Units
       ↓
    ┌─────────────────────────────┐
    │  WF 201a-207 (COACHES)      │ [PARALLEL]
    │  WF 301a-307 (MARKERS)      │ [PARALLEL]
    └─────────────────────────────┘
       ↓
    [PHASE COMPLETE]
```
---

## 📊 Statistics by Type

| Type | Quantity | IDs |
|------|-----------|-----|
| **Event Trigger** | 5 | 101, 102a, 102b, 201b, 301b |
| **Recurrence Trigger** | 17 | 103a, 103b, 104, 105, 107, 108, 109, 202, 203, 204, 206, 207, 302, 303, 304, 306, 307 |
| **Auto Trigger** | 6 | 106, 205, 305, 110 |
| **Verification** | 8 | 104, 107, 108, 203, 206, 207, 303, 306, 307 |
| **Invitation** | 5 | 103a, 201a, 202, 301a, 302 |
| **Document/Signature** | 6 | 105, 106, 204, 205, 304, 305 |
| **Other** | 8 | 102a, 102b, 103b, 109, 110, 201b, 301b |

---

## ⏰ Execution Schedule (Typical Times)

| Time | Workflows | Function |
|---------|-----------|--------|
| **1:00 AM** | WF 103a, 104 | Send & verify trainers invitations |
| **1:30 AM** | WF 105, 107, 108 | Trainers contracts |
| **3:00 AM** | WF 109 | Create Teams events |
| **3:45 AM** | WF 201a, 202 | Send coaches invitations |
| **4:00 AM** | WF 203 | Verify coaches |
| **4:30 AM** | WF 204, 206, 207, 301a, 302 | Coaches contracts + Markers invitations |
| **5:00 AM** | WF 303 | Verify markers |
| **5:30 AM** | WF 304, 306 | Markers contracts |

**Note:** Times may vary according to timezone and configuration

---

## 🔗 Quick Links

### By Phase
- [Phase 1: Setup](PHASE_1_SETUP/)
- [Phase 2: Trainers](PHASE_2_TRAINERS/)
- [Phase 3: Coaches](PHASE_3_COACHES/)
- [Phase 4: Markers](PHASE_4_MARKERS/)

### Main Documentation
- [README.md](../README.md) - Overview
- [ARCHITECTURE.md](../ARCHITECTURE.md) - Diagrams
- [GLOSSARY.md](../GLOSSARY.md) - Definitions
- [REUSABILITY_GUIDE.md](../REUSABILITY_GUIDE.md) - Patterns

---

## 📝 Notes

- **Status:** All 28 workflows are ✅ in production
- **Last updated:** March 2026
- **Upcoming:** Attendance Workflows (Phase 5) planned
- **Maintenance:** Forward questions to the development team

---

For specific details on each workflow, click the [Link] in the "File" column
