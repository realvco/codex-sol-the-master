<table align="center">
  <tr>
    <td align="center"><h3><a href="./README.md">繁體中文</a></h3></td>
    <td align="center"><h2><a href="./README.en.md">English</a></h2></td>
  </tr>
</table>

<h1 align="center">Codex Sol delegation</h1>

This repository documents a two-level, explicit delegation setup for Codex:

| Exact command | Custom agent | Model | Reasoning |
| --- | --- | --- | --- |
| `call sol high` | `sol_high` | `gpt-5.6-sol` | `high` |
| `summon sol xhigh` | `sol_xhigh` | `gpt-5.6-sol` | `xhigh` |
| `有請高手處理` | `sol_high` | `gpt-5.6-sol` | `high` |
| `恭請高高手處理` | `sol_xhigh` | `gpt-5.6-sol` | `xhigh` |

The commands are single-task controls. They do not permanently change the
primary model, effort, or speed setting. Matching is literal: do not use fuzzy,
substring, punctuation-normalized, or semantic matching. None of the four
commands may activate another command.

The English aliases intentionally use `call` for the high tier and `summon`
for the extra-high tier. The explicit `sol high` and `sol xhigh` tokens keep the
mapping visible while remaining short.

## What is included

- `SOL_DELEGATION_SETUP_REQUEST.md` — the original setup request preserved as
  supplied. It is an archival snapshot; the current command names are the
  ones in the table above.
- `docs/CONFIGURATION.md` — the intended file layout and safe installation
  guidance.
- `docs/TESTING.md` — positive, negative, and return-to-default tests.
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
call sol high. Read README.md and list three main files. Do not modify any file.
```

The parent task should resume after the delegated child reports completion.
When the client does not expose service-tier telemetry, report that limitation
instead of claiming a tier that was not observed.

## Scope and safety

The setup is deliberately opt-in. Difficulty, importance, file count, or a
request for deeper reasoning is not an activation command. Quoted examples,
documentation, translation requests, and negated instructions are not
activation commands either.

Never publish API keys, tokens, private logs, absolute personal paths, or the
contents of a private Codex home. See [SECURITY.md](SECURITY.md).

## License

The documentation and examples are released under the MIT License. See
[LICENSE](LICENSE).
