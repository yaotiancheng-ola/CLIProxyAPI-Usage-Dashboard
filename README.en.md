# CLIProxyAPI Usage Dashboard

English | [简体中文](README.md)

CLIProxyAPI Usage Dashboard is a standalone browser page for viewing CLIProxyAPI usage statistics from the management API.

CLIProxyAPI Usage Dashboard is a standalone browser page for viewing usage statistics collected by the `codex-token-usage` plugin.

The page is a single static HTML app. It reads the plugin's server-side SQLite-persisted usage statistics through the CLIProxyAPI management API, shows summaries by key, auth account, and model (including cost, cache hit rate, and average latency), and accumulates request details in the page.

## Features

- Reads `GET /v0/management/plugins/codex-token-usage/summary?window=<all|today|24h|7d|30d>&limit=2000`; the toolbar switches the time range (all / today / last 24h / last 7d / last 30d)
- Per-key server-side stats: requests, input/output tokens, cache hit rate, cost, average latency, last seen
- Per-account and per-model server-side aggregates
- Recent request list (with reasoning effort), filterable by account/model and searchable
- Key aliases can be configured in the `KEY_ALIASES` table at the top of the script (matched by the last 4 chars of each key); panels display "alias (masked key)" with the note in the hover tooltip
- Reading never deletes server-side data; details are persisted to browser IndexedDB (up to 10,000 records) and restored automatically on page load
- The Management key is stored in localStorage (XOR + Base64 obfuscation) and auto-filled on the next visit
- Runs as a static page without a backend service

## Requirements

- CLIProxyAPI with management API enabled
- The `codex-token-usage` plugin installed and enabled on the server
- Network access from the browser to your CLIProxyAPI management endpoint

The default management endpoint expected by the page:

```text
http://127.0.0.1:8317/v0/management
```

## Usage

1. Download or clone this repository.
2. Open `usage.html` in your browser.
3. Enter your CLIProxyAPI management API address.
4. Enter your Management key.
5. Click refresh to load server-side data from the plugin.

You can also open the app directly through `static/usage.html`.

## Important Notes

- The Management key is not included in this repository. Enter your own key in the browser when using the page; it is saved in this browser's localStorage in obfuscated form and auto-filled next time.
- Summaries are read from the plugin's server-side database; reading never deletes data, so you can refresh at any time.
- The recent-requests endpoint only returns the latest records each time; the page accumulates and persists them to IndexedDB (up to 10,000). "Clear page" also deletes the local detail database.
- The `KEY_ALIASES` table is published with the repository — do not put sensitive mappings in it.
- If your browser blocks `file://` requests, serve this folder with any static web server and open the local URL instead.


## Security

Do not commit your Management key or other private CLIProxyAPI credentials. This project intentionally ships with an empty Management key field.

## License

MIT
