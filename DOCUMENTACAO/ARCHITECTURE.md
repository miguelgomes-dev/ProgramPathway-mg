# 📋 Complete Workflow Reference Table

## Summary

| Total | Phases | Status |
|-------|--------|--------|
| 33 Workflows | 5 Phases | Phase 5 in progress |

---

## 📊 Complete Table

### PHASE 1: COHORT SETUP (WF 101-110)

| ID | Name | Type | Trigger | Purpose | File |
|---|---|---|---|---|---|
| **101** | When a Cohort is created | Setup | Event | Starts the process when a new Cohort is created | [Link](FASE_1_SETUP/101-WhenaCohortiscreated.md) |
| **102a** | When a Cohort Schedule is created | Setup | Event | Triggers when a Cohort Schedule is created | [Link](FASE_1_SETUP/102a-WhenaCohortScheduleiscreated.md) |
| **102b** | When a Trainer Invitation Rejected is created | Rejection | Event | Handles a rejected trainer invitation | [Link](FASE_1_SETUP/102b-WhenaTrainerInvitationRejectediscreated.md) |
| **103a** | Send Invitation to Trainer and Wait for Confirmation | Invitation | Recurrence (1:00 AM) | Sends the initial invitation to trainers | [Link](FASE_1_SETUP/103a-SendInvitationtoTrainerandWaitforConfirmation.md) |
| **103b** | Calculate and Update Dates for Deadline Reminder | Calculation | Recurrence | Calculates dates for deadline reminders | [Link](FASE_1_SETUP/103b-CalculateandUpdateDatesforDeadlineReminder.md) |
| **104** | Check For Trainers Dates Confirmed | Verification | Recurrence (1:00 AM) | Checks whether all trainers confirmed dates | [Link](FASE_1_SETUP/104-CheckForTrainersDatesConfirmed.md) |
| **105** | Send Contracts to Trainers | Document | Recurrence (1:30 AM) | Sends contract documents by email | [Link](FASE_1_SETUP/105-SendContractstoTrainers.md) |
| **106** | Send Contract to Trainer using Adobe Sign | Signature | Auto | Integrates with Adobe Sign for digital signature | [Link](FASE_1_SETUP/106-SendContracttoTrainerusingAdobeSign.md) |
| **107** | Check For Trainers Adobe Sign Agreement | Verification | Recurrence Daily | Checks Adobe Sign signature status | [Link](FASE_1_SETUP/107-CheckForTrainersAdobeSignAgreement.md) |
| **108** | Check For Trainers Contracts Confirmed | Verification | Recurrence Daily | Validates that contracts have been signed | [Link](FASE_1_SETUP/108-CheckForTrainersContractsConfirmed.md) |
| **109** | Create Online Events | Meeting | Recurrence (3:00 AM) | Creates meetings in Microsoft Teams | [Link](FASE_1_SETUP/109-CreateOnlineEvents.md) |
| **110** | Create Cohorts Units | Structure | Auto | Creates the cohort units/modules | [Link](FASE_1_SETUP/110-CreateCohortsUnits.md) |

**Phase 1 Status:** ✅ Complete | **Typical Duration:** 3-5 days

---

### PHASE 2: TRAINERS (WF 102-110 detailed)

See the table above - WF 102-110 cover the full trainers phase

**Sequence:**
1. Initial invitation (103a)
2. Date calculation (103b)
3. Confirmation check (104)
4. Contract sending (105)
5. Adobe Sign (106)
6. Adobe verification (107)
7. Contract confirmation (108)
8. Event creation (109)
9. Unit creation (110)

**Expected end status:** "All Trainers Contracts Confirmed" + "All Online Events Created"

---

### PHASE 3: COACHES (WF 201-207)

