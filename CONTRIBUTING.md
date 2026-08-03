# Contributing

Keep contributions small, explicit, and evidence-backed.

1. Read `AGENTS.md`, `CONTENT-SPEC.md`, and the relevant document before
   editing.
2. Preserve the exact command strings and the no-fuzzy-matching rule.
3. Use sanitized paths and examples only; never add personal Codex state or
   credentials.
4. Run `CHECKLIST.md`, parse both TOML examples, and inspect the complete diff.
5. Explain observed behavior versus intended behavior in the change summary.

Changes that alter command semantics, agent identity, or speed policy require
an explicit rationale and updated positive/negative tests.`n