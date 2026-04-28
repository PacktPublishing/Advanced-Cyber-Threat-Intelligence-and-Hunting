# Chapter 16 — Hunting in Cloud and Specialized Environments

This directory contains the detections introduced in Chapter 16, which extend the methodology beyond the traditional endpoint perimeter into AWS, Azure, Kubernetes, and operational-technology (OT) environments.

## Files

### AWS / CloudTrail

- `aws-cross-activity.esql` — Detects cross-account `AssumeRole` activity by joining source and target account IDs from CloudTrail. New cross-account role assumptions appearing in the last 7 days surface as candidates for review — the typical signature of pivoting through a delegated trust.

- `aws-imds-attack.esql` — Flags EC2 instance-role credentials (`AssumedRole` sessions whose issuer is an `ec2-*`, `EC2*`, or `web-*` role) that are used from more than two distinct source IPs. Instance-role credentials should only ever be used from the instance they were issued to — multiple source IPs imply credentials stolen via the Instance Metadata Service.

- `aws-rare-api-calls.esql` — Surfaces rarely-used IAM API actions (≤ 3 calls in 30 days) per principal, flagging the kind of one-off privilege-discovery and -manipulation calls that characterize cloud reconnaissance.

- `aws-self-escalation.esql` — Detects principals attaching or putting policies onto themselves (`AttachUserPolicy`, `AttachRolePolicy`, `PutUserPolicy`, `PutRolePolicy` where caller and target are the same identity) — the textbook self-escalation pattern.

- `defense-evasion-cloudtrail.esql` — Catches CloudTrail tampering: `StopLogging`, `DeleteTrail`, `UpdateTrail`, `PutEventSelectors`, and `DeleteEventDataStore`. These are the canonical "turn off the cameras" actions that precede further malicious activity.

- `lambda-creation.esql` — Detects principals exercising the full Lambda-creation chain (`CreateFunction`, `UpdateFunctionCode`, `AddPermission`, `CreateEventSourceMapping`) — the signature of an adversary deploying a serverless backdoor or persistence function.

- `s3-bucket-exfil.esql` — Flags S3 principals that perform more than 100 `GetObject` calls within a single hour, with the distinct-key count as a noise filter. Bulk object retrieval is the operational signature of S3 data exfiltration.

- `s3-buckets-firstseen-principals.esql` — Surfaces the first time a `(principal, bucket)` pair appears in CloudTrail across a 30-day window, with the first-seen filter narrowed to the last 7 days. New principal-to-bucket access relationships are a high-fidelity discovery signal.

### Azure / Entra ID

- `azure-cross-tenant-access.kql` — Detects sign-ins where `HomeTenantId` differs from `ResourceTenantId` — that is, guest or external-identity authentications. New cross-tenant identities appearing in the last 7 days are surfaced for review.

### Kubernetes

- `pod-creation-azure.kql` — KQL query against `AzureDiagnostics` that detects creation of Kubernetes pods running with `privileged: true` or `hostPID: true` — the two security-context flags most commonly abused for container-escape and node-takeover.

### Operational Technology

- `ot-plc-stack-counting.esql` — Detects executions of engineering-workstation software that talks to PLCs (`S7TgtOPx.exe`, `RSLogix5000.exe`, `Studio5000.exe`, `UnityPro.exe`, `ProficyMachineEdition.exe`) outside of an approved maintenance window. The maintenance-window lookup is the lookup table that the chapter walks through building.
