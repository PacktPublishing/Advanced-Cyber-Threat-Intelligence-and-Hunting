# Chapter 15 — Behavioral Clustering for Zero Day Detection

This directory contains the ES|QL implementation of the base Time-Terrain-Behavior (TTB) framework introduced in Chapter 15. TTB scores each entity on three orthogonal dimensions of operational diversity — *when* it acts, *where* it shows up, and *what* it does — then multiplies them to surface entities whose footprint is broad across all three.

## Files

- `ttb-diversity-scoring.esql` — The full diversity-scoring query. Classifies each event into one of six time contexts (business hours, evening, late night, early morning, weekend day/night), one of eight terrain categories (endpoint, IDS, firewall, proxy, DNS, authentication, cloud, other), and one of seven behavior categories (auth, process execution, file ops, network comms, registry, IDS alert, other). It then computes per-host `time_diversity`, `terrain_diversity`, and `behavior_diversity` over a 30-day window, sorted so that the broadest entities surface first.

- `ttb-decomposition.esql` — Drill-down query for a specific host (`WKST-0142` in the example): returns the per-event-count breakdown across the same three dimensions, allowing an analyst to inspect *which* time/terrain/behavior combinations drove the host's TTB score. Replace the `host.name` filter with the entity under investigation.
