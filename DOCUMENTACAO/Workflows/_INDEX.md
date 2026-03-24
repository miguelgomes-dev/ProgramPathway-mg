# 📑 Documentation Index

## 🎯 How to Use This Documentation

Choose your entry point:

### 🚀 For New to Project
1. Start here: [README.md](../README.md) - Project overview
2. Then read: [ARCHITECTURE.md](../ARCHITECTURE.md) - How it works
3. Consult: [GLOSSARY.md](../GLOSSARY.md) - Important terms

### 🔧 For Maintenance
1. Find workflow: [00_WORKFLOW_REFERENCE.md](00_WORKFLOW_REFERENCE.md) (table with all)
2. Open specific file within PHASE_X
3. Need to understand patterns: [REUSABILITY_GUIDE.md](../REUSABILITY_GUIDE.md)

### 🎓 For Reuse in New Project
1. Study: [REUSABILITY_GUIDE.md](../REUSABILITY_GUIDE.md) - Patterns used
2. Consult: [GLOSSARY.md](../GLOSSARY.md) - Key concepts
3. Adapt patterns to your case

---

## 📂 Complete File Structure

```
DOCUMENTATION/
│
├── README.md ⭐
│   └─ Project overview
│      - Problem, solution, benefits
│      - Technology stack
│      - Version history
│
├── ARCHITECTURE.md 🏗️
│   └─ How the system works
│      - Overall flow (Mermaid diagram)
│      - Phase details
│      - Data flow (SharePoint ↔ Power Automate ↔ Connectors)
│      - Scheduling & triggers
│      - States & transitions
│
├── GLOSSARY.md 📖
│   └─ Term definitions
│      - Entities: Cohort, Program, Schedule, etc
│      - Actors: Trainer, Coach, Marker, Student
│      - Concepts: Availability, Capacity, Contract, etc
│      - Abbreviations & acronyms
│
├── REUSABILITY_GUIDE.md 🔄
│   └─ Patterns & Best Practices
│      - 5 main patterns identified
│      - Recipes for creating new workflows
│      - Checklist for new workflow
│      - Extension methods (new phase, new connector)
│      - Common troubleshooting
│
├── Workflows/
│   │
│   ├── 00_WORKFLOW_REFERENCE.md 📋
│   │   └─ Table with ALL 28 workflows
│   │      - By phase
│   │      - With status, trigger, objective
│   │      - Execution sequence
│   │      - Statistics by type
│   │      - Execution schedule (times)
│   │
│   ├── PHASE_1_SETUP/
│   │   ├── _README.md (phase overview)
│   │   ├── 101-WhenaCohortiscreated.md
│   │   ├── 102a-WhenaCohortScheduleiscreated.md
│   │   ├── 102b-WhenaTrainerInvitationRejectediscreated.md
│   │   ├── 103a-SendInvitationtoTrainerandWaitforConfirmation.md
│   │   ├── 103b-CalculateandUpdateDatesforDeadlineReminder.md
│   │   ├── 104-CheckForTrainersDatesConfirmed.md
│   │   ├── 105-SendContractstoTrainers.md
│   │   ├── 106-SendContracttoTrainerusingAdobeSign.md
│   │   ├── 107-CheckForTrainersAdobeSignAgreement.md
│   │   ├── 108-CheckForTrainersContractsConfirmed.md
│   │   ├── 109-CreateOnlineEvents.md
│   │   └── 110-CreateCohortsUnits.md
│   │
│   ├── PHASE_2_TRAINERS/
│   │   └── _README.md (Trainers - part of PHASE 1, separated for clarity)
│   │
│   ├── PHASE_3_COACHES/
│   │   ├── _README.md (phase overview)
│   │   ├── 201a-SendCapacityConfirmationtoCoaches.md
│   │   ├── 201b-WhenaCoachConfirmationRejectediscreated.md
│   │   ├── 202-SendCapacityConfirmationtoCoachandWaitforConfi.md
│   │   ├── 203-CheckForCoachesCapacityConfirmed.md
│   │   ├── 204-SendContractstoCoaches.md
│   │   ├── 205-SendContracttoCoachusingAdobeSign.md
│   │   ├── 206-CheckForCoachesAdobeSignAgreement.md
│   │   └── 207-CheckForCoachesContractsConfirmed.md
│   │
│   ├── PHASE_4_MARKERS/
│   │   ├── _README.md (phase overview)
│   │   ├── 301a-SendCapacityConfirmationtoMarkers.md
│   │   ├── 301b-WhenaMarkerConfirmationRejectediscreated.md
│   │   ├── 302-SendCapacityConfirmationtoMarkerandWaitforConf.md
│   │   ├── 303-CheckForMarkersCapacityConfirmed.md
│   │   ├── 304-SendContractstoMarkers.md
│   │   ├── 305-SendContracttoMarkerusingAdobeSign.md
│   │   ├── 306-CheckForMarkersAdobeSignAgreement.md
│   │   └── 307-CheckForMarkersContractsConfirmed.md
│   │
│   └── PHASE_5_ATTENDANCE/
│       └── _PLACEHOLDER.md (⏳ In development)
│
└── [This file: _INDEX.md]
```

