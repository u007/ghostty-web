# Security Policy

## Reporting a vulnerability

Please do not open a public issue for security problems.

Report privately through GitHub's private vulnerability reporting:
<https://github.com/u007/ghostty-web/security/advisories/new>

Include the affected version or commit, a description of the impact, and steps
or a proof of concept to reproduce it.

Issues in code inherited from upstream (`coder/ghostty-web`) or from Ghostty
itself may also need to be reported to those projects:

- <https://github.com/coder/ghostty-web>
- <https://github.com/ghostty-org/ghostty>

## Scope

In scope:

- The `ghostty-web` library in `lib/`, including paste handling, OSC 8
  hyperlinks, window titles and WASM loading.
- The WASM API patch in `patches/ghostty-wasm-api.patch`.
- The demo server in `demo/` (a PTY exposed over WebSocket).
- CI and release workflows in `.github/`.

Terminal output is untrusted input. Anything a program can print to the
terminal and use to run commands, open unexpected URLs or script the host page
is a vulnerability.

## Security behaviour embedders should know

- Pasted text has unsafe control characters replaced with spaces, and bracketed
  paste cannot be closed early by pasted `ESC[201~`.
- OSC 8 hyperlinks are clickable only for `http:` and `https:` unless a
  `linkHandler` sets `allowNonHttpProtocols`. If you enable other schemes,
  validate the URI in `activate`.
- Window titles from OSC 0 and OSC 2 have control characters removed and are
  ignored above 255 bytes.
- Loading the WASM needs `'wasm-unsafe-eval'` in `script-src`.
- The demo server is for local use. It binds to `127.0.0.1` and requires a
  token and a matching `Origin`. Do not bind it to a public interface.

## Supported versions

Only the latest release and the `security-fixes` line of this fork receive
fixes.
