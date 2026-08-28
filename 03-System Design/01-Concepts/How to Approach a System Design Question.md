---
title: "How to Approach a System Design Question"
date: 2026-08-28
type: concept
tags: [concept/system-design, interview]
---

# How to Approach a System Design Question

## Why interviewers ask it
Design questions test **traffic-light judgment** under ambiguity: do you clarify scope before building, do you know which trade-off matters, can you zoom between 50,000 feet and one server? There is no "correct" architecture — there is a *defensible* one.

## The playbook (40–45 min)

### 1. Scope it (5–10 min) — *the most skipped, most valuable step*
**Never start drawing.** Ask until the problem shrinks:
- Who are the users? (millions? a team?)
- What are the **core** features vs. nice-to-haves? (design the MVP slice)
- What's the read/write ratio? Consistency vs availability priority?
- Do we need a mobile client, WebSockets, offline support?

The goal: a two-sentence spec so boring it can't be argued with.

### 2. Rough estimates (2–3 min)
Back-of-envelope only (order of magnitude, not exactness):
- Users/day → requests/sec → data written/day → storage over N years → read:write ratio → cache sizes.
- Keep exponents on your side: "1M DAU, each doing 10 writes → ~100 req/s write" is plenty.

### 3. High-level design (10–15 min)
Draw the happy path first: **client → load balancer → app servers → database → back to client**. Then:
- Add the read path (cache) and write path (queue) if the estimates suggest them.
- One box per component; label responsibilities; **don't over-detail yet**.

### 4. Deep dive (10–15 min) — *chosen by you, steer the conversation*
Pick the one part most likely to fail:
- If writes dominate → database scaling, sharding, replication.
- If reads dominate → caching strategy, why cache-aside works.
- If consistency matters → leader election, quorum, or event-sourcing.
- If latency dominates → CDN, geo-distribution, batching.

### 5. Wrap (2–3 min)
Trade-offs recap, what you'd add more time (monitoring, security, multi-region), and what you deliberately deferred.

## What interviewers penalize
- Designing before scoping (jumping to a huge stack for what's a CRUD app).
- Vague boxes with no *reasoning* ("add a cache" without "writes are 10% of reads").
- Ignoring trade-offs (nothing in distributed systems is free).
- Single point of failure untreated in a "scalable" pitch.

## Templates to reuse
- The four-question checklist in [README](../README.md).
- Numbers to memorize (memorize *ratios*, derive absolutes):
  - 1 DAU ≈ a few requests/day average
  - One SSD/HDD does ~10²–10³ IOPS sustained; a DB node handles ~10³–10⁴ QPS for simple reads
  - A single server: ~1k concurrent connections practical; ~100 MB/s network
  - Cache hit ratios: 80–99% for hot reads

## Related
- [The Building Blocks](The%20Building%20Blocks.md) — what each component actually buys you.
- [Resources](Resources.md) — deeper playbooks and canonical write-ups.