# Kloud Kart: what to build first, and what it costs

Recommendation to Nana Adjei and the board. 13 November 2026.

## The recommendation

I would suggest move all reporting onto the pre-approved second server, so no report can slow the shop again. On it, keep a read-only copy of the live database and a small reporting database that rebuilds Kloud Kart's numbers every night from one definition of revenue, signed by Nana and Finance. Stock stays on the live database, where checkout already reads it on every order. Names, phone numbers, emails and addresses are removed before data reaches the reporting database, and a register lists every place customer data lives, so the Data Protection Commission (DPC) complaint gets an answer backed by evidence. Hire a data engineer first and an analytics engineer second. Buy no cloud warehouse, lakehouse or streaming system this year, and write down now what would change that.

## Why this fits Kloud Kart's size

Kloud Kart holds about 40 GB of structured records (orders, stock, customers), growing 15 to 20 per cent a year: roughly 48 GB next year, under 85 GB in four years. One ordinary server handles that. The questions repeat every week: revenue, orders, stock, repeat customers, the Nigeria pilot.

- **A warehouse, sized down.** A reporting database with a fixed, agreed layout gives fast answers to known questions. That is all Nana needs from a warehouse (ADR-002).
- **No data lake.** Lakes are cheap storage for messy files whose use is not known yet. Kloud Kart has none, and a lake is one more place a deletion must reach.
- **No lakehouse.** It cuts the cost of running a lake and a warehouse side by side. Kloud Kart runs neither.
- **No Snowflake or BigQuery yet.** Probably the right move in a year or two. Today it adds a usage bill Finance has already flinched at, puts customer data abroad while the complaint is open, and gives two new hires a platform to learn before revenue has a definition.
- **No Spark.** It spreads work across many machines; Kloud Kart's whole database fits on one.

## "Real-time": where it is needed

The test for each flow: what decision changes if this number is ten seconds old instead of a day old?

| Flow | Who decides, how often | Freshness |
|---|---|---|
| Stock at checkout | The shop, every order | Live, on the live database, unchanged |
| Today's orders so far | The board member who asked for "live" | Every 15 minutes, read from the copy |
| Revenue, orders, categories | Nana and Finance, weekly | Rebuilt nightly, ready by 06:00 |
| Customers, repeat buyers, Nigeria | Marketing and management, monthly | Rebuilt nightly |

Only stock passes, and it is already live. Streaming it elsewhere would create a second, less reliable copy of a number the shop already has right, and someone would have to watch it at night. The 15-minute figure gives the board something live almost for free (ADR-003).

## Customer data and Act 843

Each night only changed rows are pulled from the copy. Identifying fields are dropped and each customer becomes a reference number (ADR-004). Deleting a customer then means the live database (the copy follows in seconds), backups (expire after 35 days) and one reference number. Ad-hoc exports of customer records stop; Marketing's lists are logged in the register with an owner and a delete-by date. As I understand Act 843, this answers what the DPC will ask: where the data is, who owns each copy, and how a deletion reaches it. Kloud Kart should also confirm its DPC registration and train a manager as its data protection supervisor.

## The two hires, in order

**Data engineer, from February 2027.** Owns getting data out without touching the shop, the nightly rebuild, alerts, backups and deletion. SQL and MySQL administration matter more than cloud certificates. First 90 days: take over the copy and reporting server; nightly loads with a 07:00 failure email; deletion tested on a dummy customer; every register row cleared or owned.

**Analytics engineer, from April 2027.** Owns what each number means: the reporting tables built from the signed definition, Nana's Monday report and the checks that catch a wrong figure. First 90 days: retire Nana's spreadsheet; match a full month with Finance to the cedi; a one-page glossary of every figure; the Nigeria view.

The engineer comes first because the two problems costing money now are engineering ones. The revenue definition is cheap, so Nana and Finance agree it with me in Q1, before the second hire arrives (ADR-005).

**Not staffed:** demand forecasting, which needs twelve months of trusted history (early 2028); buyers keep reordering from the stock report. And night cover: if the rebuild fails, the Monday report is a day late and the alert says so, while the shop is unaffected.

## What it costs

| Item | Per year |
|---|---|
| Second server (pre-approved): copy and reporting database, US$150 to US$250 a month | GH¢21,000 to GH¢35,000 |
| Offsite backups | GH¢3,600 |
| Reporting tool (free, open source, same server) | GH¢0 |
| DPC registration (GH¢2,340 every two years) and supervisor training (assumed) | GH¢10,000 one-off, then GH¢1,200 |
| **Servers, tools and compliance** | **GH¢35,000 to GH¢50,000** (a cloud stack: GH¢210,000 to GH¢350,000) |
| Two data hires at an assumed GH¢15,000 to GH¢25,000 a month each, fully loaded | GH¢360,000 to GH¢600,000 (less this year) |

Against that, the plan stops an estimated GH¢240,000 a year of sales lost to slowdowns, gives Nana back five working weeks a year, and lets Kloud Kart answer the DPC with evidence.

## What I could not verify

Things that could not be verified include, the average order value, how long slowdowns lasted and how many customers left, whether Kloud Kart is on the DPC register, when the complaint's 90 days began, where the server is hosted, local salaries, and how Finance defines revenue. Each is a numbered assumption in the diagnosis, with what changes if it is wrong.
