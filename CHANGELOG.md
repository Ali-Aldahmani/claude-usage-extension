# Changelog

All notable changes to Claude Usage Tracker — Chrome Extension will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Planned
- API console usage tracking (Anthropic API key support)
- Firefox / Edge extension ports
- Usage history charts and trends
- Export usage data to CSV

---

## [1.0.0] - 2026-XX-XX

### Added — Chrome Extension (this fork)

This release introduces a fully functional Chrome extension under `chrome-extension/`, bringing the core features of the original macOS menu bar app to all platforms — Windows, macOS, and Linux — through the browser toolbar.

#### Zero-Config Authentication
- Auto-reads `sessionKey` cookie directly from `claude.ai` — no manual extraction needed
- Fallback to manual session key entry in Settings for advanced users
- Credentials stored securely in `chrome.storage.local` (encrypted by Chrome, sandboxed to this extension only)
- Graceful handling of expired or invalid session keys

#### Usage Monitoring
- **Session usage** — 5-hour rolling window percentage with reset countdown timer
- **Weekly usage** — overall weekly consumption percentage with reset date
- **Opus usage** — Opus-specific weekly percentage (shown for Pro users only)
- Color-coded indicators: 🟢 0–74% · 🟡 75–89% · 🔴 90–100%

#### Toolbar Badge
- Live session or weekly usage % displayed on the extension icon
- Badge color reflects consumption level in real time
- Configurable: show session %, weekly %, or hide badge entirely

#### Popup UI
- One-click access from the Chrome toolbar
- Session, weekly, and Opus usage cards with progress bars and reset countdowns
- Manual refresh button with last-updated timestamp
- Profile switcher dropdown in popup header
- Quick link to Settings

#### Smart Notifications
- Desktop alerts at 75%, 90%, and 95% thresholds for both session and weekly usage
- Each threshold fires only once per reset window — no spam
- Per-threshold toggle in Settings (enable/disable individually)
- Notification click opens the extension popup

#### Auto-Refresh
- Background polling via Chrome Alarms API
- Default refresh interval: 60 seconds
- Configurable range: 15–300 seconds
- Automatically pauses when offline, resumes on reconnect
- Always fetches fresh data when popup opens

#### Multi-Profile Support
- Create, rename, and delete unlimited profiles for multiple Claude accounts
- Each profile stores its own session key and organization ID independently
- Quick profile switcher in popup header dropdown
- Per-profile settings: refresh interval, notifications, badge display, percentage mode
- Fun auto-generated profile names

#### Settings Page
- Badge display mode: session %, weekly %, or off
- Percentage mode: used vs. remaining (e.g., "75% used" or "25% remaining")
- Refresh interval selector: 15s, 30s, 60s, 120s, 300s
- Per-threshold notification toggles: 75%, 90%, 95%
- Dark / light / auto color mode
- Manual session key entry for advanced users

#### Privacy & Security
- All data stored locally — no telemetry, no cloud sync, no third-party servers
- HTTPS-only communication with `claude.ai`
- Permissions scoped minimally: `cookies`, `storage`, `alarms`, `notifications`, `claude.ai` host only
- No dynamic code evaluation (`eval` / `new Function`) — Manifest V3 CSP enforced

---

## Original macOS App History

