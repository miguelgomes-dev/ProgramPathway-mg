# 📖 Glossary of Terms

## Main Entities

### Cohort (Class/Group)
**Definition:** The central unit of the system - represents a class/group of students following a specific training program with a defined schedule.

**Key attributes:**
- Unique ID
- Program (which program will be taught)
- Start Date
- Status (ranges from 0 to 33+)
- Schedule
- Associated students

**Lifecycle:** Creation → Setup → Trainers → Coaches → Markers → Attendance → Closure

**Example:** "Cohort Q1-2026: Advanced Python, starting April 15"

---

### Program
**Definition:** Educational model that defines which masterclasses will be taught and in what sequence.

**Key attributes:**
- Program name
- List of masterclasses
- Total duration
- Learning objectives

**Relationship:** A Cohort is based on a specific Program

**Example:** "Advanced Python 2026", "Full Stack Web Development"

---

### Masterclass
**Definition:** Individual class/module that is part of a program.

**Key attributes:**
- Topic/title
- Responsible Trainer
- Duration
- Prerequisites (if any)

**Relationship:** Multiple masterclasses compose a Program, and all must be scheduled in a Cohort

**Example:** "Module 1: Python Fundamentals", "Module 5: Testing and Debugging"

---

### Schedule
**Definition:** Timetable that defines WHEN each masterclass will be taught within a cohort.

**Key attributes:**
- Date and time of classes
- Trainer responsible for each date
- Teams room/Team
- Confirmation status

**Creation:** Automatic via WF 102, based on:
- Trainer availability
- Program structure
- Cohort start date

**Important:** The Schedule must be confirmed by ALL trainers before proceeding

---

## System Actors

### Trainer (Instructor)
**Definition:** Professional responsible for teaching the masterclasses of a cohort.

**Responsibilities:**
- Confirm availability for proposed dates
- Sign contract
- Teach classes as scheduled

**Communications:**
- Receives invitations via Email/Teams
- Accesses SharePoint for details
- Signs contracts via Adobe Sign

**Status during process:**
- "Inviting Trainers" (inviting)
- "All Trainers Dates Confirmed" (confirmed availability)
- "Sending Out Trainers Contracts" (receiving contract)
- "All Trainers Contracts Confirmed" (contract signed)

**Associated workflows:** WF 103-110

---

### Coach (Facilitator)
**Definition:** Professional who provides support, mentoring, and facilitates student learning during the cohort.

**Responsibilities:**
- Confirm capacity to serve this cohort
- Sign contract
- Support and guide students

**Communications:**
- Receives "Capacity Confirmation" invitations
- Accesses documentation on SharePoint
- Signs contracts via Adobe Sign

**Status during process:**
- "Coach Capacity Pending" (awaiting response)
- "Coach Capacity Confirmed" (confirmed availability)
- "Coaches Contracts Confirmed" (contract signed)

**Associated workflows:** WF 201-207

---

### Marker (Evaluator)
**Definition:** Professional responsible for evaluating, correcting, and providing feedback on student performance.

**Responsibilities:**
- Confirm availability
- Sign contract
- Conduct evaluations per schedule

**Communications:**
- Receives "Capacity Confirmation" invitations
- Accesses evaluation tasks on SharePoint
- Signs contracts via Adobe Sign

**Status during process:**
- "Marker Capacity Pending" (awaiting response)
- "Marker Capacity Confirmed" (confirmed)
- "Markers Contracts Confirmed" (contract signed)

**Associated workflows:** WF 301-307

---

### Student
**Definition:** Participant taking the program within a specific cohort.

**Key attributes:**
- ID/CPF
- Name
- Email
- Cohort attending
- Progress/Grades

**Communications:**
- Receives schedule information
- Accesses materials via Teams/SharePoint
- Participates in masterclasses

**Note:** Organization of students by coaches/markers is done automatically by the system

---

## Operational Concepts

### Availability
**Definition:** Ability of a trainer/coach/marker to be present on proposed dates.

