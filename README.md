# Shab TraceBrowser

## v2.0.0.0 theme consistency

- Dark-mode workbench tab headers are owner-drawn and no longer remain white.
- Light-mode request/status areas use light surfaces instead of hard-coded dark backgrounds.
- Product, assembly, file, informational, window-title and About version are aligned to 2.0.0.0.


Shab TraceBrowser is a Windows desktop browser and diagnostic workbench for website/network troubleshooting. It is built with .NET 8 WinForms and Microsoft Edge WebView2.

## Previous UI refinement

- Compact top area with reduced toolbar/button/panel spacing.
- Top settings are now grouped as **Permissions**, **Block resources**, and **Browser tools**.
- Theme selector, Disable cache, Site data and Report are now correctly located under **Browser tools**, not Permissions.
- Resource blocking receives the widest top group so all diagnostic block switches wrap more predictably.
- Extra Access state is now a compact badge inside the Permissions group.

## Previous feature highlights

- Resource blocking: Images, JavaScript, CSS, Fonts, 3rd party, Media, WebSocket, XHR/Fetch, common Tracking hosts, common Ad hosts and Service Workers.
- Dedicated DNS Analyzer with A/AAAA/CNAME/MX/TXT/NS/PTR queries and resolver comparison.
- Traceroute, live Ping Monitor with loss/jitter statistics, TCP Port Test and IPv4/IPv6 diagnostics.
- Performance dashboard with Navigation Timing, FCP, LCP, CLS and approximate INP, memory/FPS and a simple Core Web Vitals grade.
- Resource Summary with request counts, blocked count, known Content-Length total and largest known resources.
- Cache Analyzer for Cache-Control, ETag, Last-Modified, Expires and Age.
- TLS Inspector with protocol, cipher, certificate lifetime, SAN and certificate chain.
- HTTP protocol detection using WebView2 DevTools Protocol when available (HTTP/1.1, HTTP/2, HTTP/3).
- Redirect visualizer with status/URL hops and total probe cost.
- Separate **Disable cache** mode (different from Clear Cache).
- **Clear site data** for the current origin: cookies, local/session storage, IndexedDB (when enumerable), CacheStorage and Service Workers. It intentionally does not erase the global HTTP disk cache for other sites.
- Diagnostic report export to HTML, JSON or CSV.
- Repeat HTTP test with min/average/max/P95 for TTFB and total time.
- Workbench tab layout, persistent bottom diagnostic status line and System/Light/Dark theme selector.
- WebView2 process-failure recovery and navigation-scoped cancellation of diagnostics.

## Resource blocking notes

The built-in Tracking/Ads lists are intentionally small diagnostic lists, not a full subscription-based ad blocker. They are intended for quick comparisons while testing a page.

Service Worker blocking has two layers: service-worker network requests are rejected when identified, and a document-created guard prevents new `navigator.serviceWorker.register()` calls while the option is enabled. Existing registrations on the current page are unregistered before reload when possible.

When any resource blocker is active, TraceBrowser bypasses the browser HTTP cache so cached content cannot silently defeat the test. The explicit **Disable cache** checkbox keeps cache bypass active even when no blocker is selected.

## Diagnostic behavior

- DNS Analyzer uses direct UDP DNS queries for record inspection. Corporate firewalls may block direct public resolver queries; this is reported as timeout/blocked rather than treated as a program failure.
- Traceroute and Ping rely on ICMP, which some hosts/networks block.
- IPv4/IPv6 HTTP family probes use direct connections with proxy bypass so each IP family can be tested independently; on proxy-only networks this direct test can fail even when the browser works through a proxy.
- Throughput shown in the Network tab is a bounded active sample, not a full ISP speed test.
- INP is an approximation based on Event Timing observations collected by the embedded page instrumentation.
- Resource transferred-size totals only include responses where a usable Content-Length was observed; chunked/compressed responses may not contribute to that total.

## Requirements

- Windows 10/11 or Windows Server with desktop support.
- .NET 8 SDK for development.
- Microsoft Edge WebView2 Runtime.

## Version

Current source version: **2.0.0.0**

Maintained by `shabahang.j`  
Copyright © 2026 shabahang.j — All Rights Reserved.

