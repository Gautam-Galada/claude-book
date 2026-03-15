# SKILL.md

> This file teaches Claude the patterns and conventions
> of your codebase so it writes conforming code from the
> first line without needing correction.
>
> Do NOT load this every session — only load it when you
> are asking Claude to write new code that must fit your
> existing style. For analysis or planning sessions,
> leave this file out entirely.
>
> How to load it:
>   "Load @SKILL.md and build the new session manager."
>
> Keep it current. A stale SKILL.md that describes old
> patterns is worse than no SKILL.md — Claude will follow
> outdated conventions confidently.

---

## Language & Style
[The non-negotiable code style rules for this codebase.
Only include what Claude cannot infer from reading files.

Example: "Python 3.11+. Type hints on all public functions.
No bare excepts — always catch specific exception types.
f-strings only, no .format() or % formatting. Black-formatted,
line length 88. No print() in src/ — use the logger."]

---

## Error Handling
[How errors are raised and surfaced in this codebase.
Give Claude the exact pattern so it does not invent its own.

Example:
"All service errors raise a subclass of AppError from
src/errors.py. Never raise ValueError or RuntimeError from
a service. The HTTP layer catches AppError and maps it to
a status code.

  class AppError(Exception):
      def __init__(self, code: str, message: str, status: int = 400):
          self.code = code
          self.message = message
          self.status = status"]

---

## Data Access
[How the codebase reads and writes data. This prevents Claude
from writing raw ORM calls where a pattern is expected.

Example:
"All data access goes through repository classes in
src/repositories/. Services receive repositories via
constructor injection — they never instantiate them directly.
Repositories return domain objects, not ORM models.

  Correct:
    class JobService:
        def __init__(self, job_repo: JobRepository): ...

  Wrong — never in a service:
    job = db.session.query(Job).filter_by(id=id).first()"]

---

## Testing
[Test structure and naming so Claude writes tests that fit.

Example: "pytest. Unit tests in tests/unit/, integration in
tests/integration/. Fixtures in conftest.py at the directory
level — never inside the test file itself.
Naming: test_[unit]_[scenario]_[expected_outcome]
Example: test_job_scheduler_full_queue_raises_capacity_error"]

---

## Logging
[How and where logging is done.

Example: "structlog everywhere. Logger instantiated at module
level: logger = structlog.get_logger(__name__)
Always use bound context, never string interpolation:
  logger.info('job.assigned', job_id=job_id, worker=worker_id)"]

---

## New Module Checklist
[What every new file must have before it is complete.

Example:
  [ ] Docstring at top — purpose, inputs, outputs
  [ ] Type hints on all public functions
  [ ] At least one unit test
  [ ] Errors raised as AppError subclasses
  [ ] No logic at module level — functions and classes only
  [ ] Exported via __init__.py if it is a public interface]

---

## Never Use These
[Anti-patterns this codebase has explicitly rejected.
Claude will not reintroduce them if they are listed here.

Example: "No global mutable state at module level."
Example: "No singleton pattern — use dependency injection."
Example: "No time.sleep() in production code — use a queue."
Example: "No hardcoded config values — all config via
environment variables loaded through src/config.py."]
