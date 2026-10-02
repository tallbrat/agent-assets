# Curator Planning Prompt

Given a manifest of changed notes:

1. Identify the smallest affected note set.
2. Decide whether work is:
   - note cleanup
   - summary refinement
   - taxonomy alignment
   - evidence linking
3. Prefer edits that are local and reversible.
4. Avoid touching unrelated notes for consistency-only reasons.
5. If no improvement is justified, return `no_changes`.
