# Qolsys Panel Camera Agent Notes

## GitHub Workflow

- Treat `main` as a protected no-direct-work branch. Create a focused task
  branch, push it, and open a pull request.
- Use normal merge commits. Do not squash or rebase pull requests.
- Run `npm ci`, `npm test` and `npm run build` before review, and wait for the
  required `validation` check on the current PR head before merge.
- Delete merged task branches and clean merged worktrees during closeout.

## Releases

- Never run `npm publish`, create a release tag, or change repository
  visibility without the owner's explicit approval of that exact release.
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
