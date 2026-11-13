# Improvements

## What I built

A customer data register, filled in and ready to use (below). It lists every place Kloud Kart holds customer details, who owns each one, and whether a deletion can reach it today. Alongside it sit three short logs: customer lists taken out (each deleted within 30 days), deletion requests (the DPC complaint is entered, deadline 1 March 2027), and eight Act 843 basics with an owner and a date each.

## The gap it closes

The brief says Marketing exports customer lists “with no tracking of how many copies exist” and gives 90 days to answer a complaint about a deleted customer’s data, but no deliverable asks about either. The new architecture controls where data lives from now on; it does nothing about copies already on laptops and in inboxes. The register is the inventory the DPC reply needs, plus the habit (log it, date it, delete it) that stops new copies piling up.

## What it is worth to Nana

- She answers the DPC with a dated list of places cleared, not a promise..
- She can say how many copies of the customer list exist, which nobody can today..
- It costs nothing and needs no hire, so it starts the week the plan is approved..

It targets the top-ranked risk in the diagnosis: GH¢30k to 150k of exposure.

## Limits

It only knows what people write in it, so the first Q1 job is collecting every existing list. It is kept by hand; Nana’s monthly check keeps it current. The compliance items are my reading of Act 843, not legal advice.

## The register

Day one: 12 places hold customer data (2 planned), 8 a deletion cannot reach today, 0 of 8 legal basics done.

| # | Where the data is | Owner | How a deletion reaches it | Reachable today? |
|---|---|---|---|---|
| 1 | Live shop database | Developer | Delete or anonymise the customer row | Yes |
| 2 | Database backups | Developer | Kept 35 days, then destroyed | Yes |
| 3 | Nana's Monday CSV exports | Nana Adjei | Find and delete every file | Don't know |
| 4 | Nana's Monday spreadsheet | Nana Adjei | Remove customer columns | Don't know |
| 5 | Finance revenue spreadsheet | Finance lead | Remove customer columns | Don't know |
| 6 | Marketing customer lists | Marketing lead | Log each copy; delete within 30 days | No |
| 7 | Email attachments | Each sender | Search mailboxes and delete | No |
| 8 | Developer test copies | Developer | Replace with scrambled copies | Don't know |
| 9 | Courier | Operations | Written request, with confirmation | Don't know |
| 10 | Payment / mobile money provider | Finance lead | Provider's process; some records kept by law | Don't know |
| 11 | Reporting copy (planned, ADR-001) | Data engineer | Follows the live database in seconds | Yes |
| 12 | Reporting database (planned, ADR-004) | Data engineer | Holds a reference number only; delete its rows | Yes |

Act 843 basics, all “not started”: DPC registration (s.27, by 31 Jan 2027), renewal every two years (s.50), certified data protection supervisor (s.58, by 31 Mar), retention policy (s.24, by 30 Apr), breach plan (s.31, by 30 Apr), outside companies checked (s.30, by 31 May), marketing only to customers who agreed (by 31 Mar), yearly staff training (by 30 Jun).
