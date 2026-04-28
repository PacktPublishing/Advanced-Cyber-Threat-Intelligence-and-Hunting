# Chapter 4 — Core Principles of Proactive Threat Hunting

This directory contains the detection rule introduced in Chapter 4, which focuses on identifying covert channels established through the DNS protocol.

## Files

- `dns-tunnelling.sigma` — Sigma rule that flags abnormally high query volumes against a single parent domain. The condition counts DNS queries per `parent_domain` and alerts when the count exceeds 1,000, which is a tell-tale signature of DNS tunneling used for command-and-control or data exfiltration (MITRE T1071.004 / T1048.003). Tuning guidance and the false-positive considerations (CDNs, legitimate high-volume DNS services) are discussed in the chapter.
