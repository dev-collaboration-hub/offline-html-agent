
````markdown
# offline-html-agent

A CPU-efficient, fully offline AI programming agent that takes a web-development goal and autonomously plans, builds, validates, repairs, and completes HTML/CSS/JavaScript projects.

---

## Objective

`offline-html-agent` is a local-first autonomous programming agent focused specifically on web projects built with:

- HTML
- CSS
- JavaScript

The user provides a goal.

Example:

> Build a responsive personal portfolio website with a dark mode, project section, contact form, and mobile navigation.

The agent should determine what needs to be built, create the required project structure, implement the code, validate the result, fix detected problems, and continue working until the requested goal is satisfied.

The project is designed to operate:

- fully offline,
- on ordinary CPUs,
- with low computational requirements,
- with minimal memory usage,
- without requiring cloud AI APIs.

---

## Core Idea

The agent follows a goal-completion loop:

```text
User Goal
    ↓
Understand Requirements
    ↓
Create Project Plan
    ↓
Break Plan Into Tasks
    ↓
Generate / Modify Files
    ↓
Validate Code
    ↓
Detect Problems
    ↓
Repair Problems
    ↓
Check Goal Completion
    ↓
Complete Project
````

The agent should not stop merely because code was generated.

Its job is to determine whether the **project goal has actually been completed**.

---

## Design Principles

### 1. Offline First

Core functionality must work without:

* cloud APIs,
* remote inference services,
* internet access,
* mandatory external AI providers.

Internet access may eventually exist as an optional capability, but it must never be required by the core agent.

---

### 2. CPU First

The system should be designed for ordinary desktop and laptop CPUs.

The architecture should avoid unnecessary:

* large neural-network inference,
* repeated full-project processing,
* expensive embeddings,
* GPU dependencies,
* excessive memory allocation.

---

### 3. Low Computation

The agent should perform the smallest amount of computation necessary to make progress.

Examples:

Instead of regenerating an entire project:

```text
Detect affected file
→ locate affected section
→ make minimal edit
```

Instead of repeatedly analysing every file:

```text
Track project state
→ analyse only changed or relevant files
```

---

### 4. Goal Driven

The user specifies **what should exist**, not every implementation step.

Example:

```text
Goal:
Create a responsive landing page for an AI startup.
```

The agent may derive:

```text
Requirements
├── navigation
├── hero section
├── features section
├── CTA
├── responsive layout
└── mobile navigation
```

The agent then converts those requirements into executable tasks.

---

### 5. Project Completion

Generating valid HTML alone is not considered success.

The agent should verify:

* required features exist,
* required files exist,
* HTML structure is valid,
* referenced resources exist,
* CSS selectors match the page,
* JavaScript references valid DOM elements,
* obvious runtime problems are absent,
* requested functionality has been implemented.

Only then should a task be considered complete.

---

### 6. Incremental Editing

Existing projects should be modified instead of unnecessarily regenerated.

Example:

```text
User:
Add dark mode.
```

Preferred behavior:

```text
Inspect existing project
→ identify affected HTML/CSS/JS
→ create minimal implementation plan
→ modify affected sections
→ validate changes
```

Not:

```text
Regenerate entire website from scratch.
```

---

## Intended Capabilities

The long-term agent should be able to:

* create new web projects,
* understand existing HTML projects,
* generate HTML,
* generate CSS,
* generate JavaScript,
* create project directories,
* create and modify files,
* maintain project state,
* derive requirements from goals,
* break requirements into tasks,
* track task completion,
* inspect generated code,
* detect structural problems,
* detect broken references,
* detect incomplete requirements,
* repair its own output,
* iteratively improve a project,
* determine when the requested project is complete.

---

## Agent Architecture

A possible high-level architecture:

```text
                ┌───────────────────┐
                │     User Goal     │
                └─────────┬─────────┘
                          │
                          ▼
                ┌───────────────────┐
                │ Goal Interpreter  │
                └─────────┬─────────┘
                          │
                          ▼
                ┌───────────────────┐
                │ Requirement Model │
                └─────────┬─────────┘
                          │
                          ▼
                ┌───────────────────┐
                │      Planner      │
                └─────────┬─────────┘
                          │
                          ▼
                ┌───────────────────┐
                │   Task Manager    │
                └─────────┬─────────┘
                          │
                          ▼
                ┌───────────────────┐
                │   Code Engine     │
                └─────────┬─────────┘
                          │
                          ▼
                ┌───────────────────┐
                │ File Operations   │
                └─────────┬─────────┘
                          │
                          ▼
                ┌───────────────────┐
                │     Validator     │
                └─────────┬─────────┘
                          │
                          ▼
                ┌───────────────────┐
                │   Repair Engine   │
                └─────────┬─────────┘
                          │
                          ▼
                ┌───────────────────┐
                │ Completion Check  │
                └───────────────────┘
```

---

## Major Components

### Goal Interpreter

Transforms a natural-language user goal into structured intent.

Example:

```text
"Build a responsive calculator"
```

may become:

```text
project_type: web_application

