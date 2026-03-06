# Release Process

This document describes how to create a new release of the Claude Usage Tracker Chrome Extension.

## Prerequisites

- **Git** configured with push access to the repository
- **Google Chrome** for final testing
- **A Chrome Web Store developer account** (one-time $5 registration fee)
- All changes committed and pushed to `main` branch

---

## Release Checklist

### 1. Update Version Number

Edit `chrome-extension/manifest.json`:

```json
{
  "version": "1.1.0"
}
```

> Chrome Web Store requires the version to always increment. You cannot re-upload the same version number.

### 2. Update CHANGELOG.md

Add a new section at the top:

```markdown
## [X.Y.Z] - YYYY-MM-DD

### Added
- New features...

### Changed
- Changes to existing features...

### Fixed
- Bug fixes...
```

### 3. Test Before Release

Load the extension unpacked and verify:

- [ ] Extension loads without errors in `chrome://extensions/`
- [ ] Usage data displays correctly in the popup
- [ ] Badge updates on the toolbar icon
- [ ] Notifications fire at correct thresholds
- [ ] Settings page saves and loads correctly
- [ ] Multi-profile switching works
- [ ] No errors in the service worker console

### 4. Commit Version Changes

```bash
git add chrome-extension/manifest.json CHANGELOG.md
git commit -m "chore: bump version to X.Y.Z"
git push
```

### 5. Create and Push Tag

```bash
git tag -a vX.Y.Z -m "Release vX.Y.Z: Brief description

- Key feature 1
- Key feature 2
- Bug fixes"

git push origin vX.Y.Z
```

### 6. Package the Extension

```bash
# Zip only the chrome-extension folder
zip -r claude-usage-extension-vX.Y.Z.zip chrome-extension/

# Verify the zip contains the right files
unzip -l claude-usage-extension-vX.Y.Z.zip
```

> Make sure the zip does not include any development files, `.DS_Store`, or test files.

### 7. Publish to Chrome Web Store

1. Go to the [Chrome Web Store Developer Dashboard](https://chrome.google.com/webstore/devconsole)
2. Select **Claude Usage Tracker**
3. Click **"Package"** → **"Upload new package"**
4. Upload `claude-usage-extension-vX.Y.Z.zip`
5. Update the store listing description if needed
6. Click **"Submit for review"**

> Review typically takes 1–3 business days for updates.

### 8. Create GitHub Release

1. Go to [Releases](../../releases) → **"Draft a new release"**
2. Select the tag `vX.Y.Z` you pushed
3. Set title: `v X.Y.Z — Brief description`
4. Paste the relevant CHANGELOG.md section into the description
5. Attach `claude-usage-extension-vX.Y.Z.zip`
6. Click **"Publish release"**

### 9. Verify Release

- [ ] GitHub release page shows the correct tag and zip
- [ ] Chrome Web Store listing shows the new version (after review approval)
- [ ] Test installing from the Web Store on a clean Chrome profile

---

## Troubleshooting

### Chrome Web Store rejected the submission
- Check that `manifest.json` has no unsupported fields
- Verify all declared permissions are actually used in code
- Review the rejection reason in the Developer Dashboard — it usually tells you exactly what to fix

### Extension not updating for existing users
- Chrome auto-updates extensions every few hours
- Users can force update via `chrome://extensions/` → **"Update"** button
- Verify the new version number in `manifest.json` is higher than the previous one

### Service worker not loading after update
- Check `background.js` for syntax errors
- Open `chrome://extensions/` → click the service worker link → check Console tab
- Manifest V3 service workers terminate when idle — this is expected behavior

### Zip file issues
- Never zip the parent folder — zip the `chrome-extension/` contents directly
- Avoid including hidden files: `zip -r release.zip chrome-extension/ -x "*.DS_Store" -x "*/.git/*"`

---

## Quick Reference

```bash
# Full release in 5 commands:
# 1. Update version in manifest.json manually, then:
git add chrome-extension/manifest.json CHANGELOG.md
git commit -m "chore: bump version to X.Y.Z"
git tag -a vX.Y.Z -m "Release vX.Y.Z" && git push origin main --tags
zip -r claude-usage-extension-vX.Y.Z.zip chrome-extension/
# Then upload zip to Chrome Web Store + GitHub Releases
```

---

## Version Numbering

Follow [Semantic Versioning](https://semver.org/):

- **Major** (`X.0.0`): Breaking changes, major new features
- **Minor** (`x.Y.0`): New features, backwards compatible
- **Patch** (`x.y.Z`): Bug fixes, minor improvements

> Chrome Web Store compares versions numerically. `1.0.10` is greater than `1.0.9`. Always use clean semver — no build numbers needed.
