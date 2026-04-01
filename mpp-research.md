# Research: Software Supporting Microsoft Project (.mpp) Format

## Overview
This document analyzes major project management software that supports the Microsoft Project (`.mpp`) file format, specifically focusing on their common UI patterns and schedule editing logic. The tools analyzed are Microsoft Project, ProjectLibre, GanttProject, Smartsheet, and monday.com.

## 1. Analyzed Software & .mpp Support Profiles

| Software | `.mpp` Support Level | Core Architecture | Target Audience |
| :--- | :--- | :--- | :--- |
| **MS Project** | Native (Creator) | Desktop & Cloud (Enterprise) | Professional Project Managers, PMOs |
| **ProjectLibre** | High (Open-source clone) | Desktop (Java-based) | Budget-conscious PMs, traditional workflows |
| **GanttProject** | Moderate (Import/Export) | Desktop (Java-based) | Small teams, straightforward Gantt needs |
| **Smartsheet** | High (Import workflow) | Cloud (Spreadsheet-first) | Agile teams, cross-functional collaborators |
| **monday.com** | Moderate (Import workflow) | Cloud (Work OS / Board-first) | General business workflows, modern agile teams |

---

## 2. Common UI Patterns

Despite differing architectures (desktop vs. cloud-native), these applications converge on several established UI paradigms to handle complex project data.

### 2.1 The "Split-View" Paradigm (Grid + Visual)
*   **Implementation:** The screen is divided vertically. The left pane is a hierarchical grid/table containing task metadata (WBS, Task Name, Duration, Start, Finish, Predecessors). The right pane is a timeline/Gantt chart visualizing the data.
*   **Adopted by:** MS Project, ProjectLibre, GanttProject, Smartsheet (in Gantt view), monday.com (via Timeline/Gantt widget paired with a board).

### 2.2 Hierarchical Data Structures (WBS)
*   **Implementation:** Tasks are organized into parent-child relationships (summary tasks and subtasks).
*   **UI Interaction:** Expand/collapse chevrons or plus/minus icons next to summary tasks. Indentation in the primary "Task Name" column indicates hierarchy depth.

### 2.3 Visual Dependency Linking
*   **Implementation:** Interactive Gantt bars where users can click and drag from the end of one task bar to the beginning of another to establish a Finish-to-Start (FS) dependency.
*   **Visual Cue:** Lines with directional arrows connecting task bars.

### 2.4 Contextual Toolbars / Ribbons
*   **Legacy (MS Project, ProjectLibre):** Heavy use of the Microsoft Ribbon UI paradigm, categorizing actions into tabs (Task, Resource, View, Format).
*   **Modern (Smartsheet, monday.com):** Contextual floating menus or simplified top action bars that appear when a row/task is selected.

---

## 3. Common Schedule Editing Logic

The underlying scheduling engines of these tools share a common lineage rooted in the Critical Path Method (CPM), though cloud tools often simplify the logic for accessibility.

### 3.1 Auto-Scheduling vs. Manual Scheduling
*   **Logic:** 
    *   *Auto-scheduled:* The engine calculates Start/Finish dates automatically based on project start date, task duration, dependencies, and constraints.
    *   *Manual-scheduled:* User inputs dates directly; the engine ignores dependencies for date calculation (often flagging scheduling conflicts rather than auto-resolving them).
*   **Prevalence:** MS Project and ProjectLibre heavily emphasize this toggle. Smartsheet supports Project Settings to enforce auto-scheduling based on predecessor columns.

### 3.2 The Predecessor-Driven Equation
*   **Logic:** `Start Date = Predecessor Finish Date + Lag/Lead Time`.
*   **Standard Constraints:** Finish-to-Start (FS - default), Start-to-Start (SS), Finish-to-Finish (FF), Start-to-Finish (SF).
*   All analyzed tools support FS. MS Project, ProjectLibre, and Smartsheet support all four types plus lead/lag times. monday.com primarily supports FS through its strict dependency columns.

### 3.3 Duration-Driven Scheduling
*   **Logic:** Rather than picking a start and end date on a calendar, the user defines a Start Date and a Duration (e.g., "5d"). The engine calculates the Finish Date by skipping non-working days (weekends, holidays defined in a project calendar).
*   **Formula:** `Finish Date = Start Date + Duration + Non-working time`.

### 3.4 Effort-Driven Scheduling (Resource Constrained)
*   **Logic:** `Duration = Work (Effort) / Assignment Units (Resource allocation)`.
*   If a task takes 40 hours of work, assigning two people reduces the duration to 20 hours (5 days down to 2.5 days).
*   **Prevalence:** Highly prominent in MS Project and ProjectLibre. Simplified or requires manual formula setup in Smartsheet and monday.com.

### 3.5 The Critical Path Calculation
*   **Logic:** A dynamic algorithm identifying the longest sequence of dependent tasks that prevents the project from being completed sooner. Tasks on this path have zero "float" or "slack."
*   **UI Representation:** Usually highlighted in red on the Gantt chart.

## Summary Conclusion
While legacy tools (MS Project, ProjectLibre) operate as strict Critical Path engines focusing on resource leveling and effort-driven scheduling, modern cloud tools (Smartsheet, monday.com) treat `.mpp` data more like structured spreadsheet or database imports. However, all tools retain the core UI split-screen Gantt paradigm and the basic algebraic logic of `Start + Duration + Predecessor = Finish` to maintain familiarity for project managers.