<div align="center">

<img src="https://img.shields.io/badge/Chrome-Extension-4285F4?style=for-the-badge&logo=googlechrome&logoColor=white" />
<img src="https://img.shields.io/badge/Manifest-V3-orange?style=for-the-badge" />
<img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" />
<img src="https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-blueviolet?style=for-the-badge" />

<br /><br />

# 🧠 Claude Usage Tracker — Chrome Extension

**Real-time Claude AI usage monitoring, right in your browser toolbar.**
Cross-platform port of the [macOS menu bar app](https://github.com/hamed-elfayome/Claude-Usage-Tracker) — rebuilt as a Chrome extension for Windows, macOS, and Linux.

[Install from Chrome Web Store](#installation) · [Report a Bug](../../issues) · [Request a Feature](../../issues)

---

</div>

## ✨ What's New in This Fork

This fork adds a fully functional Chrome extension under `chrome-extension/`, bringing every core feature of the original macOS app to the browser — with zero setup required.

| Feature | macOS App | Chrome Extension |
| :--- | :--- | :--- |
| **Platform** | macOS 14+ only | Windows, macOS, Linux |
| **Session Key** | Manual extraction | ✅ Auto-read from browser |
| **Installation** | App download + Homebrew | ✅ One-click from Chrome Web Store |
| **Usage monitoring** | ✅ | ✅ |
| **Notifications** | ✅ | ✅ |
| **Multi-profile** | ✅ | ✅ |

---

## 🚀 Features

- **🔐 Zero-Config Auth** — Automatically reads your `sessionKey` cookie when you're logged into `claude.ai`. No manual extraction, no setup wizard.
- **🔴 Toolbar Badge** — Live session or weekly usage percentage on the extension icon. Color-coded: 🟢 green → 🟡 orange → 🔴 red.
- **📊 Popup UI** — One click shows session (5-hour window), weekly, and Opus usage cards with progress bars and reset countdowns.
- **🔔 Smart Notifications** — Desktop alerts at 75%, 90%, and 95% thresholds. Fires once per reset window — no spam.
- **👤 Multi-Profile** — Manage multiple Claude accounts with completely independent session keys, refresh rates, and settings.
- **⚙️ Settings Page** — Configure badge display, percentage mode (used vs. remaining), refresh interval, and per-threshold notifications.
- **🌙 Dark / Light Mode** — Follows your system preference automatically.

---

## 📦 Installation

### ⚠️ Note
The Chrome Web Store listing is **pending approval**. You can wait for it to go live, or use the extension immediately via **Developer Mode**.

### Option 1 — Load Unpacked (Developer Mode)

1. **Clone this repository** ```bash
   git clone [https://github.com/ali-aldahmani/claude-usage-extension.git](https://github.com/ali-aldahmani/claude-usage-extension.git)
   ```
2. Open Chrome and go to: `chrome://extensions/`
3. Enable **"Developer mode"** (toggle at the top right).
4. Click **"Load unpacked"** and select the `chrome-extension/` folder.
5. The extension icon will appear in your toolbar immediately.

### Option 2 — Chrome Web Store (coming soon)
Click **Add to Chrome** once the listing is approved.

---

## 🛠️ How It Works

The extension uses Chrome's cookies API to automatically read your `sessionKey` from `claude.ai` — the same cookie the original macOS app asks you to extract manually. No credentials are ever sent anywhere except directly to Anthropic's own API.

```text
Browser Cookie (sessionKey)
        ↓
GET claude.ai/api/organizations/{org_id}/usage
        ↓
{ five_hour, seven_day, seven_day_opus }
        ↓
Toolbar badge + Popup UI + Notifications
```

---

## 📁 File Structure

```text
chrome-extension/
├── manifest.json        # Manifest V3 config + permissions
├── background.js        # Service worker — polling, badge, notifications
├── api.js               # Claude.ai API integration + org ID discovery
├── storage.js           # Profile & settings persistence
├── popup.html           # Toolbar popup shell
├── popup.js             # Popup logic — render data, profile switcher
├── popup.css            # Popup styles — dark/light mode, progress bars
├── settings.html        # Full settings page
├── settings.js          # Settings logic — profiles, thresholds, intervals
└── icons/               # PNG icons — 16, 32, 48, 128px
```

---

## 🔑 Permissions

| Permission | Why it's needed |
| :--- | :--- |
| `cookies` | Read `sessionKey` from `claude.ai` automatically |
| `storage` | Save profiles and settings locally |
| `alarms` | Schedule background refresh polling |
| `notifications` | Desktop threshold alerts |
| `host_permissions: claude.ai` | Make usage API requests |

> **Privacy:** All data is stored locally on your device. No telemetry, no cloud sync, no third-party servers. HTTPS-only communication with Anthropic's API.

---

## 🗺️ Roadmap

- [x] Phase 1 — Core: auth, API, popup UI, toolbar badge
- [x] Phase 2 — Automation: auto-refresh, notifications, settings page
- [x] Phase 3 — Profiles: multi-account management, profile switcher
- [x] Phase 4 — Polish: Chrome Web Store submission, icon styles
- [ ] Phase 5 — API Console tracking (Anthropic API key support)
- [ ] Firefox / Edge port

---

## 🤝 Contributing

Contributions are welcome! Please open an issue first for major changes.

```bash
# Fork → clone → create branch
git checkout -b feature/your-feature

# Make changes, then
git commit -m "feat: describe your change"
git push origin feature/your-feature

# Open a Pull Request
```

Follow the existing code style and keep commits descriptive.

---

## 📜 License

**MIT** — see [LICENSE](LICENSE) for details.  
This project is a fork of [Claude Usage Tracker](https://github.com/hamed-elfayome/Claude-Usage-Tracker) by @hamed-elfayome, used under the MIT License.

---

## ⚠️ Disclaimer

This extension is not affiliated with, endorsed by, or sponsored by Anthropic PBC. Claude is a trademark of Anthropic PBC. This is an independent third-party tool created for personal usage monitoring.

<div align="center"> 
  <br /> 
  Built with ❤️ for the Claude community · <a href="../../issues">Report an issue</a> 
</div>
