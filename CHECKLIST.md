# Release checklist

## Content

- [ ] README and Chinese README agree on the two exact commands.
- [ ] `README.md` is the Traditional Chinese primary page and `README.en.md` is
      the English companion page.
- [ ] Both README files expose the same clickable language selector near the
      top and link to each other.
- [ ] `docs/CONFIGURATION.md` and both TOML examples agree on agent names,
      model, and effort.
- [ ] The original request remains intact and is clearly marked as reference
      material.
- [ ] No unsupported service-tier value is presented as a guaranteed runtime
      setting.

## Safety

- [ ] No API key, token, cookie, private log, or secret appears in tracked
      files.
- [ ] No personal absolute path such as `C:\Users\<user>\.codex` appears in
      public examples.
- [ ] Live personal Codex files were not copied into the repository.
- [ ] The diff contains only intended project files.

## Technical QA

- [ ] `sol_high.toml` parses with Python `tomllib`.
- [ ] `sol_xhigh.toml` parses with Python `tomllib`.
- [ ] Exact-match collision checks pass.
- [ ] Positive activation tests pass for both commands.
- [ ] Negative near-match tests do not activate either agent.
- [ ] Parent `/status` or equivalent confirms return to the normal parent task.
- [ ] Service-tier claims are limited to observable evidence.
- [ ] `git status` and `git diff --check` are clean apart from intended changes.
