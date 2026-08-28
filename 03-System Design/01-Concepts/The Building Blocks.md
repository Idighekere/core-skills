---
title: "The Building Blocks"
date: 2026-08-28
type: concept
tags: [concept/system-design]
---

# The Building Blocks

A component-by-component cheat sheet for the common whiteboard vocabulary. For each: **what it does, when it shows up, what it costs.**

## Load balancer
**What:** distributes traffic across N servers.
**When:** more than one app server; health-checking; TLS termination.
**Cost:** adds a hop; must itself be HA (pair it, use DNS round-robin underneath).
**Decisions:** round-robin vs least-connections vs sticky sessions — the last one trades even distribution for session affinity.

## Cache
**What:** stores hot results so reads skip the database.
**When:** read-heavy workloads (reads ≫ writes), slow/full DB, expensive computations.
**Patterns:** cache-aside (read: check cache, miss → DB → write back; write: invalidate), write-through, TTL-based.
**Cost:** invalidation correctness is the hard tax — stale data, thundering herd on cold start, and caching the wrong thing (per-user volatile data).
**LFU vs LRU:** know the eviction policy your cache uses and why.

## Message queue
**What:** an async buffer between producers and consumers.
**When:** spikes (bursts of writes/compute), decoupling services (order service → email service), fan-out.
**Costs:** eventual consistency — the consumer lag means the system is *briefly* behind; ordering and exactly-once are hard.
**Verdict:** if a task leaves the latency-critical path, queue it.

## Databases
| Type | Good for | When to reach for it |
|------|----------|----------------------|
| Relational (SQL) | transactions, joins, strong consistency | the default; money/orders stay here |
| Key-value (Redis/DynamoDB) | fast single-key reads | sessions, hot lookups |
| Columnar (BigTable/Cassandra) | huge write volumes, time series | the 10⁵+ writes/sec tier |
| Document (Mongo) | flexible schemas | evolving shapes, denormalized reads |

**Indexing:** an index is a sorted (or hashed) structure that turns a scan into a lookup — at the cost of write amplification. Choose indexes on the query hot path.
**Replication** (copies for reads/HA) and **sharding** (horizontal split by key) are the two scaling levers; sharding kills cross-shard joins and makes resharding a project.

## The trade-off lattice
Not a table — a reflex:
- Consistency ↔ Availability (CAP: you pick which to sacrifice during partition; most systems amuse AP).
- Latency ↔ Consistency (eventual consistency is a latency/consistency deal).
- Cache correctness ↔ Cache hit helpfulness.
- Horizontal scaling ↔ Operational complexity.

## When to mention each (cheat)
| Symptom | Component |
|---------|-----------|
| Read-heavy, hot subset of data | cache (LRU, TTL) |
| Bursty writes / slow fan-out | queue |
| One DB node saturated | read replicas → then shard |
| Users across continents | CDN + region-aware serving |
| DB ahead-of-scale, joins needed | keep relational, scale vertically first |

## Related
- [How to Approach a System Design Question](How%20to%20Approach%20a%20System%20Design%20Question.md)
- [Resources](Resources.md)