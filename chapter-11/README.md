# Chapter 11 — Hunting for Lateral Movement and Discovery

This directory contains the detections introduced in Chapter 11, covering three distinct lateral-movement and credential-access techniques: AD FS token-signing certificate theft, resource-based constrained delegation (RBCD) abuse, and remote SMB execution.

## Files

- `adfs-certs-exfil.eql` — EQL rule that flags PowerShell or `cmd.exe` command lines invoking `Export-PfxCertificate` or `certutil … -exportPFX`, the two primary methods used to extract the AD FS token-signing certificate (the prerequisite for a Golden SAML attack).

- `rbcd-lateral-movement.eql` — EQL rule against Windows Security Event 5136 that detects modifications of the `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute initiated by a computer account (account name ending in `$`). Writes to this attribute are the signature of an RBCD-based privilege escalation or lateral movement.

- `smb-remote-execution.eql` — EQL sequence rule that catches the canonical PsExec-style remote-execution pattern: an `.exe` file is written to disk by the `System` process (the SMB write side) and the same file is then executed within ten seconds — the signature of a service or scheduled binary delivered over SMB.
