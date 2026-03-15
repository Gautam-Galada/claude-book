# CLAUDE.md

> This file loads automatically every Claude Code session.
> Keep it under 200 lines — beyond that adherence drops.
> Hard rules and permanent constraints only live here.
> Everything else (tasks, findings, patterns) lives in
> separate files you reference when you need them.
>
> HOW TO REFERENCE OTHER FILES FROM HERE:
> Use @filename.md to pull in any file on demand.
> Examples of what you would say to Claude Code:
>
>   "Load @SKILL.md and implement the new cache layer."
>   "Load @.claude/task_auth.md and continue the refactor."
>   "Load @docs/findings.md and diagnose this failure."
>
> You can also reference files in your prompt directly:
>   "Read @outputs/cartography.jsonl and summarise patterns."
>
> Claude Code will inline the file contents at that point.
> Only load what the session actually needs.

---

## Project
[One sentence — what the system does and what problem it solves.
Example: "Job scheduler that assigns compute tasks to worker
nodes based on real-time resource availability."]

## Role
[What kind of engineer Claude is in this repo.
Example: "You are a senior backend engineer on the scheduler
core. You do not touch the frontend or infrastructure layer."]

---

## Hard Constraints
[Non-negotiable rules. Claude checks these before touching
any file. Write as absolutes, not suggestions.

Example: "Never modify anything in src/core/ — import only."
Example: "All new modules go in src/services/, not root."
Example: "Do not install new dependencies without confirmation."
Example: "All database writes go through the repository layer —
never call the ORM directly from a service."]

---

## File Permissions

  [R]  Read-only — import or load, never modify
  [W]  Write — you may create or edit this file
  [O]  Output — artefacts only, never scripts here
  [X]  Off-limits — do not reference or duplicate

[R]  [Example: src/core/]
[R]  [Example: config/schema.yaml]
[W]  [Example: src/services/]
[O]  [Example: outputs/]
[X]  [Example: scripts/deploy.sh]

---

## Placement Rules
[Where new files go. Claude checks this before creating anything.

Example: "New scripts → src/services/ only"
Example: "New artefacts → outputs/ only"
Example: "New tests → tests/ mirroring the src/ structure"
Example: "If a new file's location is ambiguous — ask first."]

---

## Critical Conventions
[Only conventions Claude cannot infer from reading the code.
Violations here silently break things.

Example: "Job IDs are always uuid4 strings — never integers."
Example: "All timestamps are UTC unix integers, never strings."
Example: "Load records by stable_id, not by position index."]

---

## Known Bugs Fixed — Do Not Reintroduce
[Deliberate fixes Claude might accidentally undo. Name the
bug, the fix, and the file it applies to.

Example: "worker_id vs node_id — node_id is the stable
identifier across restarts. worker_id is ephemeral.
All persistence layers use node_id. Do not swap these."]

---

## If Uncertain
State the uncertainty before writing any code.
A one-line question is always better than a wrong file.