---

## 🔍 Quick Search Guide

### By Question Type

**"What is a Cohort?"**
→ [GLOSSARY.md](../GLOSSARY.md#Cohort)

**"How does the system work?"**
→ [ARCHITECTURE.md](../ARCHITECTURE.md)

**"What are the 28 workflows?"**
→ [00_WORKFLOW_REFERENCE.md](00_WORKFLOW_REFERENCE.md)

**"How do I create a new workflow?"**
→ [REUSABILITY_GUIDE.md](../REUSABILITY_GUIDE.md#Pattern-Recipes)

**"What is 'Capacity Confirmation'?"**
→ [GLOSSARY.md](../GLOSSARY.md#Capacity-Confirmation)

**"What time do workflows run?"**
→ [00_WORKFLOW_REFERENCE.md](00_WORKFLOW_REFERENCE.md#-Execution-Schedule)

**"How do Coaches and Markers work?"**
→ [PHASE_3_COACHES/_README.md](PHASE_3_COACHES/_README.md) and [PHASE_4_MARKERS/_README.md](PHASE_4_MARKERS/_README.md)

**"Why did we choose Power Automate?"**
→ [README.md](../README.md#-Technology-Stack)

**"What are the 5 main patterns?"**
→ [REUSABILITY_GUIDE.md](../REUSABILITY_GUIDE.md#-Identified-Patterns)

---

## 📊 Documentation Statistics

| Metric | Value |
|--------|-------|
| **Main documents** | 4 |
| **READMEs per phase** | 5 |
| **Reference tables** | 1 (00_WORKFLOW_REFERENCE.md) |
| **Patterns described** | 5 |
| **Documented workflows** | 28 |
| **Glossary terms** | 30+ |
| **Recipes** | 3 |

---

## 🎯 Reading Checklist

Mark as you read:

- [ ] **README.md** - Understand the project
- [ ] **ARCHITECTURE.md** - See overall flow
- [ ] **GLOSSARY.md** - Learn terms
- [ ] **00_WORKFLOW_REFERENCE.md** - See all workflows
- [ ] **Phase 1 (_README)** - Understand setup
- [ ] **Phase 3 (_README)** - Understand coaches
- [ ] **Phase 4 (_README)** - Understand markers
- [ ] **REUSABILITY_GUIDE.md** - Learn patterns

---

## 🚀 Next Actions

### After reading documentation:
1. Discuss identified patterns with your team
2. Validate if anything is missing
3. When Attendance workflows are ready, update Phase 5
4. Keep this documentation synchronized with code

### For Azure DevOps Wiki:
1. Copy content from these files to the Wiki
2. Convert links to Azure DevOps links
3. Configure Wiki for "Markdown"
4. Create a home page with link to README.md

---

## 📞 Documentation Version

```
Version: 1.0
Date: March 2026
Status: ✅ Complete (Phases 1-4)
Missing: Phase 5 (Attendance)
Next review: When Attendance is ready
```

---

**Last updated:** March 2026 | **Responsible:** Development Team

For improvement suggestions, contact your solution architect.
