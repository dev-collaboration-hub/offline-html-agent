# offline-html-agent

A CPU-efficient, fully offline AI programming agent that autonomously plans, builds, validates, repairs, and completes HTML, CSS, and JavaScript projects from a user-defined goal.

## Objective

`offline-html-agent` is a local-first autonomous programming agent focused on completing web-development projects using:

* HTML
* CSS
* JavaScript

The user describes the desired project in natural language.

For example:

> Build a responsive portfolio website with dark mode, a projects section, contact form, and mobile navigation.

The agent should understand the goal, derive requirements, plan the implementation, create or modify files, validate the project, repair detected problems, and continue until the requested goal is satisfied.

The system is designed to work:

* Fully offline
* On ordinary CPUs
* With low computational requirements
* With low memory usage
* Without mandatory cloud AI APIs
* Without requiring a GPU

---

## Core Workflow

The agent follows this process:

**User Goal → Requirements → Plan → Tasks → Implementation → Validation → Repair → Goal Check → Complete Project**

Generating code alone does not mean the task is complete.

The agent must determine whether the original project goal has actually been satisfied.

---

## Design Principles

### 1. Offline First

Core functionality must work without:

* Cloud APIs
* Remote inference services
* Internet connectivity
* Mandatory external AI providers

Internet-based functionality may eventually be optional, but the core agent must remain usable offline.

### 2. CPU First

The architecture should be optimized for ordinary desktop and laptop CPUs.

The system should avoid unnecessary:

* Large neural-network inference
* Full-project regeneration
* GPU dependencies
* Repeated analysis
* Excessive memory allocation
* Expensive background computation

### 3. Low Computation

The agent should perform only the computation necessary for the current task.

If one CSS rule needs modification, the agent should not regenerate the entire project.

Preferred behavior:

**Find affected requirement → Find affected file → Find affected section → Apply minimal change → Validate**

### 4. Goal Driven

The user defines what should be created.

The agent determines how to create it.

A goal such as:

> Create a responsive landing page for an AI startup.

may produce requirements such as:

* Navigation
* Hero section
* Features section
* Call-to-action
* Responsive layout
* Mobile navigation

These requirements then become implementation tasks.

### 5. Incremental Editing

Existing projects should be modified rather than unnecessarily regenerated.

For example, when the user asks:

> Add dark mode.

The agent should:

* Inspect the existing project
* Locate relevant HTML, CSS, and JavaScript
* Determine the minimal required changes
* Modify only affected sections
* Validate the result

### 6. Deterministic Operations First

Whenever a problem can be solved reliably without AI inference, deterministic algorithms should be preferred.

Examples include:

* HTML parsing
* File discovery
* Path validation
* Duplicate ID detection
* Dependency checking
* Task tracking
* Requirement coverage
* Project graph construction
* Changed-file detection

This helps keep CPU usage low.

---

## Intended Capabilities

The agent should eventually be capable of:

* Creating complete web projects
* Understanding existing projects
* Generating HTML
* Generating CSS
* Generating JavaScript
* Creating folders and files
* Editing existing code
* Understanding file relationships
* Extracting requirements from goals
* Breaking requirements into tasks
* Tracking project progress
* Detecting incomplete functionality
* Detecting structural errors
* Detecting broken references
* Repairing generated code
* Revalidating repaired code
* Determining whether the project goal is complete

---

## Core Architecture

The system is divided into several focused components.

### Goal Interpreter

Understands the user's requested project and converts it into structured intent.

### Requirement Engine

Maintains explicit project requirements.

Every implementation task should correspond to one or more requirements.

This reduces feature drift.

### Planner

Determines how the requirements should be implemented.

### Task Manager

Breaks the plan into small executable tasks and tracks their status.

Possible task states include:

* Pending
* Ready
* Running
* Completed
* Failed
* Blocked

### Project Model

Maintains a lightweight representation of the current project.

It should understand:

* Project files
* File types
* Dependencies
* References
* Components
* Changed files

### Code Engine

Creates and modifies HTML, CSS, and JavaScript.

It should prefer small targeted edits instead of rewriting entire files.

### File Engine

Safely performs:

* File creation
* File reading
* File modification
* Directory creation
* Project scanning

### Validator

Checks the generated project for problems.

Validation may include:

