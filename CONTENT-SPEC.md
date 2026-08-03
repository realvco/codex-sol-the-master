# Content specification

## Audience

1. A Codex user who wants explicit, opt-in Sol delegation.
2. An agent maintaining the documentation and examples.
3. A reviewer checking that the public repository contains no private state.

## Canonical behavior

| Command | Agent | Model | Effort | Speed policy |
| --- | --- | --- | --- | --- |
| `call sol high` | `sol_high` | `gpt-5.6-sol` | `high` | Normal/default; never request Fast |
| `summon sol xhigh` | `sol_xhigh` | `gpt-5.6-sol` | `xhigh` | Normal/default; never request Fast |

These are exact active commands. Similar wording, a partial phrase, quoted
text, documentation, examples, or a request for more depth do not activate an
agent. The English aliases are `call sol high` and `summon sol xhigh`; they are
not translations that may be paraphrased at runtime. Traditional Chinese
aliases are maintained in `README.md` and the `.zh-TW.md` companion documents.

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
- `docs/CONFIGURATION.zh-TW.md` and `docs/CONFIGURATION.md` are paired
  configuration guides with reciprocal language links.
- `docs/TESTING.zh-TW.md` and `docs/TESTING.md` are paired testing guides with
  equivalent positive, negative, collision, and return-to-default coverage.
- `docs/CHANGELOG.zh-TW.md` and `docs/CHANGELOG.md` are paired change records.
- English-facing files contain no CJK text; they link to Traditional Chinese
  companions by English labels and paths.
- Both root README pages show only two centered language links at the very top, before the
  title and long-form content: `Traditional Chinese` at approximately 20px and a visibly
  emphasized `English` link at approximately 24px.
- The two text links point to the two README files; image badges are not used as
  the language selector. Use plain anchor text links rather than `h2`/`h3`
  wrappers so GitHub does not add heading hover anchors or a clipped hash icon.
- Language links use `text-decoration: none` so they do not show underlines. The English
  link uses a thin accent border, rounded corners, and padding to make the available English
  documentation obvious. Keep the root README scannable and move detailed procedures into
  `docs/`.

## Change policy

The repository documents a personal configuration pattern; it is not an
installer. Any future installer must be proposed separately with a reversible
dry-run, backup, explicit target paths, and a security review.
