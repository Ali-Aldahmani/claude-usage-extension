# Contributing to Claude Usage Tracker — Chrome Extension

First off, thank you for considering contributing! 🎉

This document provides guidelines for contributing to the Chrome extension. We welcome all kinds of contributions: bug reports, feature requests, documentation improvements, and code contributions.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Development Setup](#development-setup)
  - [Project Structure](#project-structure)
- [How to Contribute](#how-to-contribute)
  - [Reporting Bugs](#reporting-bugs)
  - [Suggesting Features](#suggesting-features)
  - [Contributing Code](#contributing-code)
- [Development Guidelines](#development-guidelines)
  - [Code Style](#code-style)
  - [Architecture](#architecture)
  - [Commit Messages](#commit-messages)
  - [Branch Naming](#branch-naming)
- [Pull Request Process](#pull-request-process)
- [Release Process](#release-process)
- [Getting Help](#getting-help)

---

## Code of Conduct

This project adheres to a code of conduct that all contributors are expected to follow. Please be respectful, inclusive, and considerate in all interactions.

**Our Standards:**
- Be welcoming and inclusive
- Be respectful of differing viewpoints
- Accept constructive criticism gracefully
- Focus on what's best for the community
- Show empathy towards other community members

---

## Getting Started

### Prerequisites

Before you begin, ensure you have the following:

- **Google Chrome** (latest stable)
- **Git** for version control
- **A code editor** — VS Code recommended
- **A Claude AI account** for testing (Free, Pro, or API)
- **Node.js** (optional, for any build tooling)

### Development Setup

1. **Fork the repository**

   Click the "Fork" button on GitHub to create your own copy.

2. **Clone your fork**
   ```bash
   git clone https://github.com/Ali-Aldahmani/claude-usage-extension.git
   cd claude-usage-extension
   ```

3. **Add upstream remote**
   ```bash
   git remote add upstream https://github.com/Ali-Aldahmani/claude-usage-extension.git
   ```

4. **Load the extension in Chrome**
   - Open Chrome and navigate to `chrome://extensions/`
   - Enable **Developer mode** (top right toggle)
   - Click **"Load unpacked"**
   - Select the `chrome-extension/` folder

5. **Verify it works**
   - Make sure you're logged into `claude.ai`
   - Click the extension icon in the toolbar
   - You should see your live usage data immediately

6. **Make changes and reload**
   - Edit files inside `chrome-extension/`
   - Go to `chrome://extensions/` and click the **reload** button on the extension
   - For `background.js` changes, click the service worker link to inspect it

### Project Structure

```
chrome-extension/
├── manifest.json        # Manifest V3 config and permissions
├── background.js        # Service worker — polling, badge updates, notifications
├── api.js               # Claude.ai API integration + org ID discovery
├── storage.js           # Profile and settings persistence
├── popup.html           # Toolbar popup shell
├── popup.js             # Popup logic — render data, profile switcher
├── popup.css            # Popup styles — dark/light mode, progress bars
├── settings.html        # Full settings page
├── settings.js          # Settings logic — profiles, thresholds, intervals
└── icons/               # PNG icons — 16, 32, 48, 128px
```

---

## How to Contribute

### Reporting Bugs

Before submitting a bug report:
1. Check existing [issues](../../issues) to avoid duplicates
2. Ensure you're running the latest version

**When reporting a bug, include:**
- Chrome version (Help → About Google Chrome)
- Extension version (found in `chrome://extensions/`)
- Steps to reproduce the issue
- Expected behavior vs. actual behavior
- Screenshots if applicable
- Console errors (right-click extension popup → Inspect → Console tab)

### Suggesting Features

We love feature suggestions! Please:
1. Check existing issues and discussions first
2. Describe the problem your feature would solve
3. Explain your proposed solution
4. Consider alternative approaches

### Contributing Code

1. **Find or create an issue** for what you want to work on
2. **Comment on the issue** to let others know you're working on it
3. **Fork and create a branch** (see [Branch Naming](#branch-naming))
4. **Make your changes** following our [guidelines](#development-guidelines)
5. **Test thoroughly** in Chrome with a real Claude account
6. **Submit a pull request**

---

## Development Guidelines

### Code Style

We follow standard JavaScript best practices for Chrome extensions (Manifest V3).

**Key conventions:**

```javascript
// Use descriptive names
async function fetchUsageData(sessionKey, orgId) { ... }  // ✅ Good
async function getData() { ... }                           // ❌ Avoid

// Always handle errors gracefully
try {
  const data = await fetchUsageData(key, orgId);
} catch (error) {
  console.error('[claude-usage] fetch failed:', error);
}

// Use async/await over callbacks
const result = await chrome.storage.local.get('profiles');  // ✅ Good

// Use const/let, never var
const REFRESH_INTERVAL = 60;   // ✅ Good
var refreshInterval = 60;      // ❌ Avoid

// Group constants at the top of each file
const API_BASE = 'https://claude.ai/api';
const DEFAULT_REFRESH = 60;
const THRESHOLDS = [75, 90, 95];
```

**Chrome Extension specific:**

```javascript
// Always use chrome.storage, never localStorage
await chrome.storage.local.set({ profiles });       // ✅ Good
localStorage.setItem('profiles', JSON.stringify()); // ❌ Avoid

// Send messages between popup and background correctly
chrome.runtime.sendMessage({ type: 'REFRESH' });

// Always check for errors in chrome API callbacks
chrome.tabs.query({}, (tabs) => {
  if (chrome.runtime.lastError) {
    console.error(chrome.runtime.lastError);
    return;
  }
});
```

### Architecture

The extension follows a clear separation of responsibilities:

- **`background.js`** (Service Worker): All API calls, badge updates, alarm scheduling, and notifications. The single source of truth for data.
- **`popup.js`** (UI): Reads cached data from `chrome.storage` only — never calls the API directly. Renders fast.
- **`api.js`** (Shared): Pure functions for fetching usage from Claude's API. No side effects.
- **`storage.js`** (Shared): All reads and writes to `chrome.storage.local`. Single interface for persistence.

**Guidelines:**
- Popup never calls the API — always reads from storage cache
- All API calls go through `background.js` only
- Keep functions small and single-purpose
- Comment anything non-obvious, especially Chrome API quirks

### Commit Messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Formatting, no logic change
- `refactor`: Code change that neither fixes a bug nor adds a feature
- `perf`: Performance improvement
- `test`: Adding or updating tests
- `chore`: Build process, dependencies, etc.

**Examples:**
```
feat(popup): add Opus usage card with progress bar

fix(background): resolve badge not updating after session reset

docs(readme): add Chrome Web Store installation instructions

refactor(storage): consolidate profile read/write into storage.js
```

### Branch Naming

Use descriptive branch names with prefixes:

| Prefix | Use Case | Example |
|---|---|---|
| `feat/` | New features | `feat/usage-history-chart` |
| `fix/` | Bug fixes | `fix/badge-not-updating` |
| `docs/` | Documentation | `docs/add-permissions-guide` |
| `refactor/` | Code refactoring | `refactor/extract-api-module` |
| `chore/` | Maintenance | `chore/update-icons` |

---

## Pull Request Process

1. **Update your fork**
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Create your branch**
   ```bash
   git checkout -b feat/your-feature-name
   ```

3. **Make your changes**
   - Write clean, documented code
   - Follow the style guidelines
   - Test in Chrome with a real Claude account

4. **Commit your changes**
   ```bash
   git add .
   git commit -m "feat(scope): description of changes"
   ```

5. **Push to your fork**
   ```bash
   git push origin feat/your-feature-name
   ```

6. **Open a Pull Request**
   - Use a clear, descriptive title
   - Reference any related issues (`Closes #123`)
   - Describe what changed and why
   - Include screenshots for any UI changes
   - List any breaking changes

7. **Code Review**
   - Respond to feedback promptly
   - Make requested changes
   - Keep the PR focused — one feature or fix per PR

**PR Checklist:**
- [ ] Code follows project style guidelines
- [ ] Self-reviewed my own code
- [ ] Added comments for complex logic
- [ ] Updated documentation if needed
- [ ] Tested in Chrome with a real Claude account
- [ ] No console errors or warnings
- [ ] UI changes include screenshots

---

## Release Process

```bash
# 1. Update version in manifest.json
# "version": "1.0.0" → "1.1.0"

# 2. Update CHANGELOG.md with what's new

# 3. Commit and tag
git commit -am "chore: bump version to X.Y.Z"
git tag vX.Y.Z
git push origin main --tags

# 4. Zip the extension folder for Chrome Web Store
zip -r claude-usage-extension-vX.Y.Z.zip chrome-extension/

# 5. Upload to Chrome Web Store Developer Dashboard
# 6. Publish GitHub release with the zip attached
```

---

## Getting Help

- **Questions?** Open a [Discussion](../../discussions)
- **Found a bug?** Open an [Issue](../../issues)
- **Debugging tips:** Right-click the popup → Inspect, or go to `chrome://extensions/` → service worker link for background logs

---

## Recognition

Contributors are recognized in:
- The GitHub contributors graph
- Release notes for significant contributions
- README acknowledgments for major features

Thank you for helping make Claude Usage Tracker better for everyone! 🙏
