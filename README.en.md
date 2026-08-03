<table align="center">
  <tr>
    <td align="center"><a href="./README.md">Traditional Chinese</a></td>
    <td align="center"><strong>English</strong></td>
  </tr>
</table>

<h1 align="center">Codex Sol delegation</h1>

When you have configured ChatGPT 5.6 Luna Max Fast but need to temporarily
switch a single conversation to Sol High or Sol xHigh, you need
`codex-sol-the-master`. It provides a clear, auditable two-tier delegation
pattern: exact Chinese or English commands send one complete task to the named
Sol agent, then control returns to the original parent task after completion,
failure, or interruption. It also includes safe-install examples, positive and
negative tests, and return-to-default verification so you can summon deeper
reasoning when needed without permanently changing the parent configuration.

This repository documents a two-level, explicit delegation setup for Codex:

| Exact command | Custom agent | Model | Reasoning |
| --- | --- | --- | --- |
| `call high` | `sol_high` | `gpt-5.6-sol` | `high` |
| `summon xhigh` | `sol_xhigh` | `gpt-5.6-sol` | `xhigh` |

The English aliases are single-task controls. They do not permanently change
the primary model, effort, or speed setting. Matching is literal: do not use
fuzzy, substring, punctuation-normalized, or semantic matching. The two English
aliases must never activate one another.

The English aliases intentionally use `call` for the high tier and `summon`
for the extra-high tier. The explicit `high` and `xhigh` tokens keep the mapping
visible while remaining short.

## Design principles

This setup separates the normal Codex parent task from explicitly summoned Sol
child agents. The parent model, reasoning effort, and speed remain whatever the
Codex App currently selects. A Sol command authorizes only one complete task;
it is not a permanent conversation-wide mode switch.

### The two Sol tiers

- `sol_high` uses `gpt-5.6-sol` with `high` reasoning. It is suited to root-cause
  analysis, targeted fixes, test completion, and ordinary cross-file work.
- `sol_xhigh` uses `gpt-5.6-sol` with `xhigh` reasoning. It is suited to
  architecture tradeoffs, migrations, high-risk changes, complex dependencies,
  and tasks that require an additional validation pass.

`high` and `xhigh` describe reasoning depth, not service speed. Neither Sol
agent may request Fast. When the client does not expose service-tier evidence,
reports must say “not observable” rather than turning an inference into a
verified claim.

### One-task delegation lifecycle

1. The user supplies a complete command with the task, or supplies the command
   alone after a clear unfinished task already exists.
2. The parent creates the named custom agent and passes the task, constraints,
   current progress, file scope, and acceptance criteria.
3. The child performs the work and reports changes, checks, model/effort, and
   any unobservable runtime behavior.
4. When the child completes, fails, reports a blocker, is stopped, or its
   thread closes, that authorization ends and control returns to the parent.
5. A later task needs the full command again. Difficulty, file count, or parent
   uncertainty never activates Sol automatically.

### Exact matching and precedence

Both English aliases use complete literal matching. Quotes, documentation,
translation requests, negations, partial phrases, and general requests for
deeper reasoning are not activation commands. The canonical English aliases are
lowercase and must not be rewritten.

If both tiers are assigned to the same task, use only the higher-priority
`sol_xhigh` so two agents do not duplicate the same work. Separate, clearly
non-overlapping tasks may be assigned to different agents; if the boundary is
unclear, give the complete task to `sol_xhigh`.

### Safety boundaries

This repository is documentation and configuration examples, not an installer.
Before applying the examples, check the Codex version and Agent TOML schema,
back up `$CODEX_HOME`, inspect existing agents and routing blocks, and change
only explicitly authorized files. Never publish API keys, tokens, cookies,
private logs, personal absolute paths, or a private Codex Home.

## What is included

- `SOL_DELEGATION_SETUP_REQUEST.md` — the original setup request preserved as
  supplied. It is an archival snapshot; the current command names are the
  ones in the table above.
- `docs/CONFIGURATION.md` — the English file layout and safe-install guide.
- `docs/TESTING.md` — English positive, negative, and return-to-default tests.
- `docs/CHANGELOG.md` — the English change record.
- `examples/` — sanitized example files; these are templates, not active
  personal configuration.
- `AGENTS.md`, `CONTENT-SPEC.md`, and `CHECKLIST.md` — project operating rules,
  scope, and release checks.

This repository is documentation and configuration examples. It is not a Codex
plugin and it does not automatically modify a user's Codex installation.

## Quick start

1. Read [Configuration](docs/CONFIGURATION.md).
2. Back up the personal Codex home before making changes.
3. Install the examples only after checking the current Codex version and the
   supported agent schema for that version.
4. Start a new task and run the tests in [Testing](docs/TESTING.md).

Use the exact command as an active instruction followed by a harmless,
read-only task. For example:

```text
call high. Read README.md and list three main files. Do not modify any file.
```

The parent task should resume after the delegated child reports completion.
When the client does not expose service-tier telemetry, report that limitation
instead of claiming a tier that was not observed.

## Public repository safety

The setup is deliberately opt-in. Difficulty, importance, file count, or a
request for deeper reasoning is not an activation command. Quoted examples,
documentation, translation requests, and negated instructions are not
activation commands either.

Never publish API keys, tokens, private logs, absolute personal paths, or the
contents of a private Codex home. See [SECURITY.md](SECURITY.md).

## License

The documentation and examples are released under the MIT License. See
[LICENSE](LICENSE).
