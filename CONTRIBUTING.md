# Contributing

Thanks for your interest. This is a small plugin, so please open an issue
before starting anything larger than a fix.

Reports from other panels and firmware versions are especially useful. Please
say which panel and firmware you have, and what worked.

## Setup

You need Node.js and [uv](https://docs.astral.sh/uv/).

```sh
npm ci
npm run build
```

Follow [test selection](AGENTS.md#tests) for local tests and the complete CI gate.
Tests must pass without a real panel. If a change needs real-panel testing, say
what you tested and on which panel and firmware in the pull request.

## Panel protocol changes

The panel protocol lives in
[qolsys-controller](https://github.com/EHylands/QolsysController). Changes to how
the panel is talked to belong there first.
