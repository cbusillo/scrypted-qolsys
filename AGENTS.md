# Qolsys Panel Camera Agent Notes

## What Qolsys Panel Camera does

Qolsys Panel Camera is a Scrypted plugin that exposes a Qolsys IQ Panel's built-in camera as local snapshots and a limited live view.
It uses the local IQ Remote connection, not Alarm.com, and does not provide continuous recording or arm/disarm controls.
Evidence: [README.md](README.md#limits); hardware qualification is limited to the panel and firmware documented there.

## GitHub Workflow

- Treat `main` as a protected no-direct-work branch. Create a focused task
  branch, push it, and open a pull request.
- Use normal merge commits. Do not squash or rebase pull requests.
- Run `npm ci`, selected local tests and `npm run build` before review, and wait
  for the complete `validation` check on the current PR head before ready or merge.
- Delete merged task branches and clean merged worktrees during closeout.

## Tests

Follow the [local/CI test command contract](https://github.com/cbusillo/codex-skills/blob/HEAD/skills/github/references/config-schema.md#workflow-metadata).
Replace `{test_expression}` in `test.local` with a nonempty pytest `-k` expression.
Pairing, panel-state and media-cleanup safety changes use whole affected module
stems, such as `test_panel_client`; trace callers for other affected safety modules.
CI runs the complete unfiltered `test.ci` command. Existing real-panel and release
requirements below still apply.

## Releases

- Never run `npm publish`, create a release tag, or change repository
  visibility without the owner's explicit approval of that exact release.
- Releases publish through `.github/workflows/release.yml` when a GitHub
  release is published. npm trusted publishing authenticates it, so there is
  no npm token. The release tag must be `v` plus the `package.json` version.
  A version already on npm is skipped, not republished.
- Bump `version` in `package.json` and add a `CHANGELOG.md` entry in the same
  pull request as the change it describes.
- A release needs hands-on owner testing against the real panel first.

## Panel Safety

- The plugin talks to a live security panel. Never arm, disarm, trigger alarms,
  or change panel settings other than the camera fields the plugin already
  manages.
- Tests use made-up media and fake transports. Real-panel tests are opt-in,
  run by hand, and are never part of CI.
- Never commit pairing keys, certificates, panel addresses, MAC addresses,
  request IDs from a real panel, photos, or video. Use synthetic values.
- Every snapshot writes to panel flash. Do not add background polling or
  shorten the snapshot interval floor without a reason recorded in the PR.

## Engineering Defaults

- Panel protocol code belongs upstream in
  [qolsys-controller](https://github.com/EHylands/QolsysController). Keep this
  plugin to the Scrypted adapter, live-view chaining and settings. Propose
  reusable protocol changes upstream instead of patching around them here.
- Keep `src/requirements.txt` on a released qolsys-controller version. Do not
  bundle wheels or depend on branches.
- Python targets 3.12, the version Scrypted's Python runtime uses.
