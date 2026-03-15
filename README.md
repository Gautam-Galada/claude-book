# README — Claude Optimization Guide

Three files. That is all you need to start.
Everything else in this guide is optional and additive.

  CLAUDE.md                   → Claude Code memory
  SKILL.md                    → Claude Code code patterns
  claude_ai_project_instructions.md → Claude.ai chat memory

---

## What Each File Does and When to Use It

### CLAUDE.md
Where it lives: repo root
Loaded by: Claude Code automatically, every session
Token cost: first turn only — cached for the rest of
            the session after that
Size limit: 200 lines hard limit. Beyond that Claude
            loads it but adherence drops.
What goes in it: hard constraints, file permissions,
                 placement rules, critical conventions,
                 known bugs fixed
What does NOT go in it: task-specific scope, code patterns,
                        investigation findings, anything
                        that only applies sometimes

---

### SKILL.md
Where it lives: repo root
Loaded by: you, explicitly, when writing new code
Token cost: only when you load it
What goes in it: error handling patterns, data access
                 patterns, testing conventions, logging
                 style, anti-patterns to never use
When NOT to load it: analysis sessions, planning sessions,
                     anything where you are not writing code

How to load it:
  "Load @SKILL.md and build the new cache layer."
  "Load @SKILL.md and write tests for the session manager."

---

### claude_ai_project_instructions.md
Where it lives: Claude.ai → your project → Instructions field
Loaded by: Claude.ai on every turn, automatically
Token cost: every single turn — no caching
Size limit: 400 words maximum. Every word costs every time.
What goes in it: project summary, your role, what Claude
                 is here for, stack, 2–3 ground truth facts,
                 active focus, response preferences, do-nots
What does NOT go in it: code, full findings, task detail —
                        those go in the conversation per turn

---

## How to Reference Other Files from CLAUDE.md

CLAUDE.md stays lean by design. When a session needs more
context — a task brief, investigation findings, an output
artefact — you reference it explicitly in your prompt.
Claude Code inlines the file contents at that point.

SYNTAX: @path/to/file.md

---

EXAMPLES:

You have a task brief written up:
  "Load @.claude/task_auth_refactor.md and continue
   the session token implementation."

You want Claude to reason from past findings:
  "Load @docs/findings.md and diagnose why the new
   cartography run shows different instability events."

You want Claude to read an output artefact:
  "Read @outputs/cartography_nl.jsonl and identify
   which samples are still in the ambiguous region."

You want both task context and code patterns:
  "Load @SKILL.md and @.claude/task_bootstrapper.md
   and build analyse_bootstrapper.py."

You want Claude to check a config before touching it:
  "Read @config/schema.yaml before proposing any
   changes to the validation layer."

---

## Optional Files to Add Later

Once you are comfortable with the three core files,
these are the next additions in order of usefulness.

### .claude/task_[name].md
What it is: a brief for one active task — scope, agreed
            approach, done criteria, autonomy boundaries
When to create one: when a task is complex enough that
                    you will be working on it across
                    multiple sessions
How to load it: @.claude/task_auth_refactor.md
Archive when done: rename to task_[name]_DONE.md so
                   it does not get loaded accidentally

Template structure:
  ## Task
  [Name and one-line objective]

  ## Status
  ACTIVE

  ## Prerequisite
  [What must be true before this task proceeds]

  ## Scope
  In scope:  [files and changes this task touches]
  Out of scope: [explicit boundary — what not to touch]

  ## Agreed Approach
  [The implementation strategy. Claude works within this.]

  ## What Done Looks Like
    [ ] [specific completion criterion]
    [ ] [specific completion criterion]

  ## Autonomy Boundaries
  Decide autonomously: [implementation details, naming]
  Stop and confirm:    [new dependencies, [R]/[X] files]

---

### docs/findings.md
What it is: ground truth from completed investigations —
            facts Claude must reason from, never re-derive
When to create one: after your first completed investigation
                    produces conclusions you will build on
How to load it: @docs/findings.md
How to write entries: declarative, not tentative.
                      "X is the cause" not "X may suggest"
When to trim it: when a finding has no bearing on current
                 active decisions — stale facts waste tokens

Template structure:
  ## Finding: [Short name]
  Established: [date or task name]
  Source: [path to artefact]

  [2–4 sentences. Declarative. Confirmed fact, not hypothesis.]

  Implications:
  [What this means for decisions being made right now.]

  ## Superseded Findings
  [Findings invalidated by new evidence. Never delete —
   mark as superseded so Claude does not resurrect them.]

---

### CLAUDE.local.md
What it is: personal preferences — never committed to the repo
Loaded by: Claude Code automatically alongside CLAUDE.md
What goes in it: your local environment paths, personal
                 workflow preferences, test credentials,
                 reminders only relevant to you
What does NOT go in it: team conventions — those stay in CLAUDE.md

Example contents:
  ## My Local Environment
  Local Redis runs on port 6380, not 6379.
  Python env: ~/.pyenv/versions/3.11.4/bin/python

  ## My Preferences
  Show diffs, not full files, when iterating on existing code.
  One step at a time with confirmation between steps.

  ## Things I Always Forget
  Run make proto-gen after any .proto file change.

---

## Token Cost at a Glance

| File | Loaded | Cost |
|---|---|---|
| CLAUDE.md | Every Claude Code session | First turn only (cached) |
| SKILL.md | When you explicitly load it | Only that session |
| .claude/task_*.md | When you explicitly load it | Only that session |
| docs/findings.md | When you explicitly load it | Only that session |
| CLAUDE.local.md | Every Claude Code session | First turn only (cached) |
| claude_ai_project_instructions.md | Every claude.ai turn | Every turn, no cache |

The most expensive file per message is the claude.ai
project instructions — it pays full price on every turn
with no caching. Keep it under 400 words.

The cheapest upgrade you can make is writing a good
CLAUDE.md — it costs almost nothing after the first turn
and saves you re-explaining constraints every session.

---

## The Rule That Governs All of Them

Every file has one job. When a file tries to do more than
its job it gets long, loads when it should not, and starts
costing tokens on sessions that did not need that information.

Keep each file focused on its job.
Keep it current when things change.
Load only what the session actually needs.
