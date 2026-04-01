# 📚 Programme Pathway Portal - Complete Documentation

## 🎯 Project Overview

The **Programme Pathway Portal** is an automation solution developed with **Power Automate** and **SharePoint Online** that revolutionizes the process of creating and managing professional training programs.

### The Original Problem

The client faced an **extremely manual and time-consuming process**:
- Required **multiple entire days** to set up a new class (Cohort)
- Manual inclusion of: program → masterclass → trainer availability → contracts → coaches/markers → students → Teams meetings
- **High risk of human errors** in repetitive processes
- Lack of standardization
- Poor scalability

### The Solution

We developed an **intelligent automation** that reduces manual work to a single step:

**Client only needs to provide:**
1. Which Program will be taught
2. What date the class starts

**The system automatically:**
- ✅ Creates the cohort structure
- ✅ Builds schedule based on trainer availability
- ✅ Sends confirmations to Trainers, Coaches, and Markers
- ✅ Manages contracts (including digital signature via Adobe Sign)
- ✅ Organizes students by roles
- ✅ Creates Teams events/meetings
- ✅ Extracts attendance data from Teams via Microsoft Graph API
- ✅ Creates cohort units
- ✅ All with built-in error prevention logic

### Benefits Achieved

| Benefit | Impact |
|---------|--------|
| ⚡ **Time Savings** | Reduction from days to hours |
| ✅ **Standardization** | 100% of processes standardized |
| 🛡️ **Error Reduction** | Integrated validation logic |
| 💰 **Cost Savings** | Reduced human resource consumption |
| 📈 **Scalability** | Same effort for multiple cohorts |

---

## 🏗️ Technology Stack

| Technology | Purpose |
|------------|---------|
| **Power Automate** | Workflow orchestration and automation |
| **SharePoint Online** | Central database (lists and repositories) |
| **Microsoft Graph API** | Retrieve Teams meeting and attendance data |
| **Office 365** | Email and communications |
| **Microsoft Teams** | Event/meeting creation and notifications |
| **Adobe Sign** | Digital contract signatures |
| **Content Conversion** | Document transformation (Word→PDF, etc.) |

---

## 📋 Documentation Structure

```
📁 DOCUMENTATION/
├── README.md (this file)
├── ARCHITECTURE.md (flow diagrams)
├── GLOSSARY.md (domain terms)
├── REUSABILITY_GUIDE.md (patterns and best practices)
│
└── 📁 Workflows/
    ├── 00_WORKFLOW_REFERENCE.md (table of all 28 workflows)
    │
    ├── 📁 PHASE_1_SETUP/
    │   ├── _README.md
    │   ├── 101-WhenaCohortiscreated.md
    │   ├── 102a-WhenaCohortScheduleiscreated.md
    │   └── ... (remaining workflows for this phase)
    │
    ├── 📁 PHASE_2_TRAINERS/
    │   ├── _README.md
    │   └── ... (workflows 102-110)
    │
    ├── 📁 PHASE_3_COACHES/
    │   ├── _README.md
    │   └── ... (workflows 201-207)
    │
    ├── 📁 PHASE_4_MARKERS/
    │   ├── _README.md
    │   └── ... (workflows 301-307)
    │
    └── 📁 PHASE_5_ATTENDANCE/ (⏳ In development)
        ├── _README.md
        └── ... (workflows 401-405)
```

---

## 🚀 How to Use This Documentation

### For Maintenance
1. Read **ARCHITECTURE.md** to understand the overall flow
2. Access the specific workflow in **Workflows/**
3. Consult **GLOSSARY.md** to understand terms

### For Reuse in New Projects
1. Read **REUSABILITY_GUIDE.md**
2. Identify patterns applicable to your case
3. Adapt as needed

### To Add New Workflows
See **REUSABILITY_GUIDE.md** - section "How to Add New Workflows"

---

## 📊 Project Statistics

| Metric | Value |
|--------|-------|
| **Total Workflows** | 33 (Phases 1-4 in production, Phase 5 in progress) |
| **Main Phases** | 5 phases |
| **Connectors Used** | 6 connectors |
| **Controlled States** | 34+ states |
| **Documentation** | Complete + Extensible |

---

## 📝 Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | Mar 2026 | **Complete** - Phases 1-4 fully documented (28 workflows + 11 core files) |
| 1.1 | Apr 2026 | Attendance Workflows added to documentation (Phase 5) |

---

## 👥 Contact & Support

For questions or updates:
- Consult this documentation repository
- Validate changes with your development team

---

**Status**: ✅ In Production | 📅 Last updated: April 2026
