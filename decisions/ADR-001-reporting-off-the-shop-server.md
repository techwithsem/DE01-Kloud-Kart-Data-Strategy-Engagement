# ADR-001: All reporting reads from a copy of the database on the second server, never from the shop's machine

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision owner:** Data consultant, Kloud Kart engagement

## Context

MySQL (about 40 GB) shares one machine with the live website. Monday exports run against it, and twice last month one slowed the shop to a crawl, costing an estimated GH¢10,000 in orders each time. A second server or read replica is pre-approved. Whatever we build must be run by two new hires and one busy developer.

## Decision

We put a read-only copy (replica) of the live database on the second server. Every report, export and Finance query reads the copy. Only the shop's own code reads the live database, and that includes stock at checkout.

## Alternatives considered

### Alternative A: Keep one database and run a throttled export at 02:00

- **How it would work:** A script runs the export in small batches at the quietest hour.
- **Why it is attractive:** No new machine and nothing new to learn. Live this week, and it would probably stop most slowdowns at today's volume.
- **Why rejected:** It moves the collision instead of removing it. Late mobile orders, Nigeria and 20% yearly growth all shrink the quiet window. People would also keep export access to the live database, which is how untracked customer files spread. With the server already approved, the full fix costs about the same.

### Alternative B: Move everything to one bigger machine

- **How it would work:** Spend the approved budget on a larger server for the shop and the database.
- **Why it is attractive:** Simpler than two machines, the developer already knows it, and checkout gets faster every day.
- **Why rejected:** Reports and customers would still share one machine, just with more room before they collide. It does nothing about who can read customer data, and one failure would take down the shop and every report together.

## Consequences

- **What this buys:** No report can slow the shop again, at any hour. Nana and Finance read the same rows. Far fewer people can touch the live database.
- **What this costs:** One more server to patch and watch (GH¢21k to 35k a year, shared with ADR-002). The copy holds customer data, so it goes on the customer data register, although it deletes automatically when the live database does.
- **What it costs the client if I am wrong:** If anyone ever points the stock check at the copy, a minute of lag on a busy day means selling goods that are gone, then refunds and support calls. That is why stock stays on the live database, and why that rule is written here.
- **What would make me revisit this:** The copy lagging over 30 seconds at peak for a week; reports the copy cannot answer within five minutes; or the second server running out of room.

## What this survives

It survives order volume doubling and Nigeria becoming a full market, because the shop's path is unchanged and reporting grows on its own machine. It survives the developer leaving, because no report depends on one laptop. If the board pushes harder for real-time, the copy runs seconds behind and can feed a live figure. It does not survive stock needing to be live in a second system, for example a separate Nigerian warehouse; that would supersede this record.
