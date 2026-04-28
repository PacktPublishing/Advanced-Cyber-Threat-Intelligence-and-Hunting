# Chapter 9 — Hunting for Exploitation and Execution 

This directory contains the detections introduced in Chapter 9, which target the Execution tactic (TA0002): the moment an adversary's code first runs on a host. The set covers container delivery, shellcode injection, PowerShell abuse, and abuse of Windows execution mechanisms (scheduled tasks and services).

## Files

- `container-based-execution.eql` — EQL sequence rule that detects the ISO/IMG container-delivery pattern used to bypass mark-of-the-web: a `.iso` or `.img` file is mounted, and within five minutes a script-host process (`wscript.exe`, `cscript.exe`, `powershell.exe`, `mshta.exe`, `rundll32.exe`, etc.) executes a `.lnk` or `.vbs` from the mounted volume.

- `defender-shellcode-injection.kql` — KQL query against `DeviceMemoryEvents` (Microsoft Defender for Endpoint) that flags `VirtualAlloc` calls allocating memory pages with `PAGE_EXECUTE_READWRITE` protection — the canonical RWX allocation that precedes shellcode execution. Browsers and `devenv.exe` are excluded as known noisy sources.

- `ps-encoded-commands.esql` — ES|QL query that surfaces PowerShell invocations using `-EncodedCommand` or `-Enc`, the standard mechanism for running base64-encoded payloads to evade command-line inspection.

- `ps-staging.esql` — ES|QL query that flags PowerShell command lines containing the staging primitives `Invoke-WebRequest` and `IEX` (Invoke-Expression), the building blocks of most PowerShell download-and-execute one-liners.

- `ps-temp-execution.esql` — ES|QL query that detects PowerShell file activity inside `AppData\Local\Temp`, a common staging directory used by phishing and malware loaders to drop and execute follow-on payloads.

- `windows-scheduled-task-execution.eql` — EQL sequence rule that catches the create-then-delete scheduled task pattern (Security Event ID 4698 followed by 4699 within one minute, on the same `TaskName`). Adversaries use ephemeral tasks for one-shot execution while leaving minimal persistence behind.

- `windows-service-execution.eql` — EQL sequence rule that detects the create → start → delete service pattern (System Event IDs 7045, 7036, then 7035/7034 on the same `ServiceName` within one minute) — the lifecycle of a single-use service used for one-shot SYSTEM execution.
