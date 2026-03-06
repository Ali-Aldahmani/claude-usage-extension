# Security Policy

## Supported Versions

We release security updates for the latest stable version only. Please ensure you're running the most recent version before reporting issues.

| Version | Supported |
| ------- | --------- |
| 1.x.x (latest) | ✅ |
| < 1.0  | ❌ |

[Download the latest version](../../releases/latest)

---

## Reporting a Vulnerability

We take security seriously. If you discover a security vulnerability, please report it responsibly.

### How to Report

**Please do NOT report security vulnerabilities through public GitHub issues.**

Instead, use GitHub's private security advisory feature:

1. Go to the [Security tab](../../security/advisories)
2. Click **"Report a vulnerability"**
3. Provide detailed information about the vulnerability

### What to Include

To help us assess and address the issue quickly, please include:

- **Type of vulnerability** (e.g., credential exposure, data leakage, permission abuse)
- **Step-by-step reproduction** instructions
- **Affected versions** (if known)
- **Potential impact** assessment
- **Proof of concept** code (if applicable)
- **Suggested fix** (if you have one)

### Response Timeline

- **Acknowledgment**: Within 24–48 hours
- **Initial assessment**: Within 1 week
- **Resolution timeline**: Depends on severity and complexity

We'll keep you informed throughout the process and credit you in the security advisory and release notes (unless you prefer to remain anonymous).

---

## Security Considerations

### Session Key Storage

- The `sessionKey` cookie is read directly from the browser — it is **never copied or stored** by the extension unless the user manually enters it in Settings
- If manually entered, it is stored in `chrome.storage.local`, which is encrypted by Chrome and sandboxed to this extension only
- The session key is never transmitted to any server other than `claude.ai` via HTTPS
- No cloud sync, no external storage, no third-party access

### Cookie Access

- The extension requests `cookies` permission **scoped exclusively to `claude.ai`**
- It reads only the `sessionKey` cookie — no other cookies are accessed
- Cookie data is used solely to authenticate API requests to Anthropic's own endpoints

### Network Security

- All communication uses **HTTPS only**
- API requests are sent exclusively to `claude.ai` endpoints
- No telemetry, analytics, or third-party tracking of any kind
- The extension never opens external connections beyond `claude.ai`

### Chrome Extension Sandboxing

- The extension runs inside Chrome's built-in sandbox
- It has no access to the file system, OS, or other applications
- Permissions are strictly limited to what's declared in `manifest.json`:
  - `cookies` — read `sessionKey` from `claude.ai` only
  - `storage` — local profile and settings persistence
  - `alarms` — schedule background refresh
  - `notifications` — threshold desktop alerts
  - `host_permissions: https://claude.ai/*` — API requests only

### No Arbitrary Code Execution

- The extension contains no dynamic code evaluation (`eval`, `new Function`, etc.)
- All scripts are statically declared in `manifest.json`
- Content Security Policy (CSP) is enforced via Manifest V3

---

## Best Practices for Users

### Protect Your Session Key

- Never share your `sessionKey` value publicly
- Treat it like a password — it grants access to your Claude account
- If you suspect compromise, log out of claude.ai and back in to rotate it
- The extension never displays your full session key anywhere in its UI

### Verify the Extension Source

- Install only from the official [Chrome Web Store listing](../../releases) or by loading from this repository directly
- When loading unpacked, always verify you're using the source from this repo
- Review `manifest.json` permissions before installing — they should match exactly what's listed above

### Keep Updated

- Security patches are released for the latest version only
- Enable GitHub notifications for new releases
- Review [CHANGELOG.md](CHANGELOG.md) for security-related updates

---

## Security Acknowledgments

We recognize and appreciate security researchers who help keep our community safe. Contributors who responsibly disclose vulnerabilities will be:

- Credited in the security advisory (with permission)
- Acknowledged in release notes
- Listed as security contributors in the project

Thank you for helping keep Claude Usage Tracker secure! 🔒

---

## Questions?

For non-security issues, please use [GitHub Issues](../../issues).

For general questions, see our [Contributing Guide](CONTRIBUTING.md).
