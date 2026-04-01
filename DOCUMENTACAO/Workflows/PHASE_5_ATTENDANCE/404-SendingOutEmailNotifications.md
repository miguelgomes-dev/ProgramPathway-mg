# 404 - Sending Out Email Notifications

## ✅ Status
Documented

## 📋 Overview
This workflow sends attendance-related email notifications for cohort schedule records that have reached a notification state.

## 🎯 Purpose
- Retrieve attendance tracker records ready for notification
- Determine notification recipients and message content
- Send attendance-related emails

## ⏱ Trigger
- Recurrence: Daily at **5:00 AM GMT**

## 🔧 Execution Summary
1. Query SharePoint for cohort schedule attendance records
2. Initialize notification settings and email variables
3. Send appropriate attendance email notifications

## 📡 Connectors
- SharePoint Online
- Office 365 Outlook

## 🔗 Next Workflow
- **405 - Analyse Data and Send Email Notification** (manual review)
