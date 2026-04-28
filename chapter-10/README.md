# Chapter 10 — Hunting for Persistence and Privilege Escalation 

This directory contains the detections introduced in Chapter 10, which cover three closely related post-exploitation tactics on Windows: privilege escalation, the persistence mechanisms used to survive reboots, and the credential-access techniques that fuel further movement.

## Files

- `credential-dumping-registry.eql` — EQL rule that detects direct reads of the SAM and SECURITY registry hives — the offline route to local password hashes and LSA secrets. Legitimate readers (`lsass.exe`, `services.exe`, `svchost.exe`) are excluded.

- `cross-process-privesc.esql` — ES|QL query that surfaces privilege-escalation injection patterns: API calls (`OpenProcess`, `WriteProcessMemory`, `CreateRemoteThread`, etc.) where a low/medium-integrity process targets a high/system-integrity process, aggregated and filtered to keep only rare combinations.

- `dll-hijacking.esql` — ES|QL query that detects DLL side-loading by finding trusted Microsoft-signed processes loading DLLs that are *not* Microsoft-signed and that sit outside the standard Windows directories. Filtering keeps only one-off occurrences (single host, single load).

- `elevation-privesc.eql` — EQL sequence rule that catches UAC bypass and token-elevation patterns: a low/medium-integrity process whose child process runs as `SYSTEM` (SID `S-1-5-18`) within five minutes.

- `memory-dumping-comsvcs.sigma` — Sigma rule for the well-known `rundll32.exe comsvcs.dll, MiniDump` LSASS-dumping technique. Detects the specific command-line markers (`comsvcs`, `full`, MiniDump ordinals like `#-`, `#+`, `#24`) that this credential-dumping tradecraft requires.

- `outlook-macro-persistence.eql` — EQL rule that flags any process other than `OUTLOOK.EXE` writing to `VbaProject.OTM`, the Outlook VBA project file. Modifying it from outside Outlook itself is a strong indicator of macro-based persistence (T1137).

- `process-injection-sysmon.sigma` — Sigma rule for Sysmon Event ID 8 (`CreateRemoteThread`) with filters that suppress the two dominant noise sources: WMI provider service threads into `svchost.exe`, and `csrss.exe`-sourced threads.

- `run-keys-baselining.esql` — ES|QL baselining query that aggregates Run/RunOnce registry values across the operations workstation fleet, sorted ascending so that the rarest entries — the ones likely to represent real persistence — surface first.

- `run-keys-scripted-execution.sigma` — Sigma rule that fires when a Run/RunOnce key is set to invoke a script engine (`powershell`, `cmd.exe`, `wscript.exe`, `cscript.exe`, `mshta.exe`). Legitimate persistence rarely calls these binaries directly; persistent malware almost always does.

- `suspicious-run-keys.eql` — EQL rule that flags Run/RunOnce values pointing into user-writable or world-writable directories (`AppData`, `Users\Public`, `ProgramData`, `Temp`, `Downloads`) — locations from which legitimate installed software does not normally autostart.

- `windows-services-baselining.esql` — ES|QL baselining query over service-creation events (4697 and 7045) that ranks `(service_name, image_path)` tuples by host count, ascending. Rare combinations seen on five or fewer hosts surface to the top for review.

- `word-template-persistence.sigma` — Sigma rule that detects modifications of the Word global template `Normal.dotm` by script-host or shell processes (`powershell.exe`, `cmd.exe`, `wscript.exe`, etc.) — the file-side signature of T1137.001 template-injection persistence.