**Process:**
1. System proposes dates based on program
2. Actor confirms their availability (yes/no)
3. If "no": rejection workflow triggered (WF 102b, 201b, 301b)
4. If "yes": progresses to next phase

**Impact:** Affects entire cohort timeline

---

### Capacity Confirmation
**Definition:** Confirmation that a coach/marker HAS CAPACITY (resources, time) to serve this cohort.

**Difference from "Availability":**
- Availability = Presence on specific dates
- Capacity = Overall resources to do the work

**Workflows:** WF 201a (Coaches), WF 301a (Markers)

---

### Contract
**Definition:** Legal document that formalizes the relationship between the company and trainer/coach/marker.

**Characteristics:**
- Auto-generated from Word template
- Sent via email with signing link
- Digitally signed via **Adobe Sign**
- Stored in SharePoint
- Creates complete audit trail

**Flow:**
1. WF 105/204/304 = Send contract
2. WF 106/205/305 = Integrate Adobe Sign
3. WF 107/206/306 = Verify signature
4. WF 108/207/307 = Confirm completion

---

### Adobe Sign Signature
**Definition:** Digital document signature using Adobe Sign.

**Benefits:**
- ✅ Legal and valid
- ✅ Signing timestamp tracked
- ✅ No need to print/scan
- ✅ Automatic Power Automate integration

**System flow:**
1. Contract is generated (Word → PDF via Content Conversion)
2. Adobe Sign sends to signing actor
3. Actor receives email with Adobe Sign link
4. Actor signs digitally
5. Webhook notifies Power Automate
6. System validates signature receipt

---

### Online Event / Teams Meeting
**Definition:** Meeting/event automatically created in Microsoft Teams for each masterclass.

**Creation:** WF 109 (automatic)

**Purpose:**
- Centralized class location
- Automatic recording
- Facilitates student-trainer communication
- Screen/document sharing

**Integration:** Teams ↔ SharePoint ↔ Schedules

---

## Status & Cycles

### Status Codes (Cohort)
```
0 - Pending
1 - Preprocessing Data Validation
2 - Creating Cohort Schedule
3 - Inviting Trainers
4 - All Trainers Dates Confirmed
5 - Sending Out Trainers Contracts
6 - All Trainers Contracts Confirmed
8 - All Online Events Created
10 - All Cohorts Units Created
20-23 - Coach related statuses
30-33 - Marker related statuses
40+ - Attendance & Beyond
```

### Validations (Error Prevention Logic)

**Data Validation:**
- Future dates
- Valid emails
- Complete documents
- Rejection handling

**User Validation:**
- Confirms activity (no defaults)
- Handles rejections by re-inviting
- Timeouts trigger escalation

---

## Communications & Notifications

### Email
**Used for:**
- Initial invitations
- Contract delivery
- Confirmations
- Reminders

**Technology:** Office 365 Connector

---

### Teams Notifications
**Used for:**
- Event notifications
- Channel/meeting creation
- Real-time communication

**Technology:** Teams Connector

---

### SharePoint
**Used for:**
- Central data storage
- Documents (contracts, materials)
- Access without duplicate authentication

**Permissions:** Controlled by group/role

---

## Abbreviations & Acronyms

| Acronym | Meaning |
|---------|---------|
| WF | Workflow (automation flow) |
| PR | Programme Pathway (project name) |
| API | Application Programming Interface |
| PDF | Portable Document Format |
| CSV | Comma-Separated Values |
| GUID | Globally Unique Identifier |
| UTC | Coordinated Universal Time |
| Adobe Sign | Digital Signature Service |

---

## Quick References

- **Phases**: Setup (WF 101-110) → Trainers → Coaches (parallel) + Markers (parallel)
- **Triggers**: Cohort creation (WF 101) → Automatic per status
- **Database**: SharePoint Online (lists + libraries)
- **Communication**: Email + Teams + Adobe Sign

For detailed flows, see [ARCHITECTURE.md](ARCHITECTURE.md)
