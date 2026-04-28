# Chapter 13 — Hunting for Collection, Exfiltration and Impact 

This directory contains the detections introduced in Chapter 13, which span the final three tactics of the kill chain. The queries are grouped by tactic in the list below.

## Collection

- `collection-bulk-copy.esql` — Detects bulk-copy operations using the canonical Windows utilities: `robocopy`/`xcopy` with recursive switches (`/S`, `/E`, `/MIR`), `Copy-Item -Recurse` in PowerShell, or `cmd.exe copy` to a UNC path. Volume aggregation surfaces the noisiest invocations.

- `collection-file-enumeration.esql` — Flags processes that produce more than 500 distinct directory listings within a 5-minute window — the volumetric signature of a discovery or staging script walking the filesystem.

- `collection-file-searching.esql` — Catches command lines that recursively search the filesystem for sensitive content (`dir /s`, `Get-ChildItem -Recurse`) using extension or keyword filters such as `docx`, `pst`, `kdbx`, `password`, `secret`, `credential`, `api_key`, `token`.

- `collection-process-file-reads.esql` — Flags processes that perform more than 500 file reads across many distinct files and directories within a 10-minute window — the signature of a collection script reading documents in bulk.

- `collection-smb-enumeration.esql` — Surfaces user accounts that touch more than ten distinct SMB shares across more than three hosts in 24 hours — the network equivalent of bulk filesystem enumeration.

- `collection-staging-file-writes.esql` — Catches staging behavior: more than 250 file writes within 10 minutes into one of the standard staging directories (`Temp`, `PerfLogs`, `ProgramData`, `Users\Public`, `$Recycle.Bin`).

## Exfiltration

- `exfiltration-asymmetric-sessions.esql` — Computes the outbound-to-inbound byte ratio per `(source, destination)` pair and flags pairs with ratio > 5 and > 50 MB outbound — the asymmetric profile of data leaving the network.

- `exfiltration-session-baselining.esql` — Splits the lookback window into a 6-day baseline and a 24-hour recent window, and flags `(source, destination, port)` tuples whose recent average session duration is more than 5× the baseline average — long-running tunnels emerging where short sessions used to be.

- `exfiltration-session-frequency.esql` — Same baseline-vs-recent split applied to daily session count: flags tuples whose recent daily count exceeds 3× the baseline daily average and is greater than 100 — periodic callbacks emerging where there were few before.

- `exfiltration-traffic-baselining-01.esql` — Builds the per-day outbound-volume baseline (avg, max, stddev) for `(source, destination)` pairs from server hosts over 7 days. Output feeds the `baseline_daily_volume` lookup used by the second query.

- `exfiltration-traffic-baselining-02.esql` — Joins the last 24 hours of outbound volume against the baseline lookup and flags pairs where today's volume exceeds 3× the baseline average and is over 10 MB — large-volume deviations from the established daily norm.

## Impact

- `impact-file-encryption.esql` — Flags processes that modify, overwrite, or rename more than 1,000 files within a 5-minute window — the volumetric and temporal signature of ransomware encryption.

- `impact-mass-credential-events.esql` — Catches mass credential modification (Security Event IDs 4724 / 4725 / 4738) by a single subject affecting more than 100 distinct target accounts within 10 minutes — the destructive-impact pattern of a domain-wide password reset by an adversary in control of a privileged account.

- `impact-raw-wiping.esql` — Surfaces any process opening raw disk devices (`\\.\PhysicalDrive*`, `\\.\HarddiskVolume*`), the access pattern used by disk wipers and bootloader-overwriting destructive tooling.

- `impact-shadow-copy-deletion.esql` — Detects the canonical anti-recovery command set used by ransomware: `vssadmin delete shadow*`, `wmic shadowcopy delete`, or PowerShell `Win32_ShadowCopy ... Delete`.
