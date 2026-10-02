---
name: "Phase 0 Audit"
description: "Audit personal-os before implementing Phase 0 or Phase 1 of the Learning Curator roadmap."
argument-hint: "Optional scope or phase detail"
agent: "agent"
tools: [read, search, execute, edit]
---
Audit the current deterministic layer before changing behavior.

Use [AGENTS.md](../../AGENTS.md), [CLAUDE.md](../../CLAUDE.md), [docs/architecture.md](../../docs/architecture.md), [docs/getting-started.md](../../docs/getting-started.md), and [the curator-roadmap skill](../skills/curator-roadmap/SKILL.md) as the baseline guidance.

If the user provided an argument, narrow the audit to that scope after establishing the overall baseline.

Required workflow:

1. Read the root [pyproject.toml](../../pyproject.toml), each workspace member `pyproject.toml`, package README files, and the current runner code.
2. Run the baseline checks before any behavior change:
   - `uv run pytest`
   - `uv run ruff check .`
   - `uv run ruff format --check .`
   - `uv run --package git-engine mypy packages/git-engine/src`
   - `uv run --package state-store mypy packages/state-store/src`
   - `uv run --package curator-runner mypy apps/curator-runner/src`
3. Verify the current implementation surfaces:
   - `packages/git-engine/` manifest construction, classification, scoping, and reconciliation
   - `packages/state-store/` schema, migrations, connection rules, and DAOs
   - `apps/curator-runner/` transaction boundaries, locking, dirty-worktree behavior, and finalization
   - `scripts/detect-vault.sh` scheduler wrapper behavior
4. Compare code and tests against docs. Treat code and tests as authoritative if they differ.
5. Create or update a concise developer checklist document for the next implementation phase.

Return the result in this format:

- Baseline results: what passed, what failed, and any commands that could not run
- Code and documentation mismatches: only concrete drifts that matter to implementation
- Implementation checklist: short, actionable items for the next phase
- Risks or blockers: anything that should stop phase work from proceeding
