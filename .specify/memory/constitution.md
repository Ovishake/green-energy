<!--
SYNC IMPACT REPORT (scratch material, remove before committing)

Version change: 1.0.0 → 1.1.0
Bump rationale: MINOR. A new principle (VI) is added and Principle V is materially
expanded to reference the mandated runtime dependencies. No principle is removed or
redefined in a backward-incompatible way.

Modified principles:
- V. Simplicity and Minimal Dependencies → V. Simplicity and Minimal Dependencies
  (dependency rule now cross-references Principle VI; "empty dependency list" statement
  removed because it is no longer accurate)

Added principles:
- VI. Typed Agents, Quality Gates, and Coverage

Added sections:
- None

Removed sections:
- None

Deferred items / TODOs:
- None. The `agents.yaml` location (repository root) and the test runner/coverage tool
  are not named by the amendment; they are left to the plan/tasks for the first feature.
-->

# green-energy Constitution

## Core Principles

### I. Python Package First
The project MUST be a Python package under `src/green_energy/`, built with the
backend declared in `pyproject.toml`. All functionality MUST be importable as
Python code before it is exposed through any entry point. Logic MUST NOT live
only in the console script wrapper.

### II. CLI Interface
The `green-energy` console script is the primary user interface. Commands MUST
read input from arguments or stdin and write results to stdout. Errors and
diagnostics MUST go to stderr. Output MUST support a human-readable format by
default and a JSON format when requested, so other tools can consume it.

### III. Test Coverage for Behavior
Any change to scheduling, timing, or recommendation logic MUST be accompanied by
automated tests that cover the changed behavior. A change MUST NOT be merged while
its tests fail. Tests SHOULD be written before the implementation they verify.

### IV. Transparent Recommendations
The tool advises when to run appliances, so every recommendation MUST be
explainable. Output MUST state the data or assumptions it is based on (for
example, the energy-source data used and the time zone). When data is missing or
stale, the tool MUST say so rather than produce a silent best guess.

### V. Simplicity and Minimal Dependencies
Solutions MUST start with the simplest approach that meets the requirement
(YAGNI). Runtime dependencies MUST be limited to those mandated by Principle VI
(Pydantic and PydanticAI) plus any further dependency that is justified in the pull
request that adds it. Each new dependency MUST be justified against the standard
library first.

### VI. Typed Agents, Quality Gates, and Coverage
- All AI agents MUST be built with PydanticAI. All data models, validation, and
  structured configuration MUST use Pydantic. No other agent framework or
  validation library MAY be introduced.
- Agent definitions MUST be kept in `agents.yaml` at the repository root. Agent
  behavior MUST NOT be hard-coded in Python; the file MUST be loaded and validated
  into Pydantic models before use.
- Code MUST be formatted and linted with Ruff. `ruff format` and `ruff check` MUST
  pass before a pull request is merged.
- The automated test suite MUST achieve at least 85% line coverage of
  `src/green_energy/`. A test run below 85% MUST be treated as a failing run.
- Code MUST be the minimum needed for the current objective. Speculative
  abstractions, unused options, and unrequested features MUST NOT be added.

## Technology and Packaging Constraints

- The minimum supported Python version is 3.12, as declared in `pyproject.toml`.
- Dependencies MUST be managed with `uv`; `pyproject.toml` is the single source of
  truth for metadata and dependencies.
- Source code MUST use the `src/` layout. Tests MUST live outside `src/`.
- Generated artifacts (for example `__pycache__/`) MUST NOT be committed.

## Development Workflow

- Non-trivial features MUST follow the Spec Kit sequence: specification, plan,
  tasks, then implementation. Each step SHOULD be reviewed before the next begins.
- Every pull request MUST be reviewed against this constitution before merge. The
  reviewer MUST confirm that Principles I–VI and the constraints above are met.
- Pull requests MUST keep changes focused on one purpose. Unrelated refactors MUST
  go in a separate pull request.
- The `main` branch MUST remain runnable. Work happens on feature branches.

## Governance

This constitution supersedes all other development practices in this repository.
Where another document conflicts with it, this constitution prevails until it is
amended.

**Amendment procedure**: An amendment is proposed as a pull request that edits
`.specify/memory/constitution.md` and includes a Sync Impact Report explaining the
change. It MUST be approved by the project maintainer before merge. Dependent
Spec Kit templates and commands MUST be checked for consistency with any amended
principle in the same pull request.

**Versioning policy**: The constitution follows semantic versioning.
- MAJOR: a principle or governance rule is removed or redefined in a
  backward-incompatible way.
- MINOR: a principle or section is added, or existing guidance is materially
  expanded.
- PATCH: wording, typo, or non-semantic clarification only.

**Compliance review**: Every pull request MUST be checked for compliance during
review. Any added complexity MUST be justified against Principles V and VI. A review
MAY be re-run at any time on request, and the maintainer SHOULD review compliance
at least once per release.

**Version**: 1.1.0 | **Ratified**: 2026-10-26 | **Last Amended**: 2026-10-26