requirements:
- calculator interface
- numeric input
- arithmetic operations
- responsive layout
```

---

### Requirement Engine

Maintains explicit project requirements.

Every generated task should trace back to a requirement.

This prevents the agent from randomly adding features that were never required.

---

### Planner

Converts requirements into an implementation strategy.

Example:

```text
Requirement:
Dark mode

Plan:
1. Add theme toggle
2. Define dark-theme CSS
3. Add JavaScript state toggle
4. Persist preference if required
5. Validate theme behavior
```

---

### Task Manager

Tracks small executable tasks.

Example:

```text
[✓] Create index.html
[✓] Create styles.css
[✓] Build navigation
[ ] Build hero section
[ ] Add responsive styles
[ ] Validate project
```

---

### Code Engine

Creates or modifies code required by the current task.

The engine should prefer targeted edits over complete regeneration.

---

### Project Model

Maintains a lightweight representation of the repository.

Example:

```text
project/
├── index.html
├── css/
│   └── styles.css
└── js/
    └── app.js
```

The agent should understand relationships between these files.

---

### Validator

Checks whether produced code is structurally correct.

Possible checks include:

* HTML structure,
* duplicate IDs,
* missing files,
* broken file references,
* invalid asset paths,
* CSS/HTML mismatches,
* JavaScript DOM references,
* incomplete requirements.

---

### Repair Engine

Transforms detected problems into repair tasks.

```text
Problem
↓
Root cause
↓
Affected files
↓
Minimal repair
↓
Revalidation
```

---

### Completion Engine

Determines whether the original goal has actually been satisfied.

```text
Original Goal
      +
Requirements
      +
Completed Tasks
      +
Validation Results
      ↓
Completion Decision
```

Possible states:

```text
INCOMPLETE
BLOCKED
VALIDATING
COMPLETE
```

---

## Efficiency Strategy

The agent should minimize computation using:

### Incremental Analysis

Only changed or relevant files should be reprocessed.

### Dependency Tracking

The system should know relationships such as:

```text
index.html
    ↓ uses
styles.css

index.html
    ↓ loads
app.js
```

### Structured State

Important information should be stored explicitly rather than rediscovered repeatedly.

### Deterministic Checks

Whenever possible, use deterministic algorithms instead of AI inference.

Examples:

* HTML parsing
* dependency checking
* path validation
* duplicate-ID detection
* task tracking
* requirement coverage
* project graph analysis

### Minimal Rewrites

Modify the smallest possible code region.

---

## What This Project Is NOT

This project is not intended to become:

* a general-purpose chatbot,
* a cloud AI wrapper,
* a GPU-dependent code generator,
* a general autonomous computer-control agent,
* a full IDE replacement,
* a generic agent for every programming language.

Its primary responsibility is:

> **Efficiently completing HTML/CSS/JavaScript projects from user-defined goals on local CPU hardware.**

---

## Development Roadmap

### M0 — Foundation

* define objective
* define architecture
* define project state
* define requirement model
* define task model
* establish deterministic file operations

### M1 — Project Understanding

* scan project directories
* identify HTML/CSS/JS files
* build project representation
* detect file relationships

### M2 — Goal Understanding

* accept user goals
* extract requirements
* normalize requirements
* maintain requirement state

### M3 — Planning

* convert requirements into tasks
* determine task dependencies
* order tasks
* track progress

### M4 — Code Generation

* create HTML
* create CSS
* create JavaScript
* create project structures
* modify existing files

### M5 — Validation

* HTML validation
* resource validation
* project consistency checks
* CSS/DOM consistency checks
* JavaScript reference checks

### M6 — Self Repair

* detect implementation failures
* determine affected files
* generate repair tasks
* apply minimal corrections
* revalidate changes

### M7 — Goal Completion

* requirement coverage
* completion reasoning
* unresolved-task detection
* deterministic completion report

### M8 — Autonomous Project Loop

```text
Goal
→ Plan
→ Implement
→ Validate
→ Repair
→ Re-check
→ Complete
```

### M9 — Optimization

* incremental analysis
* cached project state
* selective file processing
* reduced memory usage
* reduced CPU work
* minimal code rewrites

---

## Example

Input:

```text
Build a responsive expense tracker with:

- add expense
- delete expense
- total amount
- category field
- browser storage
```

Possible agent workflow:

```text
1. Parse goal
2. Extract five requirements
3. Design project structure
4. Create implementation tasks
5. Generate index.html
6. Generate styles.css
7. Generate app.js
8. Validate resource links
9. Validate required UI elements
10. Validate JavaScript DOM references
11. Check requirement coverage
12. Repair missing functionality
13. Revalidate
14. Mark project COMPLETE
```

Output:

```text
expense-tracker/
├── index.html
├── styles.css
└── app.js
```

---

## Project Status

🚧 Early development / architecture stage.

The architecture, interfaces, algorithms, and implementation will evolve while preserving the locked project objective.

---

## Long-Term Vision

`offline-html-agent` should make it possible for a user to describe a web project in plain language and let a lightweight local agent handle the implementation process.

The final experience should move toward:

```text
Describe what you want.
        ↓
Agent builds it.
        ↓
Agent checks it.
        ↓
Agent fixes it.
        ↓
Working project.
```

without requiring powerful GPUs or permanent cloud connectivity.

---

## License

License to be decided.

```

```
