# Change log

<p align="center"><a href="./CHANGELOG.zh-TW.md">Traditional Chinese</a> · <strong>English</strong></p>

## Current public contract

The active command names in this repository are:

- `call high` → `sol_high`
- `summon xhigh` → `sol_xhigh`

The preserved `SOL_DELEGATION_SETUP_REQUEST.md` is the original request
snapshot and predates that wording decision. It remains unchanged so that the
request can be audited, while the examples and current documentation describe
the final names.

## Documentation status

- `SOL_DELEGATION_SETUP_REQUEST.md` is the preserved original request snapshot.
- `README.en.md` is the English primary entry page.
- `docs/CONFIGURATION.md` is the English configuration guide.
- `docs/TESTING.md` is the English testing guide.

## Speed and verification principles

`high` and `xhigh` describe reasoning depth, not Fast service. When the client
does not expose service-tier evidence, documentation and test reports must say
that the tier is not observable instead of presenting an inference as verified.
