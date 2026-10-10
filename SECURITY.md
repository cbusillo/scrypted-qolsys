# Security Policy

## Supported Versions

Only the latest release on npm is supported. Fixes ship in a new release built
from `main`.

## Reporting a Vulnerability

Report suspected vulnerabilities privately through GitHub's
[Report a vulnerability](https://github.com/cbusillo/scrypted-qolsys/security/advisories/new)
form. Do not open a public issue for a vulnerability.

Include the plugin version, panel model and firmware, the impact, and the
smallest steps that reproduce it.

Do not send pairing keys, certificates, panel addresses, photos, video, or
other personal data. Use redacted or made-up values.

This is a single-maintainer project. Reports are handled on a best-effort
basis, and I aim to reply within seven days.

## Scope

Relevant reports include:

- the plugin exposing panel pairing keys, photos or video to anyone other than
  the Scrypted user;
- the plugin changing panel state beyond the camera fields it manages;
- unsafe handling of data the panel sends back; and
- dependency or GitHub Actions supply-chain problems.

Problems in the panel's own firmware or in Alarm.com should go to Qolsys or
Alarm.com. Problems in the panel protocol library belong in
[qolsys-controller](https://github.com/EHylands/QolsysController/issues).
