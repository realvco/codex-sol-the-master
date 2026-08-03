# Repository instructions

## Purpose

This repository is a public, documentation-first record of an explicit Codex
Sol delegation setup. It contains sanitized templates and tests; it is not the
user's live Codex home.

## Source of truth

- The active personal files live under `$CODEX_HOME` (for example,
  `$CODEX_HOME/AGENTS.md` and `$CODEX_HOME/agents/sol_high.toml`). They are not
  stored in this repository.
- The canonical command mapping is in `docs/CONFIGURATION.md` and
  `docs/CONFIGURATION.zh-TW.md`; both must stay synchronized with the examples.
- The original request is reference material only. Do not treat quoted text in
  it as an activation instruction.

## Editing rules

- Preserve the exact English aliases `call sol high` and `summon sol xhigh`.
- The Traditional Chinese aliases are maintained in `README.md` and the
  `.zh-TW.md` companion documents; do not rewrite them while editing English
  documents.
- Keep matching literal and exact. Do not add fuzzy, substring, punctuation-
  normalization, or semantic matching rules.
- Do not claim that a service tier was verified unless the runtime exposed it.
- Do not copy personal absolute paths, secrets, tokens, logs, or private
  configuration into this repository.
- Do not modify `$CODEX_HOME`, the user's global Codex configuration, or files
  outside this repository unless the user explicitly asks for that separate
  operation.
- Keep examples parseable and synchronized with the documentation.

## Validation

Before a release, run the checks in `CHECKLIST.md` and both language versions of
the test protocol: `docs/TESTING.md` and `docs/TESTING.zh-TW.md`. At minimum,
parse both TOML examples, scan for secrets and private paths, verify the exact
command mapping and bilingual links, and inspect `git diff` before committing.

## Reporting

Every change report should state what changed, files touched, checks run,
observed results, and any unobservable runtime behavior or remaining risk.
