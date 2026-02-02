# README — Shab TraceBrowser

## What the project does

Shab TraceBrowser is a small, developer-focused test browser built with **PySide6 / Qt WebEngine**.
It provides a lightweight UI for loading pages and inspecting:

* network / redirect chain / headers,
* TLS / certificate details (CN, OU, O, issued/expires, SANs, cipher, TLS version),
* resource requests (images, JS, CSS, fonts, other) with a request list you can inspect and resend,
* timing metrics (load time, TTFB — from the page when available — ping, download speed, FPS),
* a **permission indicator** (a small circle next to the URL) that lights and blinks only when the site *actually uses* features like microphone, camera, location, notifications, screen share, clipboard-read, pointer lock, etc.,
* a test toggle **Grant Extra Access** which (when enabled) programmatically grants extra feature permissions and relaxes a few page/profile settings for easier testing,
* quick actions: Clear Cache, Refresh, Proxy modes (Off/System/Manual), Block Resources checkboxes (Images/JS/CSS/Fonts/3rd-party) which auto-run Clear Cache → Refresh to apply changes,
* an About dialog and Dev Panel with word-wrapped diagnostic fields.

The browser aims to be a practical tool for front-end and security testing, debugging certificates, and quickly verifying how a page behaves under different browser capabilities.

---

## Why the project is useful

* **Fast diagnostics**: view TLS/certificate information and response headers without opening external tooling.
* **Permission usage detection**: shows *actual usage* of sensitive features (not just whether the browser *could* grant them).
* **Controlled testing**: quickly toggle resource blocking or grant broad permissions to reproduce different behaviors.
* **Lightweight & auditable**: small codebase you can read and extend (no heavy browser distribution required).
* **Dev panel**: useful for developers testing CSP/HSTS, content-type headers, redirect chains, and network metrics.

This is useful for frontend developers, QA/security testers, and anyone who needs quick, repeatable checks of a site’s runtime behavior and TLS metadata.

---

## How users can get started with the project

### Requirements

* Python 3.8+ (3.9/3.10/3.11 recommended)
* `pip`
* OS: Windows / Linux / macOS (Qt WebEngine support required — packaged with PySide6)
* Internet access for the runtime tests (public IP check, download speed test, cert fetch)

### Certificate fields show `N/A`

* The browser attempts multiple strategies to decode TLS certificate details (SSL-wrapped socket + `ssl.get_server_certificate` + `_test_decode_cert`). If the environment lacks proper OpenSSL hooks or network access, fields may be `N/A`.
* Make sure your system allows outgoing TCP/443 and that your Python/OpenSSL is complete.

### Indicator blinking too often / stale state

* The app clears transient per-origin usage on navigation/origin change. If you see stale blinking, make sure you navigate to a new origin (host changes) or reload the page. The "Grant Extra Access" setting **does not** mark the indicator as "used" — only actual runtime feature calls or explicit JS permission states mark activity.

---

## Development notes (for contributors)

* Main UI: `PySide6` + `Qt WebEngine` (`QWebEngineView`, `QWebEnginePage`, `QWebEngineProfile`).
* Permission tracking: dual approach

  * Qt feature permission signal (`featurePermissionRequested`) — catches page requests for features.
  * JS wrappers injected on load — instruments `navigator.mediaDevices.getUserMedia`, `navigator.geolocation`, `Notification.requestPermission`, `getDisplayMedia`, etc., and exposes `window.__shab_get_used()` for polling real usage.
* Certificate inspection: attempts to fetch DER via `ssl` socket and decodes using `ssl._ssl._test_decode_cert(tempfile)` (Windows-safe fallback included).
* Auto-clear & refresh: controlled timers (1s/2s) when Block Resources toggle changes.
* Code style: keep UI layout changes minimal and avoid large rewrites — the project is intended to be small and easy to inspect.

---

## For help, contact the maintainer via email

`shabahang.j@gmail.com`

Please include:

* brief description of the problem,
* Python version and OS,
* exact console output / traceback (copy/paste),
* optional screenshot of the UI.

---

## Who maintains and contributes to the project

Maintained by **shabahang.j**.

Contributions are welcome via pull requests on GitHub. Suggested contribution workflow:

1. Fork the repository.
2. Create a topic branch (`feature/xxx` or `fix/yyy`).
3. Run and test changes locally.
4. Open a pull request with a clear description and related test steps/screenshots.
5. Keep changes focused and avoid rewriting the entire UI unless necessary.

---

## License


Maintained by shabahang.j

Contributions are welcome via pull requests on GitHub.

Donate : https://nowpayments.io/donation/shabahang.j
For help, contact the maintainer via email: shabahang.j@gmail.com

