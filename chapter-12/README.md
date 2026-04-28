# Chapter 12 — Hunting for Command and Control

This directory contains the queries introduced in Chapter 12, which extend the baselining methodology from Chapter 7 to web-proxy and DNS telemetry. The pattern in each case is the same: build a rarity model from a long lookback window, then use the model to flag deviations in the recent window.

## Files

- `api-dns-baselining.esql` — ES|QL query that profiles 24 hours of DNS resolutions from the operations workstation fleet, excluding browsers. Domains that are seen on at most two hosts but with high overall volume (≥ 50 lookups) surface as candidate C2 or API-style callback destinations.

- `ua-baselining-01.esql` — First half of the user-agent rarity workflow: a 7-day aggregation that produces a global frequency table of `user_agent.original` values across all proxy traffic. The output feeds the `ua_rarity_lookup` enrich index used by the second query.

- `ua-baselining-02.esql` — Second half of the user-agent rarity workflow: a 24-hour aggregation per `(user_agent, source.ip)` that joins against the rarity lookup and surfaces UAs that are globally rare (< 10 occurrences over 7 days) but locally noisy (> 50 occurrences in 24 hours from the same source) — the signature of a custom client tool calling a small set of destinations.

- `uri-stability.esql` — ES|QL query that finds URIs that are reused across multiple destination domains, a structural signal often associated with phishing kits, generic malware C2 paths, or shared exploit infrastructure. Surfaces URIs hitting at least three distinct hosts with at least 20 total requests.
