# ADR-003: Everything refreshes nightly except stock, which stays live on the shop's own database; no streaming system

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision owner:** Data consultant, Kloud Kart engagement

## Context

Investors asked about "real-time analytics", and a board member saw a live dashboard elsewhere. The brief says stock counts cannot go stale, because that is how Kloud Kart oversells, and that everything else can refresh daily. Checkout already reads stock live. Neither of the two hires will be on call at night.

## Decision

The reporting database is rebuilt each night from changed rows and is ready by 06:00. Stock used for selling stays on the live database and is never read from a reporting copy. Reports label stock "as at 06:00". A single "today's orders so far" figure is read from the replica every 15 minutes for the board.

## Alternatives considered

### Alternative A: Stream every stock change into the reporting side

- **How it would work:** Stock changes are captured from the database log and sent through a managed message service (Kinesis or hosted Kafka) to the reporting database.
- **Why it is attractive:** It gives the board exactly the live dashboard it asked for, managed services remove most of the server work, and it lays groundwork for live alerts such as "about to sell out".
- **Why rejected:** The one decision that needs live stock, whether checkout can sell an item, already uses live stock. A second live copy can drift from the first: a late or duplicated message shows Nana a figure that disagrees with the shop. And a streaming service never switches off. With two hires, it would take most of one person.

### Alternative B: Refresh everything hourly

- **How it would work:** The same rebuild runs every hour instead of every night.
- **Why it is attractive:** No new technology, just a schedule change, and it might satisfy the board member.
- **Why rejected:** Nothing in the brief is decided hourly. It means 24 times the load on the replica, 24 chances a day to fail, and numbers changing while Nana reads them. The 15-minute figure meets the "live" wish more cheaply.

## Consequences

- **What this buys:** A job with a start and an end that can simply be re-run. No night pager. Stock correct where it matters, at checkout.
- **What this costs:** Reports are up to a day old, and the board does not get a live stock screen. Nana must explain why in plain words: the shop already checks stock live.
- **What it costs the client if I am wrong:** If some decision really needs same-day numbers, such as flash-sale pricing, the team acts on yesterday's data for a few weeks until we add a faster feed. That means some mispriced promotions, not oversold stock.
- **What would make me revisit this:** Someone naming a decision they make several times a day that reports cannot serve; overselling rising while stock stays live (a warehouse counting problem, not a data one); or a second stock location whose counts must combine with Ghana's while customers buy.

## What this survives

It survives volume doubling (nightly loads of changed rows grow slowly), a hire leaving (a batch job is easy to hand over), and the board pressing again for real-time, because there is a concrete answer: stock is live, today's orders update every 15 minutes, and everything else is decided weekly. It does not survive a second warehouse whose stock must be live across two countries.
