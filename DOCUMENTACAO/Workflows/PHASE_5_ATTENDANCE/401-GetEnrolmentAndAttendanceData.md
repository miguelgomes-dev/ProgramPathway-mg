# 401 - Get Enrolment and Attendance Data

## ✅ Status
Documented

## 📋 Overview
This workflow collects attendance data for cohort schedules and prepares the attendance tracker for downstream validation.

## 🎯 Purpose
- Retrieve cohort schedule rows that are pending attendance processing
- Initialize Graph API authentication and meeting attendance variables
- Process Teams meeting attendance records for each schedule
- Prepare notification and attendance status values for the next phase

## ⏱ Trigger
- Recurrence: Daily at **3:00 AM GMT**

## 🔧 Execution Summary
1. Query SharePoint for cohorts schedule items that are ready for attendance processing
2. Initialize Graph API token and attendance-state variables
3. Loop through each schedule item
4. Retrieve Teams meeting details, attendance reports and attendance records
5. Build attendance data for SharePoint updates and notifications

## 📡 Connectors
- SharePoint Online
- Microsoft Graph / HTTP
- Office 365 Outlook
- Content Conversion

## 🔗 Next Workflow
- **402 - Check For Confirming Attendance**
