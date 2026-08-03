# Sanitized global routing example

This is a template for the user's personal `$CODEX_HOME/AGENTS.md`. It is not
automatically loaded from this repository.

<!-- BEGIN SOL DELEGATION ROUTING -->

## Explicit Sol delegation commands

Use literal, exact matching only:

- `call sol high` delegates the current task to `sol_high` (`gpt-5.6-sol`,
  `high`).
- `summon sol xhigh` delegates the current task to `sol_xhigh` (`gpt-5.6-sol`,
  `xhigh`).

This English template intentionally contains only the English aliases.

Do not match substrings, punctuation-normalized variants, semantic similarities,
quoted examples, or negated instructions. Do not request Fast service tier. The
delegation ends when the child returns; the parent configuration is unchanged.

<!-- END SOL DELEGATION ROUTING -->
