# Testing and return to default

Run these tests in a new Codex task with a harmless read-only request. Record
the client version and the evidence shown by the client.

## 1. Baseline

Before using a delegation command:

1. Run `/status` (or the client's equivalent).
2. Record the parent model, reasoning effort, speed/tier, and task identity.
3. Use a read-only task so a routing failure cannot damage project files.

## 2. Positive tests

Use each exact command as an active instruction in separate tasks:

```text
有請高手處理。請讀取 README.md 並回報標題，不要修改任何檔案。
```

```text
恭請高高手處理。請讀取 README.md 並回報標題，不要修改任何檔案。
```

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

## 3. Negative and collision tests

Run these without an exact active command and verify that no custom agent is
spawned:

- `有請高手處理嗎？` as a quoted question, not an instruction;
- a sentence containing only `高手處理`;
- a sentence containing only `高高手處理`;
- `call sol high?` as a question, not an instruction;
- a sentence containing only `sol high` or `sol xhigh`;
- a paraphrase such as “請用更深的推理”；
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
