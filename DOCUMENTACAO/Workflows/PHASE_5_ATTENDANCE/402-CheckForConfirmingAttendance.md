# 402 - Check For Confirming Attendance

## ✅ Status
Documented

## 📋 Overview
This workflow evaluates attendance trackers and identifies which cohort schedules can move into the confirming attendance state.

## 🎯 Purpose
- Retrieve cohort schedule attendance records for review
- Evaluate attendance status values
- Mark schedules that are ready for the next attendance confirmation step

## ⏱ Trigger
- Recurrence: Daily at **3:00 AM GMT**

## 🔧 Execution Summary
1. Query SharePoint for cohort schedule attendance records
2. Loop through each record
3. Initialize attendance status values
4. Validate whether the record is in the correct state for confirmation

## 📡 Connectors
- SharePoint Online

## 🔗 Next Workflow
- **403 - Check For Attendance Confirmed**
