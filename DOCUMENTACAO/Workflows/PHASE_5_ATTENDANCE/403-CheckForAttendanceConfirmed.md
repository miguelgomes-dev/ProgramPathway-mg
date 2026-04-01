# 403 - Check For Attendance Confirmed

## ✅ Status
Documented

## 📋 Overview
This workflow verifies completed attendance processing and confirms that cohort schedule attendance has been accepted.

## 🎯 Purpose
- Retrieve attendance tracker candidates
- Check whether attendance is confirmed
- Promote confirmed records to the next state or keep them in review

## ⏱ Trigger
- Recurrence: Daily at **4:00 AM GMT**

## 🔧 Execution Summary
1. Query SharePoint for cohort schedule attendance records
2. Initialize attendance status values
3. Evaluate confirmation conditions for each record
4. Update SharePoint state as needed

## 📡 Connectors
- SharePoint Online

## 🔗 Next Workflow
- **404 - Sending Out Email Notifications**
