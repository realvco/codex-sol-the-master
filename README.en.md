<h1 align="center">Codex Sol delegation</h1>

<p align="center">
  <a href="./README.md"><img alt="繁體中文主版本" src="https://img.shields.io/badge/%E7%B9%81%E9%AB%94%E4%B8%AD%E6%96%87-%E4%B8%BB%E7%89%88%E6%9C%AC-1f6feb?style=for-the-badge"></a>
  <a href="./README.en.md"><img alt="English version" src="https://img.shields.io/badge/English-README.en.md-6f42c1?style=for-the-badge"></a>
</p>

Traditional Chinese is the primary project language. Read the [Traditional
Chinese README](README.md) first; this file is the English version.

This repository documents a two-level, explicit delegation setup for Codex:

| Exact command | Custom agent | Model | Reasoning |
| --- | --- | --- | --- |
| `有請高手處理` | `sol_high` | `gpt-5.6-sol` | `high` |
| `恭請高高手處理` | `sol_xhigh` | `gpt-5.6-sol` | `xhigh` |

The commands are single-task controls. They do not permanently change the
primary model, effort, or speed setting. Matching is literal: do not use fuzzy,
substring, punctuation-normalized, or semantic matching. The two commands must
never activate one another.

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
有請高手處理。請讀取 README.md，列出本專案的三個主要檔案，不要修改任何檔案。
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
