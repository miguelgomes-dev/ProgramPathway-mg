# PHASE 5: Attendance (WF 401-405)

## ✅ Status
Documented

## 📋 Overview
Phase 5 tracks learner attendance after cohort events are scheduled and executed.
This phase retrieves attendance data from Microsoft Teams, records it in SharePoint,
validates attendance status, and sends notifications or escalations as needed.

## 🎯 Objective
- Collect attendance data for cohort schedule meetings
- Validate attendance status and completion
- Send attendance notifications to learners and support team
- Support manual review and escalation for non-attendance

## 📊 Phase 5 Workflows
| ID | Name | Trigger | Status |
|----|------|---------|--------|
| 401 | Get Enrolment and Attendance Data | Recurrence (3:00 AM GMT) | data collection |
| 402 | Check For Confirming Attendance | Recurrence (3:00 AM GMT) | validation |
| 403 | Check For Attendance Confirmed | Recurrence (4:00 AM GMT) | confirmation |
| 404 | Sending Out Email Notifications | Recurrence (5:00 AM GMT) | notification |
| 405 | Analyse Data and Send Email Notification | Manual (Power Apps / button) | manual review |

## 🔧 Execution Flow
1. **WF 401** collects pending attendance data from Teams and builds attendance records.
2. **WF 402** checks pending records and prepares them for confirmation.
3. **WF 403** validates confirmed attendance.
4. **WF 404** sends out attendance email notifications.
5. **WF 405** provides an on-demand review and notification path for a specific attendance record.

## 📡 Data Flow
- Teams attendance data -> Graph API
- SharePoint attendance lists -> attendance tracker fields
- Email notifications -> learners and escalation contacts

## 📝 Notes
- The implementation package is available at: `PHASE_5_ATTENDANCE/PR_AttendanceTracker_1_0_0_1_managed (1)/Workflows/`
- The live flows are based on the Attendance Tracker solution and follow the project’s recurring attendance pipeline.
- This phase is designed to integrate with the Learner Portal and to support attendance review workflows.

---

**Status:** Documented | **Updated:** April 2026
