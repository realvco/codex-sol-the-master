# Configuration

<p align="center"><a href="./CONFIGURATION.zh-TW.md">Traditional Chinese</a> · <mark><strong>English</strong></mark></p>

## Canonical mapping

The public contract is:

```text
call sol high     ->  sol_high   ->  gpt-5.6-sol / high
summon sol xhigh  ->  sol_xhigh  ->  gpt-5.6-sol / xhigh
```

The spaces above are visual separators only. The command itself is the exact
phrase inside the code span, with no required leading or trailing space. The
English aliases are exactly `call sol high` and `summon sol xhigh`.

The two English commands use different verbs and tier tokens. The distinction
is enforced by literal matching, not by visual similarity or semantic inference.

## Choosing the tier

Use `sol_high` for a bounded implementation or diagnosis where the task still
has a clear shape: reproduce a bug, trace a root cause, patch a few related
files, add tests, or review a focused change.

Use `sol_xhigh` when the task has interacting architectural decisions,
migrations, broad dependency effects, high-risk data changes, or unresolved
tradeoffs. It is also the fallback when both tiers are named for one task or
when the task boundary between tiers is unclear.

The tier is not a quality promise and it is not a speed switch. The parent
agent remains the default owner of the conversation, and each Sol child must
return control after the one delegated task ends.

## Safe installation sequence

Before copying an example into a live Codex home:

1. Identify the active `$CODEX_HOME` and record `codex --version`.
2. Check the current Agent TOML schema and whether custom agents are enabled.
3. Inspect existing `AGENTS.md`, `AGENTS.override.md`, `agents/`, and routing
   markers; do not overwrite an unrelated block.
4. Back up every target file and keep the backup outside the public repository.
5. Apply only the smallest authorized diff, preserving all unrelated settings.
6. Start a new Codex task and run the positive, negative, collision, and
   return-to-default tests in [Testing](TESTING.md).

Do not add a guessed `service_tier = "standard"`. If the current client does
not publish a verified standard-tier identifier or a local Fast-off mechanism,
leave the field absent and report the limitation. Never claim Standard merely
because Fast is not visible.

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
similar phrases are not activation commands.

## Cases that must not route

Do not activate a child when the command is being explained, quoted, translated,
edited, negated, or mentioned only as a partial phrase. A difficult task by
itself is not authorization. Neither is a request for a stronger model, deeper
analysis, faster output, or “use Sol” without one of the four exact commands.

If a command is present but there is no clear unfinished task, do not create an
empty child. Ask the user to provide the task in the same message instead.

## Failure and recovery

If the requested custom agent cannot be spawned, the model or effort cannot be
selected, or the required speed boundary cannot be verified, report the actual
failure. Do not silently substitute another agent, lower the reasoning effort,
enable Fast, or claim that the child completed work it did not perform.

After completion, failure, interruption, or blocker reporting, the command's
authorization ends. A later task must use the full command again and should be
handled by the App-selected parent configuration.