This fork is based on [Claude Usage Tracker](https://github.com/hamed-elfayome/Claude-Usage-Tracker) by [@hamed-elfayome](https://github.com/hamed-elfayome). The original macOS app changelog is preserved below for reference. The Swift/macOS codebase remains unchanged in this fork.

---

## [2.3.0] - 2026-01-23 *(original macOS app)*

### Added
- Multi-profile menu bar display — monitor all Claude accounts simultaneously
- Remaining vs. used percentage toggle (contributed by [@eliasyin](https://github.com/eliasyin))
- Unified `UsageStatusCalculator` for consistent color logic across the app
- Per-profile icons with independent styling and settings

### Fixed
- Color inversion edge cases in monochrome mode
- Profile switching reliability and icon ordering
- Memory leaks when creating/deleting profiles rapidly

---

## [2.2.3] - 2026-01-18 *(original macOS app)*

### Added
- Setup wizard banners: Claude Code info and data migration
- Complete localization in 8 languages

---

## [2.2.2] - 2026-01-18 *(original macOS app)*

### Added
- CLI OAuth authentication fallback with automatic session key prioritization
- System Keychain integration for CLI OAuth tokens

### Changed
- Simplified auto-start logic for better reliability

---

## [2.2.1] - 2026-01-14 *(original macOS app)*

### Added
- Sonnet weekly usage tracking (`seven_day_sonnet_3_5` API field)

### Fixed
- Auto-start sessions not working after Mac sleep/wake

---

## [2.2.0] - 2026-01-12 *(original macOS app)*

### Added
- Multi-profile management system with unlimited profiles
- Claude Code CLI integration with one-click credential sync
- Auto-start session service (per-profile background monitoring)
- Korean language support (8th language)
- Reorganized Settings interface with sidebar navigation

---

## [2.1.2] - 2026-01-10 *(original macOS app)*

### Fixed
- Statusline scripts now only update if already installed
- Organization ID injected directly into scripts instead of API fetch

---

## [2.1.1] - 2026-01-05 *(original macOS app)*

### Added
- Session reset countdown in menu bar icon (contributed by [@khromov](https://github.com/khromov))

---

## [2.1.0] - 2025-12-29 *(original macOS app)*

### Added
- 3-step setup wizard with non-destructive session key testing (contributed by [@alexbartok](https://github.com/alexbartok))
- Modern `UNUserNotificationCenter` integration replacing deprecated APIs

### Fixed
- Menu bar icon flicker during data refresh
- 403 errors from incorrect organization selection during setup

---

## [2.0.0] - 2025-12-28 *(original macOS app)*

### Added
- Official Apple code signing
- Automatic updates via Sparkle framework
- macOS Keychain storage for session keys
- 6-language localization support
- Multi-metric menu bar icons
- Network monitoring and auto-retry

---

## [1.6.2] - 2025-12-22 *(original macOS app)*

### Fixed
- Settings sidebar tab click area
- GitHub Actions release workflow producing working app bundles

---

## [1.6.1] - 2025-12-21 *(original macOS app)*

### Fixed
- High CPU usage (10–35%) on multi-display setups — reduced to 2–9% via image caching

---

## [1.6.0] - 2025-12-21 *(original macOS app)*

### Added
- API console usage tracking (dual endpoint support)
- 5 customizable menu bar icon styles
- Monochrome mode
- Redesigned settings with component library

---

## [1.5.0] - 2025-12-16 *(original macOS app)*

### Added
- GitHub star prompt after 24 hours of usage
- Contributors section in About settings

---

## [1.4.0] - 2025-12-15 *(original macOS app)*

### Added
- Real-time Claude system status indicator
- Detachable floating popover window
- GitHub issue templates and CONTRIBUTING.md (by [@ggfevans](https://github.com/ggfevans))

---

## [1.3.0] - 2025-12-14 *(original macOS app)*

### Added
- Claude Code terminal statusline integration
- 10-level color gradient for usage percentage

---

## [1.2.0] - 2025-12-13 *(original macOS app)*

### Added
- Extra usage cost tracking for Claude Extra subscribers (by [@khromov](https://github.com/khromov))

---

## [1.1.0] - 2025-12-13 *(original macOS app)*

### Added
- Auto-start session on reset
- Enhanced notifications with confirmation feedback

### Fixed
- Menu bar icon visibility in light/dark mode

---

## [1.0.0] - 2025-12-13 *(original macOS app)*

### Added
- Initial release — real-time Claude usage monitoring for macOS menu bar

---

[Unreleased]: ../../compare/v1.0.0...HEAD
[1.0.0]: ../../releases/tag/v1.0.0
