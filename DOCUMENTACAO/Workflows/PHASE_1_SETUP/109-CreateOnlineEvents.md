# 109- Create Online Events

## 📝 Executive Summary
Creates Microsoft Teams online meetings for each confirmed trainer schedule. Runs daily at 3:00 AM GMT. Gathers configuration settings (subject, message templates), generates Teams meeting URLs, and updates SharePoint with event links.

## ⚙️ Trigger
- **Type:** Recurrence (scheduled)
- **Frequency:** Daily (every 1 day)
- **Time:** 03:00 AM GMT
- **Initiator:** System scheduler

## 🔄 Execution Flow

```
[Daily trigger at 3:00 AM]
         ↓
[Load cohort status reference]
         ↓
[Query: Find cohorts with Status=6 ("All Trainers Contracts Confirmed")]
         ↓
[FOR EACH cohort ready for events:]
         ├─ Update Status → 7 ("Creating Online Events")
         │
         ├─ FOR EACH confirmed schedule:
         │  ├─ Get Trainer details (email, name)
         │  ├─ Fetch Event Subject (from Settings)
         │  ├─ Fetch Event Message (from Settings)
         │  ├─ Get StartDate/StartTime and EndDate/EndTime
         │  ├─ Create Teams Meeting
         │  │  ├─ Required Attendee: Program coordinator
         │  │  └─ Optional Attendee: Trainer
         │  └─ Update CohortSchedule with Teams URL
         │
         └─ Update Status → 8 ("All Online Events Created")
         ↓
[✅ Workflow 109 Complete]
```

## 📊 Main Actions

| # | Action Name | Purpose |
|---|---|---|
| 1 | Load Cohort Statuses | Reference status mapping |
| 2 | Query Ready Cohorts | Find cohorts with status=6 |
| 3 | Loop Each Cohort | Process each cohort |
| 4 | Update Status to 7 | Mark as creating events |
| 5 | Get Trainer Details | Fetch trainer info |
| 6 | Get Event Configuration | Load subject/message templates from Settings |
| 7 | Create Teams Meeting | Generate online meeting |
| 8 | Update CohortSchedule | Save Teams URL |
| 9 | Update Status to 8 | Mark events complete |

## 👥 Meeting Attendees

**Teams Meeting Setup:**
- **Required:** Program Email Account (coordinator/admin)
- **Optional:** Trainer email (receives calendar invite)
- **Students:** Not directly invited (enrolled separately)

## 🗓️ Meeting Details

**Created For:**
- Each CohortSchedule record
- One meeting per week/session
- Excludes deadline reminders (IsDeadlineReminder=0)

**Meeting Content:**
- **Subject:** Retrieved from Settings (configurable)
- **Message:** Retrieved from Settings (configurable)
- **Date/Time:** From CohortSchedule StartDate/StartTime and EndDate/EndTime
- **Platform:** Microsoft Teams (online meeting)

## 📤 SharePoint Updates

**Table: Cohorts**
- `Status` → 6 → 7 → 8 (transition states)

**Table: CohortSchedules**
- `EventUrl` → Teams meeting join URL
- (populated with onlineMeeting/joinUrl from Teams API)

## 🔗 Next Workflow
→ **Workflow 110** ("Create Cohorts Units")
- Triggered when status = 8
- Final Phase 1 step

## ⏰ Status Progression

```
Status 6: "All Trainers Contracts Confirmed"
    ↓ (during WF 109)
Status 7: "Creating Online Events"
    ↓ (after WF 109 completes)
Status 8: "All Online Events Created"
    ↓ (triggers WF 110)
Status 9: "Creating Cohorts Units" (WF 110)
```

## ⚙️ Configuration from Settings

**Fetches from SharePoint Settings List:**
- Event Subject template
- Event Message template
- Both configurable for customization

## ⚠️ Critical Points

| Aspect | Detail |
|--------|--------|
| **Student Enrollment** | Not handled here (separate process) |
| **Trainer Notification** | Receives optional calendar invite |
| **Timezone** | Uses cohort/trainer timezone settings |
| **Multiple Events** | One per schedule/week (parallel creation) |
| **URL Availability** | Stored in SharePoint for reference |

---

**Status:** ✅ Documented | **Updated:** March 2026
