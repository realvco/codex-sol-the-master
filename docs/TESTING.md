# Testing and return to default

<p align="center"><a href="./TESTING.zh-TW.md">Traditional Chinese</a> · <mark><strong>English</strong></mark></p>

Run these tests in a new Codex task with a harmless read-only request. Record
the client version and the evidence shown by the client.

## 0. Preflight and evidence rules

Before testing, confirm the active Codex executable, `$CODEX_HOME`, the custom
agent directory, and the client version. Use a fresh task so an old prompt or
cached routing block cannot be mistaken for the current configuration.

For every positive test, record the task identity, actual child agent, model,
reasoning effort, and service-tier status when the client exposes them. A
missing tier signal is recorded as “service tier not observable”; it is never
converted into a claim of Standard or Fast.

## 1. Baseline

Before using a delegation command:

1. Run `/status` (or the client's equivalent).
2. Record the parent model, reasoning effort, speed/tier, and task identity.
3. Use a read-only task so a routing failure cannot damage project files.

## 2. Positive tests

Use each exact command as an active instruction in separate tasks:

```text
call sol high. Read README.md and report its title. Do not modify any file.
```

```text
summon sol xhigh. Read README.md and report its title. Do not modify any file.
```

Expected evidence:

- the child is the actual custom agent `sol_high` or `sol_xhigh`;
- the model is `gpt-5.6-sol`;
- the effort is `high` or `xhigh` respectively;
- Fast is not requested;
- service-tier status is reported only if visible.

The positive result must show the actual custom child, not a parent response
that merely imitates Sol. The parent should wait for the child result before
presenting the task outcome.

## 3. Negative and collision tests

Run these without an exact active command and verify that no custom agent is
spawned:

- `call sol high?` as a question, not an instruction;
- a sentence containing only `sol high` or `sol xhigh`;
- a paraphrase such as “use deeper reasoning”;
- a documentation or translation request that mentions either phrase.

The routing guard is literal. Do not “correct” near matches into an exact
command.

## 4. Multiple commands in one task

If multiple exact commands are actively assigned to the same task, expected
policy is to use only `sol_xhigh`, not to run both agents on the same work.

## 5. Return-to-default verification

After the child reports completion:

1. Wait for the child result to be visibly returned to the parent task.
2. Run `/status` again in the parent.
3. Confirm the parent task is still the original task and no custom agent remains
   active.
4. Start a fresh task without either command and confirm it uses the normal
   app-selected parent configuration.

This verifies scope is single-task. It does not prove that every future client
will expose the same tier telemetry. If the client shows no tier, record
“service tier not observable” rather than inferring Standard or Fast.

## 6. Optional parent-speed isolation

If the client exposes enough telemetry, repeat the positive tests while the
parent task is set to Fast. Confirm that the child does not silently request or
inherit Fast, then confirm that the parent returns to its original setting after
the child ends. If the client cannot show independent child speed, record that
as an unresolved observability limit rather than altering the user's global App
setting.

## 7. Evidence report template

Use this compact record for each run:

```text
Client/version:
Parent task and configuration:
Exact command:
Actual child agent:
Model:
Reasoning effort:
Service tier or Fast status:
Files changed:
Checks performed:
Return-to-default result:
Unobservable behavior or blockers:
```
