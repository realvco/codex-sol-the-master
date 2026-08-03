# Security policy

Do not publish secrets or private runtime state. In particular, never commit
API keys, access tokens, cookies, private logs, raw telemetry, credentials,
personal absolute paths, or a complete private `$CODEX_HOME` directory.

The TOML files in `examples/` are intentionally sanitized templates. They do
not contain credentials and do not install anything by themselves.

If sensitive material is found:

1. Stop publication immediately.
2. Remove it from the working tree and Git history as appropriate.
3. Rotate the affected credential.
4. Report the incident privately to the repository owner rather than opening a
   public issue.

For a suspected security issue in this project, contact the repository owner
through a private GitHub channel. Do not disclose exploitable details publicly
until remediation is coordinated.