| ID | Name | Type | Trigger | Purpose | File |
|---|---|---|---|---|---|
| **201a** | Send Capacity Confirmation to Coaches | Invitation | Recurrence (3:45 AM) | Sends coach capacity confirmation invites | [Link](FASE_3_COACHES/201a-SendCapacityConfirmationtoCoaches.md) |
| **201b** | When a Coach Confirmation Rejected is created | Rejection | Event | Handles coach rejection | [Link](FASE_3_COACHES/201b-WhenaCoachConfirmationRejectediscreated.md) |
| **202** | Send Capacity Confirmation to Coach and Wait for Confirmation | Confirmation | Recurrence (3:45 AM) | Waits for coach capacity confirmation | [Link](FASE_3_COACHES/202-SendCapacityConfirmationtoCoachandWaitforConfi.md) |
| **203** | Check For Coaches Capacity Confirmed | Verification | Recurrence (4:00 AM) | Checks whether all coaches confirmed | [Link](FASE_3_COACHES/203-CheckForCoachesCapacityConfirmed.md) |
| **204** | Send Contracts to Coaches | Document | Recurrence (4:30 AM) | Sends contracts for signing | [Link](FASE_3_COACHES/204-SendContractstoCoaches.md) |
| **205** | Send Contract to Coach using Adobe Sign | Signature | Auto | Integrates Adobe Sign for coaches | [Link](FASE_3_COACHES/205-SendContracttoCoachusingAdobeSign.md) |
| **206** | Check For Coaches Adobe Sign Agreement | Verification | Recurrence Daily | Checks Adobe Sign signature status | [Link](FASE_3_COACHES/206-CheckForCoachesAdobeSignAgreement.md) |
| **207** | Check For Coaches Contracts Confirmed | Verification | Recurrence Daily | Confirms contract completion | [Link](FASE_3_COACHES/207-CheckForCoachesContractsConfirmed.md) |

**Expected end status:** "Coaches Contracts Confirmed"

**Execution:** ⏳ Parallel with Phase 4 (Markers)

---

### PHASE 4: MARKERS (WF 301-307)

| ID | Name | Type | Trigger | Purpose | File |
|---|---|---|---|---|---|
| **301a** | Send Capacity Confirmation to Markers | Invitation | Recurrence (~4:30 AM) | Sends marker capacity confirmation invites | [Link](FASE_4_MARKERS/301a-SendCapacityConfirmationtoMarkers.md) |
| **301b** | When a Marker Confirmation Rejected is created | Rejection | Event | Handles marker rejection | [Link](FASE_4_MARKERS/301b-WhenaMarkerConfirmationRejectediscreated.md) |
| **302** | Send Capacity Confirmation to Marker and Wait for Confirmation | Confirmation | Recurrence (~4:30 AM) | Waits for marker capacity confirmation | [Link](FASE_4_MARKERS/302-SendCapacityConfirmationtoMarkerandWaitforConf.md) |
| **303** | Check For Markers Capacity Confirmed | Verification | Recurrence (5:00 AM) | Checks whether all markers confirmed | [Link](FASE_4_MARKERS/303-CheckForMarkersCapacityConfirmed.md) |
| **304** | Send Contracts to Markers | Document | Recurrence (5:30 AM) | Sends contracts for signing | [Link](FASE_4_MARKERS/304-SendContractstoMarkers.md) |
| **305** | Send Contract to Marker using Adobe Sign | Signature | Auto | Integrates Adobe Sign for markers | [Link](FASE_4_MARKERS/305-SendContracttoMarkerusingAdobeSign.md) |
| **306** | Check For Markers Adobe Sign Agreement | Verification | Recurrence Daily | Checks Adobe Sign signature status | [Link](FASE_4_MARKERS/306-CheckForMarkersAdobeSignAgreement.md) |
| **307** | Check For Markers Contracts Confirmed | Verification | Recurrence Daily | Confirms contract completion | [Link](FASE_4_MARKERS/307-CheckForMarkersContractsConfirmed.md) |

**Expected end status:** "Markers Contracts Confirmed"

**Execution:** ⏳ Parallel with Phase 3 (Coaches)

