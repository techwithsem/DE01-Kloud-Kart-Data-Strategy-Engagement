# ADR-002: Kloud Kart's numbers live in a small reporting database on its own server this year, not in a cloud warehouse or a lakehouse

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision owner:** Data consultant, Kloud Kart engagement

## Context

The data is about 40 GB, all structured, growing 15 to 20% a year (under 85 GB in four years). The questions are known and weekly. Investors mentioned "a warehouse, or maybe a lakehouse"; the developer wants Snowflake or BigQuery; Finance flinched. Nobody on staff has run a warehouse. A DPC complaint is open, so every new place customer data goes must be one we can find and delete from.

## Decision

We run a small relational reporting database on the second server, beside the copy from ADR-001. Every night it is rebuilt into a fixed set of agreed tables: orders, revenue by the signed definition, a stock snapshot, and customers by reference number. All reports read those tables. No cloud warehouse, data lake or lakehouse for the next twelve months.

## Alternatives considered

### Alternative A: Buy a managed cloud warehouse now (Snowflake or BigQuery)

- **How it would work:** A paid connector copies MySQL into the warehouse each night, and reports read from it.
- **Why it is attractive:** No server to patch, it grows without planning, it helps with hiring, and it is probably where Kloud Kart ends up anyway, which saves a migration later.
- **Why rejected:** At 40 GB its scale buys nothing yet. It adds a usage bill (roughly US$1,500 to 2,500 a month with connector and reporting tool) and puts customer data abroad while the complaint is open. And this year's hard problem, agreeing what revenue means, is not something a warehouse solves.

### Alternative B: A lakehouse

- **How it would work:** Raw tables land as files in cheap cloud storage, with a table format on top for SQL and later machine learning.
- **Why it is attractive:** One copy serves reports now and forecasting later, the storage is cheap, and the open format reduces lock-in.
- **Why rejected:** It solves the cost of running a lake and a warehouse side by side, and Kloud Kart runs neither. It has the most moving parts of any option, the forecasting is a year away, and raw files kept "just in case" are hard to delete from.

## Consequences

- **What this buys:** One agreed set of numbers, cheap to run (about GH¢35k to 50k a year for servers, tools and compliance), in plain SQL that both hires and the developer can read. Customer data stays on Kloud Kart's servers.
- **What this costs:** We patch, back up and size the server ourselves, which takes some of the data engineer's time. A later move to the cloud means moving the tables, though plain SQL moves easily.
- **What it costs the client if I am wrong:** If growth far outpaces 20%, nightly rebuilds run into the morning and Nana's report arrives late for a few weeks while we move. No data is lost and the shop is not slowed.
- **What would make me revisit this:** The rebuild taking over two hours; data over 200 GB; a large new source that is not MySQL; forecasting approved; or the November 2027 cost paper favouring the cloud.

## What this survives

It survives volume doubling (100 GB is still small for one server), a hire leaving (plain SQL anyone can read), and more pressure for real-time (see ADR-003). It does not survive many new sources arriving at once, or Nigeria splitting into its own company with its own systems. Either would make a cloud warehouse the next step, recorded in a new ADR.
