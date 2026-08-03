# Content specification

## Audience

1. A Codex user who wants explicit, opt-in Sol delegation.
2. An agent maintaining the documentation and examples.
3. A reviewer checking that the public repository contains no private state.

## Canonical behavior

| Command | Agent | Model | Effort | Speed policy |
| --- | --- | --- | --- | --- |
| `有請高手處理` | `sol_high` | `gpt-5.6-sol` | `high` | Normal/default; never request Fast |
| `恭請高高手處理` | `sol_xhigh` | `gpt-5.6-sol` | `xhigh` | Normal/default; never request Fast |

These are exact active commands. Similar wording, a partial phrase, quoted
text, documentation, examples, or a request for more depth do not activate an
agent.

## Operating modes

- **Audit:** inspect current files and runtime evidence without changing state.
- **Plan:** identify the smallest safe set of changes and acceptance checks.
- **Build:** edit only authorized files in this repository.
- **QA:** parse, scan, diff, and run the activation/return tests.

## Evidence policy

Distinguish intended configuration from observed runtime behavior. Record the
Codex version, agent identity, model, reasoning effort, and service tier only
when each is actually observable. A missing tier signal is an explicit
limitation, not permission to infer Standard or Fast.

## README presentation

- `README.md` is the primary Traditional Chinese entry page.
- `README.en.md` is the English companion page.
- Both pages show only two large, centered text links — `繁體中文` and
  `English` — at the very top, before the title and long-form content.
- The two text links point to the two README files; image badges are not used as
  the language selector.
- Keep the root README scannable and move detailed procedures into `docs/`.

## Change policy

The repository documents a personal configuration pattern; it is not an
installer. Any future installer must be proposed separately with a reversible
dry-run, backup, explicit target paths, and a security review.
