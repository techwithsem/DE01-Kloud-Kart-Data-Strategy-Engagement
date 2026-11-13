# ADR-005: Hire a data engineer first and an analytics engineer second; no data scientist this year

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision owner:** Data consultant, Kloud Kart engagement

## Context

There is budget for two data hires this year, not three. The team is one PHP developer who also keeps the shop up. Investors mentioned machine learning for demand forecasting. What costs money or creates exposure now: reports slowing the shop, untracked customer files during a DPC complaint, and two revenue figures that never match.

## Decision

Hire 1, from February 2027, is a data engineer who owns safe extraction, the nightly load, backups, alerts and deletion. Hire 2, from April 2027, is an analytics engineer who owns what each number means, the reporting tables, Nana's Monday report and the checks that catch a wrong figure. No data scientist in the next twelve months.

## Alternatives considered

### Alternative A: Two data engineers

- **How it would work:** Two generalists share the platform and the reports and cover for each other.
- **Why it is attractive:** No single person who alone knows how loads work, generalists are easier to find in Accra, and it gives more building capacity for a platform starting from nothing.
- **Why rejected:** The loudest complaint is two teams computing revenue differently, and no pipeline fixes that. Someone must own the definitions and work with Nana and Finance, or that job falls back on Nana. The risk of depending on one person is handled by writing every decision down and having each hire review the other's work.

### Alternative B: A data engineer and a data scientist

- **How it would work:** The engineer builds the platform while the scientist starts on demand forecasting.
- **Why it is attractive:** It answers the investors directly, and better forecasts cut both overselling and dead stock.
- **Why rejected:** Forecasts need trusted history, and Kloud Kart cannot agree last month's revenue. The scientist would spend a year doing the analytics engineer's job without the title, and Nana would still have no Monday report. Twelve months of trusted data exist by early 2028.

## Consequences

- **What this buys:** Work that stops losses starts in February; by May someone owns the numbers and Nana is out of her spreadsheet.
- **What this costs:** No forecasting this year, which Nana must say plainly to investors. One person per role, so holidays leave gaps. The revenue definition is agreed in Q1 by Nana, Finance and the consultant, without a data hire in the room.
- **What it costs the client if I am wrong:** If investors make forecasting a condition of the next round, Kloud Kart arrives without it and the raise could be weaker. The November 2027 paper sets out when forecasting starts.
- **What would make me revisit this:** Investors making forecasting a condition; the engineer role still unfilled by March 2027 (then swap the order); or the platform needing more than one engineer by Q3.

## What this survives

It survives one hire leaving, because everything is written down and the other role keeps things running, more slowly. It survives renewed pressure for real-time, since ADR-003 needs no streaming specialist, and a slow hiring market for one quarter, because the consultant and developer can hold the Monday report. It does not survive a cut to one hire: then the engineer stays, Nana owns the definitions with a glossary, and a new ADR replaces this one.
