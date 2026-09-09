---
title: Queue
date: 2026-09-02
type: data-structure
tags:
  - ds/queue
---

# Queue

## What it solves
What problem does this data structure solve?
FIFO ordering - The first item or data that entered is the first to be worked on or removed
## Intuition
People at a fuel station, customers queuing at a bank, the first person that is on the queue will be attended to first. 

| Where                                          | What forms the structure                                         | Which one, and why                                                                 |
| ---------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **A printer spool, or a support ticket queue** | Jobs in arrival order                                            | Queue: fairness, so that no job is starved                                         |
| **A web server under load**                    | Incoming requests waiting for a worker                           | Queue: a bounded one, so that overload sheds requests instead of exhausting memory |
| **Audio playback and network packets**         | A fixed-size ring buffer of samples or frames                    | Queue: a circular one, because the memory budget must be known in advance          |
| **Google Maps finding the fewest transfers**   | The expanding frontier of a breadth-first search                 | Queue: BFS is built on a queue, and that is why the route is shortest              |
| **An API rate limiter**                        | Timestamps of recent calls, with old ones expiring off the front | Queue: requests expire in the order they arrived                                   |
## Memory model
How is it actually organized in memory?

## Operations

| Operation | Time | Why? |
|-----------|------|------|
| Access | | |
| Search | | |
| Insert | | |
| Delete | | |

## Idioms & gotchas
The stuff that always causes bugs: pointer order, edge cases, empty state. Code idioms to memorise.

## Patterns it enables
- Which [[pattern]]s usually use this data structure?
- Related problems: links into `04-Problems/`.

## Variations
Data structures that build on this one — each gets its own note + cross-link.

## Pitfalls
Misconceptions + common mistakes.