# Chapter 8 — Hunting Delivery and Initial Access

This directory contains the detections introduced in Chapter 8, which focus on identity-layer attacks against Microsoft Entra ID (Azure AD): adversary-in-the-middle session theft, MFA fatigue, illicit OAuth grants, password spraying, and session hijacking.

## Files

- `aitm-sessions.esql` — ES|QL query that correlates Azure sign-in logs with Microsoft Graph activity logs by session ID, looking for sessions where the sign-in and the subsequent Graph call originate from different source IPs within a short window. This asymmetry is the operational signature of an AiTM proxy capturing a session token and replaying it from a different infrastructure.

- `azure-mfa-fatigue.kql` — KQL query against `SigninLogs` that detects MFA fatigue (push-bombing) attacks: it identifies users who experienced three or more MFA denials within a 20-minute bucket and were then followed by a successful sign-in. The successful auth following sustained denials is the "user finally accepted" signal.

- `azure-oauth-grants.kql` — KQL query against `AuditLogs` that flags every `Add OAuth2PermissionGrant` event recorded by Core Directory. New OAuth permission grants are the persistence mechanism behind illicit-consent-grant campaigns, where an attacker tricks a user into granting a malicious application long-lived access to their mailbox or files.

- `azure-password-spray.kql` — KQL query against `SigninLogs` that aggregates failed sign-ins (`ResultType 50126`, invalid credentials) by source IP over a 24-hour window. IPs that have failed against more than ten distinct user principals are flagged as password-spray sources.

- `azure-session-hijacking.kql` — KQL query that detects token replay by surfacing successful sign-ins where the same `SessionId` is observed from more than one IP address. A single session ID by definition belongs to a single browser instance — multiple source IPs imply the session cookie has been stolen and replayed elsewhere.
