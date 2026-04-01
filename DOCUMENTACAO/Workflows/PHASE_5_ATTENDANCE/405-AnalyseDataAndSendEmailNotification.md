# 405 - Analyse Data and Send Email Notification

## ✅ Status
Documented

## 📋 Overview
This workflow is a manual Power Apps / Flow trigger that analyzes a single attendance record and sends email notifications based on the final attendance decision.

## 🎯 Purpose
- Process one CohortsSchedulesAttendance item on demand
- Evaluate non-attendance, partial attendance and escalation logic
- Send the final notification email to the learner and escalation contacts

## ⏱ Trigger
- Manual trigger via Power Apps / flow button

## 🔧 Execution Summary
1. Accept a CohortsSchedulesAttendance Id from the trigger
2. Retrieve the attendance record from SharePoint
3. Load non-attendance notices and notification settings
4. Determine the notification type
5. Send learner notification and escalation emails

## 📡 Connectors
- SharePoint Online
- Office 365 Outlook

## 🔗 Notes
This flow is typically used for manual review or on-demand notification after attendance processing is complete.