---

### PHASE 5: ATTENDANCE (WF 401-405)

| ID | Name | Type | Trigger | Purpose | File |
|---|---|---|---|---|---|
| **401** | Get Enrolment and Attendance Data | Data Collection | Recurrence (3:00 AM) | Collect attendance data for pending cohort schedules | [Link](PHASE_5_ATTENDANCE/401-GetEnrolmentAndAttendanceData.md) |
| **402** | Check For Confirming Attendance | Verification | Recurrence (3:00 AM) | Evaluate attendance records and prepare confirmation | [Link](PHASE_5_ATTENDANCE/402-CheckForConfirmingAttendance.md) |
| **403** | Check For Attendance Confirmed | Verification | Recurrence (4:00 AM) | Confirm attendance after processing | [Link](PHASE_5_ATTENDANCE/403-CheckForAttendanceConfirmed.md) |
| **404** | Sending Out Email Notifications | Notification | Recurrence (5:00 AM) | Send attendance-related emails | [Link](PHASE_5_ATTENDANCE/404-SendingOutEmailNotifications.md) |
| **405** | Analyse Data and Send Email Notification | Manual | Request | Analyse a single attendance record and send notification | [Link](PHASE_5_ATTENDANCE/405-AnalyseDataAndSendEmailNotification.md) |

**Expected end status:** "Attendance data processed"

**Execution:** ⏳ Daily attendance processing and on-demand review

---

## 🚀 Execution Sequence

```
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
| **Recurrence Trigger** | 21 | 103a, 103b, 104, 105, 107, 108, 109, 202, 203, 204, 206, 207, 302, 303, 304, 306, 307, 401, 402, 403, 404 |
| **Auto Trigger** | 6 | 106, 205, 305, 110 |
| **Verification** | 10 | 104, 107, 108, 203, 206, 207, 303, 306, 307, 402, 403 |
| **Invitation** | 5 | 103a, 201a, 202, 301a, 302 |
| **Document/Signature** | 6 | 105, 106, 204, 205, 304, 305 |
| **Other** | 10 | 102a, 102b, 103b, 109, 110, 201b, 301b, 401, 404, 405 |

---

## ⏰ Execution Schedule (Typical Times)

| Time | Workflows | Function |
|---------|-----------|--------|
| **1:00 AM** | WF 103a, 104 | Send & verify trainer invitations |
| **1:30 AM** | WF 105, 107, 108 | Trainer contracts |
| **3:00 AM** | WF 109 | Create Teams events |
| **3:45 AM** | WF 201a, 202 | Send coach invitations |
| **4:00 AM** | WF 203 | Verify coaches |
| **4:30 AM** | WF 204, 206, 207, 301a, 302 | Coach contracts + Marker invitations |
| **5:00 AM** | WF 303 | Verify markers |
| **5:30 AM** | WF 304, 306 | Marker contracts |

**Note:** Times may vary depending on timezone and configuration

---

## 🔗 Quick Links

### By Phase
- [Phase 1: Setup](FASE_1_SETUP/)
- [Phase 2: Trainers](FASE_2_TRAINERS/)
- [Phase 3: Coaches](FASE_3_COACHES/)
- [Phase 4: Markers](FASE_4_MARKERS/)

### Main Documentation
- [README.md](../README.md) - Overview
- [ARQUITETURA.md](../ARQUITETURA.md) - Diagrams
- [GLOSSARIO.md](../GLOSSARIO.md) - Definitions
- [GUIA_REUTILIZACAO.md](../GUIA_REUTILIZACAO.md) - Patterns

---

## 📝 Notes

- **Status:** 33 workflows documented. Phases 1-4 in production; Phase 5 Attendance in progress.
- **Last updated:** April 2026
- **Next:** Validate Attendance workflows and move Phase 5 to production
- **Maintenance:** Send questions to the development team

---

For specific workflow details, click the [Link] in the "File" column
