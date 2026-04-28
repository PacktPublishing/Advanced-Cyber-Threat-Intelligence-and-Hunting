# Chapter 5 — Understanding Data Sources for Threat Hunting

This directory contains the detections introduced in Chapter 5, which target the two halves of a web-shell attack: the suspicious upload itself, and the post-upload code execution that follows.

## Files

- `high-entropy-webshells.sigma` — Sigma rule that flags HTTP POST requests to script endpoints (`.php`, `.aspx`, `.jsp`, `.cgi`) when the request body has high Shannon entropy (≥ 4.5) and a moderate length (< 5 KB). The combination is characteristic of obfuscated or encoded web-shell payloads being delivered over HTTP.

- `webserver-execution.eql` — EQL sequence rule that correlates a suspicious HTTP POST to a script endpoint with the subsequent spawning of `cmd.exe` or `powershell.exe` from a web-server process (`w3wp.exe`, `php-cgi.exe`) within ten minutes. This pattern captures the classic "upload → execute" web-shell flow rather than relying on signature-based detection of the shell itself.
