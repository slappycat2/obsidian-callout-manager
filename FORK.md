# About this fork

This is a personal fork of [eth-p/obsidian-callout-manager](https://github.com/eth-p/obsidian-callout-manager).
It exists to carry one fix until upstream has it:

- **Clicks do nothing when Settings is open in a popout window** (the default since Obsidian 1.15).
  The Edit and Insert buttons in the callout list and the icon picker used `instanceof`, which is
  always false for elements that live in another window. The fork uses Obsidian's cross-window
  `Node.instanceOf()` instead. Upstream issues: #55, #59.

Everything else is upstream's code, unchanged. The plugin id is still `callout-manager`, so
settings, hotkeys and the plugin API behave exactly as with the official plugin.

## Installing

Use [BRAT](https://github.com/TfTHacker/obsidian42-brat): *Add a beta plugin* →
`slappycat2/obsidian-callout-manager`. BRAT then installs new fork releases when Obsidian starts.

## How it stays up to date

`.github/workflows/sync-release.yml` runs daily (and on every push to `master`):

1. If upstream has published a release that `master` does not contain yet, its tag is merged in.
2. If `master` then differs from the last fork release, the plugin is built and released.

If the merge conflicts, or the fix is no longer present in the build, nothing is released and an
issue is opened here instead. The previously installed fork version keeps working in the meantime.

## Version numbers

Fork releases are numbered `X.Y.(Z*1000+N)`: upstream `1.1.2`, fork revision 1 → `1.1.2001`.

- BRAT only installs a release that is semver-greater than what is installed and only reads three
  numeric parts, so `1.1.2.1` or `1.1.2-fork.1` would not work.
- Obsidian's built-in updater matches plugins by id. With `1.1.2001` installed it does not offer
  the official `1.1.x` releases over the fork, and if an official build is installed anyway, the
  next fork release is still greater, so BRAT puts the fork back.
- Quirk: a fork of an upstream `X.Y.0` release is numbered `X.Y.1`, `X.Y.2`, …

The version is set during the build only; `package.json` and `manifest.json` in the repository
always carry upstream's version.

## Retiring the fork

Once upstream ships the fix: remove the fork from BRAT's beta plugin list and install or update the
official plugin from Community plugins. Settings carry over because the id is the same.
