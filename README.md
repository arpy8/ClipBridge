# ClipBridge

> Instant clipboard sharing between two computers — no installs, no accounts, no server.

A single self-contained HTML file that lets you quickly move text between machines using a shared Room ID and password. Open it on both computers, enter the same credentials, and start passing clipboard content back and forth in seconds.

---

## Features

- **Zero dependencies** — one `.html` file, nothing to install
- **Password-protected rooms** — credentials are SHA-256 hashed before use; wrong password means unreadable data
- **Paste & send** — native clipboard API support for one-click paste and copy
- **Character counter** — live count on both compose and received panels
- **Timestamp** — shows when content was last sent
- **Responsive** — works on mobile, tablet, and desktop
- **Dark terminal UI** — monospace aesthetic with JetBrains Mono + Syne typefaces

---

## How It Works

ClipBridge uses the browser's `localStorage` as a lightweight shared store. The storage key is derived from your Room ID combined with a hash of your password, so two people using the same file with different passwords can coexist without conflict.

```
storage key = "clipbridge__<roomId>__<sha256(password)[0:16]>"
```

> **Important:** Both browsers must share the same `localStorage` origin. This means both instances need to open the file from the **same location** — e.g. a shared network drive, Dropbox folder, or a locally hosted server.

---

## Usage

### Quick Start

1. Download `clipboard-share.html`
2. Place it somewhere accessible from both computers (shared folder, NAS, Dropbox, etc.)
3. Open it in a browser on **Computer A** and **Computer B**
4. Enter the same **Room ID** and **Password** on both
5. On Computer A: paste your text → click **Send →**
6. On Computer B: click **⟳ Pull Latest** → click **copy**

### Sharing Options

| Method | Works? | Notes |
|---|---|---|
| Shared network drive / NAS | ✅ | Both machines open from the same file path |
| Dropbox / OneDrive folder | ✅ | Both must open the synced local copy |
| Local web server (`npx serve`) | ✅ | Serve from one machine, access via LAN IP |
| Two separate copies of the file | ❌ | Different origins = different `localStorage` |
| GitHub Pages / any web host | ✅ | Both access the same URL |

---

## Hosting on a Local Network (recommended)

If you have Node.js installed, you can serve the file over your LAN in one command:

```bash
npx serve .
```

Then open `http://<your-local-ip>:3000/clipboard-share.html` on both machines.

---

## Security Notes

- Passwords are hashed with **SHA-256** via the Web Crypto API — they are never stored in plain text
- Data is stored in `localStorage` and never leaves your machine or network
- This is designed for **trusted local network use** — not as a high-security solution
- Anyone with physical access to the browser's storage can inspect `localStorage` entries

---

## Browser Compatibility

Requires a modern browser with support for:
- `localStorage`
- `crypto.subtle` (Web Crypto API)
- `navigator.clipboard` (Clipboard API — requires HTTPS or `localhost`)

| Browser | Supported |
|---|---|
| Chrome / Edge 80+ | ✅ |
| Firefox 90+ | ✅ |
| Safari 15+ | ✅ |
| Mobile browsers | ✅ |

---

## Tech Stack

- Vanilla HTML / CSS / JavaScript — no frameworks
- [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) + [Syne](https://fonts.google.com/specimen/Syne) via Google Fonts
- Web Crypto API for password hashing
- Clipboard API for native paste/copy
