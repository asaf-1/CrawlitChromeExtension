([Past chat][1])([Past chat][1])([Past chat][1])([Past chat][1])([Past chat][1])

בטח. הנה README מוכן לפרויקט **Crawlit Extension** (תעתיק ל־`README.md`). הוא כתוב בצורה “GitHub-ready” ובלי לחשוף דברים רגישים.

---

# Crawlit – Chrome Extension (Single-Page Flow Scanner & Downloader)

Crawlit is a Chrome extension that **scans the current page** for relevant “flows/files” and lets you **download** what was found — with support for **single-page scanning & downloading**.

The project is split into:

* `popup.js` – Popup UI + user actions (Scan / Download)
* `content.js` – Scanning logic that runs on the active tab
* `sw.js` – Service Worker (background): stores scan results and performs downloads

---

## Features

* ✅ **Scan current page** (single-page scan)
* ✅ Persist scan results in background (e.g., `SCANNED_FILES`)
* ✅ **Download scanned results**
* ✅ Designed for “flow/file discovery” pages where download links might be indirect

---

## How it works (High Level)

1. User clicks **Scan current page** in the popup.
2. `popup.js` sends a message to `content.js` to scan the active page.
3. `content.js` extracts candidates (URLs / metadata) and returns results.
4. `sw.js` saves them (e.g., `SCANNED_FILES`) for later actions.
5. User clicks **Download scanned**, and `sw.js` downloads each item.

---

## Project Structure

```
CrawlitExtension/
├─ manifest.json
├─ popup.html
├─ popup.js
├─ content.js
├─ sw.js
└─ assets/ (icons, etc.)
```

---

## Installation (Local / Developer Mode)

1. Open Chrome and go to: `chrome://extensions`
2. Enable **Developer mode** (top right)
3. Click **Load unpacked**
4. Select the extension folder (the one containing `manifest.json`)
5. Pin the extension (optional) for easier access

---

## Usage

### Scan current page

1. Open the page you want to scan
2. Click the Crawlit extension icon
3. Click **Scan current page**
4. The extension will collect results from the active tab

### Download scanned

1. After scanning, click **Download scanned**
2. The extension downloads the collected items (via background worker)

---

## Notes on “Direct Download URL” Handling

Some pages don’t expose a clean downloadable URL directly (they might hide it behind query params like `?file=...` or `?url=...`).

Planned/Supported approach:

* Extract a `directUrl` during scanning (from query params like `file/url`)
* Save it inside `SCANNED_FILES` in `sw.js`
* Download using: `(it.directUrl || it.url)`

---

## Messaging (Typical)

Popup triggers actions by sending messages, for example:

* `SCAN_SINGLE_PAGE` – scan the active tab
* `DOWNLOAD_SCANNED` – download what was saved in the background store

(Exact message names can be aligned to what you already implemented.)

---

## Troubleshooting

**Nothing is found in scan**

* Make sure the page is fully loaded before scanning
* Check DevTools → Console (content script logs)
* Verify required permissions and host matches in `manifest.json`

**Downloads don’t start**

* Check `chrome://extensions` → Service Worker logs
* Ensure download permission exists (if required by your manifest version)
* Validate that URLs are not blocked (CORS doesn’t apply to downloads, but restrictions can still exist)

---

## Roadmap / Ideas

* Better `directUrl` extraction coverage
* Smarter deduplication + filename normalization
* Export scan results to JSON/CSV
* Rules per site (site-specific parsers)

---

## License

Add your preferred license (MIT is common for personal projects).
If you want, I can generate a `LICENSE` file too.

---

אם אתה שולח לי את **manifest.json** (רק את החלק של permissions + action + scripts) או צילום מסך של ה־popup, אני אתאים את ה־README ל־100% לפי השמות המדויקים אצלך (כפתורים, message types, permissions, וכו’) בלי לנחש.


