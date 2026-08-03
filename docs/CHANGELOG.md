# Change log

<p align="center"><a href="./CHANGELOG.zh-TW.md">繁體中文</a> · <a href="./CHANGELOG.md">English</a></p>

## Current public contract

The active command names in this repository are:

- `call sol high` → `sol_high`
- `summon sol xhigh` → `sol_xhigh`
- `有請高手處理` → `sol_high`
- `恭請高高手處理` → `sol_xhigh`

The preserved `SOL_DELEGATION_SETUP_REQUEST.md` is the original request
snapshot and predates that wording decision. It remains unchanged so that the
request can be audited, while the examples and current documentation describe
the final names.

The Traditional Chinese companion is [CHANGELOG.zh-TW.md](CHANGELOG.zh-TW.md).

## Documentation status

- `SOL_DELEGATION_SETUP_REQUEST.md` is the preserved original request snapshot.
- `README.md` is the Traditional Chinese primary entry page and `README.en.md`
  is the English companion page.
- `docs/CONFIGURATION.md` and `docs/CONFIGURATION.zh-TW.md` are paired guides.
- `docs/TESTING.md` and `docs/TESTING.zh-TW.md` are paired testing guides.

## Speed and verification principles

`high` and `xhigh` describe reasoning depth, not Fast service. When the client
does not expose service-tier evidence, documentation and test reports must say
that the tier is not observable instead of presenting an inference as verified.
