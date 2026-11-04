# Nuke Shortcut Editor: PySide6 / Nuke 17 compatibility patch

Community Nuke 17 compatibility contribution submitted by **[@omiddabir](https://github.com/omiddabir)**. This is a community compatibility patch to [Ben Dickson's Shortcut Editor for Nuke](https://github.com/dbr/shortcuteditor-nuke), originally released under GPL-2.0. This is **not** an official Foundry or upstream release.

## Supported/tested environment

- User-tested: **NukeX 17.1v1, Windows**, 4 November 2026.
- Confirmed by user: shortcut editor GUI opens, shortcut edits save and survive reopening, and no reported startup error.
- Independently verified: Python syntax and mocked Nuke/Qt6 logic for shortcuts, JSON round-trip, and duplicate-key resolution.
- **Not yet verified**: Nuke 16.x, Nuke 17 builds other than 17.1v1, macOS, Linux, or PySide2/PySide legacy installs. This patch intentionally targets PySide6.

## Install

1. Back up existing `~/.nuke/shortcuteditor.py` and `~/.nuke/shortcuteditor_settings.json`.
2. Copy `shortcuteditor.py` from this folder to `~/.nuke/shortcuteditor.py` (Windows: `C:\Users\<username>\.nuke\shortcuteditor.py`).
3. Add this near the **end** of `~/.nuke/menu.py` if not already present:

```python
try:
    import shortcuteditor
    shortcuteditor.nuke_setup()
except Exception:
    import traceback
    traceback.print_exc()
```

4. Relaunch Nuke and use **Edit > Edit keyboard shortcuts**.
5. Verify by editing a shortcut, closing the dialog, and relaunching Nuke. Keep the backup until verified.

Do **not** distribute your private `shortcuteditor_settings.json`; the plugin stores your key overrides there.

## Main changes

- Import PySide6 from the Nuke 16+ Qt6 environment.
- Update Qt6 enums and flag arithmetic for key combinations.
- Update QKeySequence construction and portable-text comparisons.
- Update Qt6 dialog APIs and screen-positioning API.
- Fix duplicate shortcut matching to compare normalized shortcut strings.
- Preserve version-1 JSON settings format.

## Contributing upstream

Proposed workflow: fork the [original repository](https://github.com/dbr/shortcuteditor-nuke), create a branch, incorporate and review this patch against the latest upstream `master`, then open a PR. This bundle was built from a user-provided copy of the original `shortcuteditor.py`; check and resolve any upstream differences before submitting.

The original repository describes the latest published release as v1.3 (2021); the internal version string of the supplied file was `1.2` and the patched source uses `1.2-nuke17-qt6.1`. Do not imply this is a new official upstream release.

## Licensing

Original by Ben Dickson and contributors, GPL-2.0. Changes distributed under the same GPL-2.0 license. Retain the original notices and credit. See `LICENSE`.