# Chapter 6 — Hunting Zero-Days Through Behavioral Signatures

This directory contains the Elastic ML job definition introduced in Chapter 6, which demonstrates how unsupervised anomaly detection can complement signature-based rules.

## Files

- `suspicious-api-request-ml.json` — Definition for an Elastic ML anomaly-detection job (`suspicious_api_requests_v1`) that profiles per-host API request behavior across 5-minute buckets. It runs three detectors in parallel: `rare` URL paths, `high_count` of requests to a path, and `high_distinct_count` of distinct query signatures. Influencer fields (`url.query`, `source.ip`, request signature) help an analyst pivot from a flagged anomaly to the underlying actor.