* HTML structure
* Missing resources
* Invalid file paths
* Duplicate IDs
* Broken references
* Missing required elements
* JavaScript DOM references
* Requirement coverage

### Repair Engine

Takes detected problems and converts them into focused repair tasks.

The repair cycle is:

**Problem → Root Cause → Affected Files → Minimal Repair → Revalidation**

### Completion Engine

Determines whether the user's original goal has been satisfied.

Possible project states:

* INCOMPLETE
* WORKING
* BLOCKED
* VALIDATING
* COMPLETE

The project should only become `COMPLETE` when required functionality has been implemented and validated.

---

## Efficiency Strategy

Low computation is a core requirement, not a later optimization.

### Incremental Analysis

Only relevant or changed files should be reprocessed whenever possible.

### Dependency Tracking

The system should remember relationships between HTML, CSS, JavaScript, assets, and components.

### Cached Project State

Information that has already been discovered should not be repeatedly recomputed unless the underlying file changes.

### Minimal Rewrites

The agent should modify the smallest practical region of code.

### Selective Validation

A local change should initially trigger validation of affected components rather than unnecessarily validating everything.

Full-project validation can run before final completion.

### Lightweight Internal Representations

Project state, requirements, tasks, and dependencies should use compact structured data rather than repeatedly reconstructing information from raw source files.

---

## Goal Completion

The agent must distinguish between:

**Code generated**

and:

**Goal completed**

A project should only be marked complete after checking:

* All required features are represented
* Required files exist
* Required functionality is implemented
* File references are valid
* Detected problems are resolved
* No required tasks remain unfinished
* Requirement coverage is complete

---

## Example

User goal:

> Build a responsive expense tracker with add expense, delete expense, categories, total amount, and browser storage.

The agent should derive requirements such as:

1. Expense input
2. Expense list
3. Category support
4. Delete functionality
5. Total calculation
6. Browser persistence
7. Responsive interface

Then it should:

1. Create the project plan
2. Create implementation tasks
3. Generate required files
4. Implement functionality
5. Validate the project
6. Detect missing or broken behavior
7. Repair problems
8. Check every requirement
9. Mark the project complete

---

## What This Project Is Not

`offline-html-agent` is not intended to become:

* A general-purpose chatbot
* A cloud AI wrapper
* A GPU-dependent coding system
* A generic computer-control agent
* A full IDE replacement
* A universal programming-language agent

Its primary responsibility is:

**Complete HTML, CSS, and JavaScript projects from user-defined goals efficiently on local CPU hardware.**

---

## Development Roadmap

### M0 — Foundation

* Lock project objective
* Define architecture
* Define project state
* Define requirement model
* Define task model
* Build safe file operations

### M1 — Project Understanding

* Scan project directories
* Detect HTML, CSS, and JavaScript
* Build project representation
* Discover file relationships
* Track changed files

### M2 — Goal Understanding

* Accept natural-language project goals
* Extract requirements
* Normalize requirements
* Maintain requirement state

### M3 — Planning

* Convert requirements into tasks
* Determine dependencies
* Order tasks
* Track execution state

### M4 — Code Engine

* Generate HTML
* Generate CSS
* Generate JavaScript
* Create project structures
* Modify existing code
* Support incremental edits

### M5 — Validation

* HTML checks
* Resource checks
* Dependency checks
* DOM consistency checks
* Requirement checks

### M6 — Self Repair

* Detect implementation failures
* Find root causes
* Identify affected files
* Apply minimal repairs
* Revalidate changes

### M7 — Goal Completion

* Requirement coverage
* Outstanding-task detection
* Completion decision
* Deterministic completion reports

### M8 — Autonomous Agent Loop

Combine planning, implementation, validation, repair, and completion checking into one controlled autonomous workflow.

### M9 — CPU Optimization

* Incremental analysis
* Cached state
* Selective processing
* Minimal rewrites
* Memory reduction
* CPU-work reduction
* Performance measurement

---

## Long-Term Vision

The intended user experience is simple:

**Describe the website you want → Agent plans it → Agent builds it → Agent checks it → Agent fixes it → Working project**

The system should achieve this locally while remaining lightweight enough to run efficiently on normal CPU hardware.

---

## Project Status

Early development.

The implementation will evolve while preserving the project's locked objective and CPU-first, offline-first constraints.

---

