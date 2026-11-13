# ADR-004: Names, phone numbers, emails and addresses are removed before data reaches the reporting database

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision owner:** Data consultant, Kloud Kart engagement

## Context

Kloud Kart has 90 days to answer a DPC complaint about a deleted customer, and cannot list where that customer's data went because exports copied it onto laptops and inboxes. The reporting database (ADR-002) would be one more place customer data lives. Reports need to count customers, repeat buyers and regions, but none needs to know who anyone is.

## Decision

The nightly load drops names, phone numbers, emails and addresses and replaces each customer with a reference number. Everything else lands as it is, and the business logic is built in SQL inside the reporting database. Hand-made exports of customer records stop. Marketing lists are still allowed, but each is logged in the customer data register with an owner and a delete-by date.

## Alternatives considered

### Alternative A: Load everything raw, transform later

- **How it would work:** Copy every column, personal details included, and build the tidy tables from that raw layer.
- **Why it is attractive:** Nothing is lost. When the revenue definition changes, we rebuild without touching the live database. Marketing's lists would sit inside a secured system instead of on laptops.
- **Why rejected:** It doubles the full copies of 300,000 people's details in a layer that grows forever, and Act 843's retention principle (s.24) is something we would have to defend during an open complaint. The rebuild benefit mostly survives anyway, because the replica holds every order.

### Alternative B: Shape everything before loading

- **How it would work:** All logic, including revenue, runs in a script, and the reporting database receives only finished tables.
- **Why it is attractive:** The smallest possible copy, with nothing personal or raw for the DPC to ask about.
- **Why rejected:** The revenue rule would live in a script only the engineer reads, and every change would mean re-extracting. The analytics engineer and Finance need to read and question that rule in plain SQL. Only a handful of identifying columns need removing early.

## Consequences

- **What this buys:** Deleting a customer touches three places: the live database (the replica follows in seconds), backups (expire after 35 days) and one reference number. The DPC can be shown that list. Nana, Finance and the reporting tool connect only to the reporting database, so a mis-sent report or a lost laptop carries no identities.
- **What this costs:** Reports cannot show names, so "our top 20 buyers by name" needs an approved, logged extract. Duplicate accounts must be matched during the load, before the identifiers are dropped.
- **What it costs the client if I am wrong:** If Marketing needs named lists every week, people will work around the rule, CSVs come back, and the DPC exposure grows again without anyone noticing.
- **What would make me revisit this:** More than two approved named extracts a month for three months running; a decision to run campaigns from the reporting side; or legal advice that the reference numbers need more protection.

## What this survives

It survives a second or third DPC complaint, because "where is this person's data?" has a short list we already keep. It survives a leaked report and a hire leaving, because the rule sits in the load and in this record. It does not survive Kloud Kart deciding to run marketing straight from its data platform.
