# Chapter 18 — Emerging Trends in Threat Hunting and CTI
This directory contains the ES|QL implementation of the velocity-extended Time-Terrain-Behavior (TTB) scoring framework introduced in Chapter 18. The query extends the base TTB framework from Chapter 15 with a velocity component on the temporal dimension, surfacing entities whose temporal footprint expands at a rate inconsistent with their peer baseline — the behavioral signature characteristic of agentic AI execution, automated reconnaissance, credential-testing scripts and other machine-speed adversary activity.

## Files
- `ttb-diversity-scoring-ai.esql` — the complete runnable query
- `host_peer_group.json` — example enrich policy mapping hosts to peer groups
- `peer_group_baselines.json` — example enrich policy mapping peer groups to baseline transition rates

## Prerequisites
The query depends on two enrich policies that need to be created in the Elastic cluster before it runs:

1. **`host_peer_group`** — maps each host to the peer group it belongs to (for example `workstation`, `domain_controller`, `build_server`, `developer_workstation`, `kiosk`). This grouping is what allows velocity comparisons to be made against entities of comparable operational role rather than against the entire fleet.

2. **`peer_group_baselines`** — maps each peer group to the median transition rate observed during legitimate operation. This is the baseline against which a candidate entity's velocity ratio is computed.

Both policies need to be populated from your environment. The repository includes example definitions in `enrich_policies/`, but the actual baseline values must be derived from your own telemetry — a baseline transition rate that is correct for one organization's workstation fleet will not be correct for another's.

## Setup

### 1. Define and populate the host-to-peer-group mapping

Create an index containing the mapping from host name to peer group:

```
PUT host_peer_group_index/_doc/_bulk
{ "index": { "_id": "host-001" } }
{ "host.name": "host-001", "peer_group": "workstation" }
{ "index": { "_id": "dc-001" } }
{ "host.name": "dc-001", "peer_group": "domain_controller" }
{ "index": { "_id": "build-001" } }
{ "host.name": "build-001", "peer_group": "build_server" }
```

Then create the enrich policy:

```
PUT _enrich/policy/host_peer_group
{
  "match": {
    "indices": "host_peer_group_index",
    "match_field": "host.name",
    "enrich_fields": ["peer_group"]
  }
}

POST _enrich/policy/host_peer_group/_execute
```

### 2. Define and populate the peer-group baseline mapping

Calculate the median transition rate for each peer group from a known-clean baseline window (typically thirty days of data from a period when no compromise is suspected). The baseline is the median value of `time_diversity / window_duration_seconds` across all entities in the peer group during the baseline window.

Create the index and policy:

```
PUT peer_group_baselines_index/_doc/_bulk
{ "index": { "_id": "workstation" } }
{ "peer_group": "workstation", "peer_median_rate": 0.00008 }
{ "index": { "_id": "domain_controller" } }
{ "peer_group": "domain_controller", "peer_median_rate": 0.0005 }
{ "index": { "_id": "build_server" } }
{ "peer_group": "build_server", "peer_median_rate": 0.002 }
```

```
PUT _enrich/policy/peer_group_baselines
{
  "match": {
    "indices": "peer_group_baselines_index",
    "match_field": "peer_group",
    "enrich_fields": ["peer_median_rate"]
  }
}

POST _enrich/policy/peer_group_baselines/_execute
```

The example values shown above are illustrative. The values for your environment must be calculated from your own baseline data.

### 3. Run the query

The query in `chapter_18_ttb_velocity.esql` can be executed directly in Kibana's Discover view or through the ES|QL API.

## Reading the output

The query produces one row per host, with the following key columns:

- `time_diversity` — count of distinct time contexts in which the host was observed (base TTB)
- `terrain_diversity` — count of distinct telemetry sources observing the host (base TTB)
- `behavior_diversity` — count of distinct action categories produced by the host (base TTB)
- `velocity_ratio` — ratio of the host's transition rate to the peer-group median
- `time_score_velocity` — velocity-extended Time Score (replaces base `time_diversity` in the TTB aggregation)
- `ttb_score` — final TTB score: `time_score_velocity × terrain_diversity × behavior_diversity`

Hosts at the top of the sorted output (highest `ttb_score`) are the candidates for analyst review. The velocity extension causes hosts with high temporal compression — a small number of distinct contexts visited in a very short window relative to peers — to surface even when their base `time_diversity` is similar to legitimate peers. This is the property that makes the extension effective against agentic AI and other machine-speed adversary behavior.

For interpretation guidance and the analytical workflow that follows from a high-scoring outlier, see Chapter 18 (TTB velocity extension) and Chapter 15 (base TTB methodology and the cluster-outlier investigation pattern).