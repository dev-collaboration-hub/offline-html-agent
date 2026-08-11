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
