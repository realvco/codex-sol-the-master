# Configuration

## Canonical mapping

The public contract is:

```text
有請高手處理  ->  sol_high   ->  gpt-5.6-sol / high
恭請高高手處理  ->  sol_xhigh  ->  gpt-5.6-sol / xhigh
```

The spaces above are visual separators only. The command itself is the exact
Chinese phrase inside the code span, with no required leading or trailing
space.

The first phrase and second phrase differ in more than one meaningful visual
cue: `有請` versus `恭請`, and `高手` versus `高高手`. The distinction is still
enforced by literal matching, not by visual similarity.

## Personal Codex layout

For a user's own Codex installation, the conceptual layout is:

```text
$CODEX_HOME/
├── AGENTS.md
└── agents/
    ├── sol_high.toml
    └── sol_xhigh.toml
```

The repository examples intentionally use `$CODEX_HOME` rather than a real
user path. Copying them into a live installation is a separate, user-authorized
operation. Back up existing files first and check the current Codex agent TOML
schema before installing.

## Agent files

The examples define:

- `sol_high`: model `gpt-5.6-sol`, reasoning effort `high`.
- `sol_xhigh`: model `gpt-5.6-sol`, reasoning effort `xhigh`.

Neither example sets `service_tier = "fast"`. The intended policy is the
normal/default tier and no Fast request. Some clients expose the parent tier,
some expose a child tier, and some expose no tier telemetry. The test report
must preserve that distinction.

## Routing rules

Routing is opt-in and single-task:

1. An exact command must appear as an active user instruction.
2. Delegate the complete current task to the named custom agent.
3. Do not imitate the child in the parent or silently substitute another agent.
4. Wait for the child result, then return control to the parent task.
5. A later task requires the exact command again.
6. If both exact commands are active for one task, use only `sol_xhigh`.

Quoted examples, negations, documentation, translation requests, and partial or
similar phrases are not activation commands.`n