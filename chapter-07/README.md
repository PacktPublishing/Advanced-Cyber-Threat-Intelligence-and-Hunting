# Chapter 7 — Advanced Hunting Techniques and Queries

This directory contains the queries introduced in Chapter 7, which illustrate the baselining workflow: build a model of normal behavior over a long lookback window, then surface what deviates from it.

## Files

- `splunk-baseline.spl` — Splunk SPL search that builds a 30-day baseline of outbound network connections from Sysmon EventCode 3 data. It surfaces tuples of `(host, process, dest_ip, dest_port)` that have only ever been observed on a single day and where that day is *today* — a "first-seen-today" filter that highlights newly emerging external destinations while excluding RFC1918 ranges.

- `suspicious-network-streams.esql` — ES|QL query that detects beaconing-style traffic by computing the average inter-event interval per `(host, process, stream)` tuple over a long window. Streams whose average interval falls in a narrow band (under one hour) and that have produced at least eight events surface periodic callbacks that user-driven browser traffic does not produce.

- `transform-schema.json` — Definition of an Elasticsearch transform (`ephemeral-lolbas-baseline`) that pivots the last 30 days of LOLBAS-binary execution events into a per-`(host, binary, command)` summary, capturing the day-count, last-seen timestamp, and unique-user count. The resulting `baseline-lolbas-30d` index is the lookup table that downstream detection rules query against to spot first-time or rarely seen LOLBAS invocations.
