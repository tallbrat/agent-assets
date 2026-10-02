---
name: curator-roadmap
description: "Use when implementing the staged Learning Curator roadmap in personal-os: audit first, preserve deterministic Git/SQLite invariants, work phase-by-phase, and validate each phase with tests, lint, and type checks."
---

# Curator Roadmap

Use this skill when the task is to implement or review the next stages of the Learning Curator system in this repository.

## Baseline

- The current codebase is still the deterministic layer. There are no LLM calls in repo code yet.
- The existing seam for the future agent is [apps/curator-runner/src/curator_runner/ports.py](../../../apps/curator-runner/src/curator_runner/ports.py).
- The authoritative runtime flow and invariants are documented in [AGENTS.md](../../../AGENTS.md), [CLAUDE.md](../../../CLAUDE.md), and [docs/architecture.md](../../../docs/architecture.md).

## Non-Negotiable Rules

- Inspect the existing implementation before editing. Do not reimplement components that already exist.
- Preserve the current architecture unless there is a concrete correctness reason to change it.
- Make changes incrementally in clearly separated phases.
- Do not broaden scope with unrelated refactors.
- Do not weaken the deterministic Git/SQLite invariants.
- Keep repository boundaries explicit: `personal-os` holds agent application code and shared packages; the vault is a separate Git repository; agent assets and skills are a separate Git repository.
- Skill, prompt, or model changes must not trigger bulk re-curation of the vault.

## Execution Order

Implement the roadmap strictly in this order unless the user explicitly narrows the ask:

1. Phase 0 - audit and baseline
2. Phase 1 - deterministic runtime hardening
3. Phase 2 - external agent-assets repository integration
4. Phase 3 - model registry and provider adapters
5. Phase 4 - curator state tools and contracts
6. Phase 5 - Deep Agent integration through `CuratorAgent`
7. Phase 6 - first skills
8. Phase 7 - end-to-end curator flow
9. Phase 8 - validation, recovery, and observability
10. Phase 9 - A2A readiness
11. Phase 10 - cron and VPS deployment refinements

Do not jump to the Deep Agent before the deterministic phases are correct.

## Phase 0 Expectations

- Read the existing docs, workspace `pyproject.toml` files, package READMEs, and the current runner code.
- Run the full existing test suite before changing behavior.
- Verify current Git-engine behavior, state-store schema/migrations, runner transaction boundaries, and the scheduler wrapper.
- Treat code and tests as authoritative if docs drift.
- Record a concise implementation checklist in a developer document before major edits.
- Do not change behavior during the audit unless you are fixing an already failing test caused by current repo code.

## Phase 1 Priorities

When hardening the deterministic runtime, preserve these outcomes:

- User commits determine curation work; actual Git state determines note synchronization.
- Agent-created, agent-renamed, and agent-deleted files must synchronize into `notes` from Git, not from a trusted agent report.
- `notes.origin` means who introduced the note, not who modified it last.
- Renaming or deleting a user-origin note by the agent must fail validation by default.
- `observed_head_sha` and `processed_head_sha` must remain distinct.
- The run lifecycle must preserve forensic failure records.
- No SQLite transaction may remain open while the agent runs.
- Concurrent runs must be prevented with a lock.
- Dirty working trees must fail before agent execution.
- Preserve blob-SHA reconciliation as the authority for history-independent recovery.

## Validation Discipline

At the end of each phase:

- Run the relevant tests for the touched slice first.
- Run the full project checks before declaring the phase complete: `uv run pytest`, `uv run ruff check .`, `uv run ruff format --check .`, and affected per-package `mypy` commands.
- Update the relevant docs.
- Summarize changed files and remaining risks.

## Focused References

- [docs/architecture.md](../../../docs/architecture.md)
- [docs/getting-started.md](../../../docs/getting-started.md)
- [packages/git-engine/README.md](../../../packages/git-engine/README.md)
- [packages/state-store/README.md](../../../packages/state-store/README.md)
- [apps/curator-runner/README.md](../../../apps/curator-runner/README.md)
- [apps/curator-runner/src/curator_runner/run.py](../../../apps/curator-runner/src/curator_runner/run.py)
- [scripts/detect-vault.sh](../../../scripts/detect-vault.sh)
