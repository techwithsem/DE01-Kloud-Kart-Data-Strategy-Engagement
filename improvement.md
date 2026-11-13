# Improvement beyond the brief: the customer data register

## What I built

A customer data register, filled in and ready to use. It is printed in full at the end of this file ("The register"), so Nana can copy it into any shared document the day the plan is approved. It has four parts:

1. **Today at a glance.** Seven numbers Nana updates once a month: places holding customer data, how many a deletion cannot reach today, customer lists still held, lists past their delete-by date, open deletion requests, days left on the DPC complaint, and compliance basics not done.
2. **Copy register.** Every place customer details live, including laptops, email, developer test copies, the courier and the payment provider, with an owner, how a deletion reaches it, and whether it works today. On day one, 8 of the 12 rows read "No" or "Don't know".
3. **Export log.** One line per customer list taken, with a delete-by date 30 days later. A worked example shows Marketing how to fill it in.
4. **Deletion requests and compliance basics.** The DPC complaint is already entered with its legal deadline (1 March 2027) and our target. Eight Act 843 basics follow (registration, renewal, certified supervisor, retention policy, breach plan, outside companies, marketing consent, staff training), each with how Nana checks it, an owner and a date.

## The gap it closes

The brief says Marketing exports customer lists "with no tracking of how many copies exist" and gives 90 days to answer a complaint about a deleted customer's data, but no deliverable asks about either. The new architecture controls where data lives from now on. It does nothing about the copies already on laptops and in inboxes, which are what the complaint is about. The register is the inventory the reply needs, plus the habit (log it, date it, delete it) that stops new copies piling up.

## What it is worth to Nana

- She answers the DPC with a list of places cleared, each dated, rather than a promise.
- She can say how many copies of the customer list exist, which nobody can today.
- Every deletion request has a written deadline and a named owner.
- "Are we compliant?" gets a specific answer, with dates.
- It costs nothing and needs no new tool, hire or engineer, so it can start the week the plan is approved.

It targets the top-ranked risk in the diagnosis: GH¢30k to 150k of exposure, including about GH¢18k in notices if a 300,000-person list leaks.

## Limits

It only knows what people write in it, so the first Q1 job is collecting every existing list from every team. Nothing in it updates by itself: dates and counts are kept by hand, and Nana's monthly check is what keeps it honest. Once the data engineer starts, it can move into the reporting database if the team wants reminders sent automatically. The compliance rows are my reading of Act 843 and the DPC's guidance, not legal advice.

## The register

Every place a customer's personal details live, who owns each one, and how a deletion reaches it. Nana Adjei keeps it. The data engineer updates it from February 2027. Last reviewed: 1 December 2026.

### Today at a glance

| Check | Today | Target |
|---|---|---|
| Places that hold customer data (including 2 planned) | 12 | Falls as copies are retired |
| ...of which a deletion cannot reach today | 8 | 0 by 26 Feb 2027 |
| Customer-list exports still held | 1 logged; unknown in total | Every list logged by 31 Jan 2027 |
| ...of which past their delete-by date | 0 | 0 |
| Open deletion requests and complaints | 1 (the DPC complaint) | 0 |
| Days left to answer the DPC complaint | 90 (deadline 1 Mar 2027) | Reply signed by 26 Feb 2027 |
| Compliance basics not yet done | 8 of 8 | 0 by 30 Jun 2027 |

### How to use it

1. **Someone takes a customer list out of any system.** They add a line to the export log the same day. It must be deleted within 30 days.
2. **A customer asks to be deleted, or the DPC writes.** Add a line to deletion requests. Clear every place in the copy register marked "Yes", write the date in each, then close the request.
3. **Once a month,** Nana updates the "Today" column above. Any number that should be zero and is not has an owner's name next to it below.
4. **A new system stores customer details** (a new SMS tool, a new courier). It gets a row in the copy register before it goes live.

### 1. Copy register

| # | Where the data is | What it holds | People | Owner | How a deletion reaches it | Reachable today? | Status | Review by |
|---|---|---|---|---|---|---|---|---|
| 1 | Live shop database (MySQL) | Names, phones, emails, addresses, orders | ~300,000 | Developer | Delete or anonymise the customer row and linked addresses | Yes | Active | 26 Feb 2027 |
| 2 | Database backups | Full copy of the live database | ~300,000 | Developer | Backups kept 35 days, then destroyed | Yes | Active | 26 Feb 2027 |
| 3 | Nana's Monday CSV exports | Orders with names and phones | ~50,000 | Nana Adjei | Find and delete every export file; list them first | Don't know | Active | 31 Jan 2027 |
| 4 | Nana's Monday spreadsheet | Order lines, some names | ~50,000 | Nana Adjei | Remove customer columns; keep totals only | Don't know | Active | 31 May 2027 |
| 5 | Finance revenue spreadsheet | Order lines, payment references, names | ~50,000 | Finance lead | Remove customer columns; keep totals only | Don't know | Active | 31 May 2027 |
| 6 | Marketing customer lists | Names, phones, emails | Up to ~300,000 per copy | Marketing lead | Each copy logged in the export log and deleted by its date | No | Active | 31 Jan 2027 |
| 7 | Email attachments | Any of the above, sent between staff | Unknown | Each sender | Search mailboxes for export attachments and delete | No | Active | 26 Feb 2027 |
| 8 | Developer test copies | Copies of the database for testing | Unknown | Developer | Replace with copies that have customer details scrambled | Don't know | Active | 26 Feb 2027 |
| 9 | Courier / delivery partner | Names, phones, delivery addresses | Unknown | Operations | Written request; ask for confirmation | Don't know | Active | 26 Feb 2027 |
| 10 | Payment / mobile money provider | Phone numbers, payment references | Unknown | Finance lead | Provider's own process; some records kept by law | Don't know | Active | 26 Feb 2027 |
| 11 | Reporting copy of the database (planned, ADR-001) | Read-only copy of the live database | ~300,000 | Data engineer | Automatic: follows the live database within seconds | Yes | Planned | 18 Jan 2027 |
| 12 | Reporting database (planned, ADR-004) | Orders and a customer reference number only; no names, phones, emails or addresses | ~300,000 | Data engineer | Delete the reference number's rows | Yes | Planned | 31 Mar 2027 |

Notes on the rows:

- **Row 1** is the source of truth. Everything else must follow it.
- **Row 2:** check the 35 days is really what happens. Any older backup found anywhere goes to Nana.
- **Rows 3 to 5** stop once the reporting server and the Monday report replace them (roadmap Q1 and Q2).
- **Row 6:** nobody knows how many copies exist today. The first Q1 job is to collect and log them all.
- **Row 7:** ask the email administrator whether a mailbox-wide search is possible.
- **Rows 9 and 10** are outside companies that process data for Kloud Kart. As I read Act 843 (s.30), their security should be checked.

### 2. Export log

One line per customer list taken out of any system. Delete-by is the date taken plus 30 days. Status is "Held", "Deleted" or "OVERDUE".

| Date taken | Taken by | Team | What it holds | People | Why | Where kept | Delete by | Deleted on | Status |
|---|---|---|---|---|---|---|---|---|---|
| 2 Dec 2026 | Example: Ama | Marketing | Names and phones of Accra buyers, last 6 months | 42,000 | December SMS promotion (opted-in customers only) | Shared drive, Marketing folder | 1 Jan 2027 | | Held |
| | | | | | | | | | |

### 3. Deletion requests

The legal deadline applies to DPC complaints (90 days from receipt). Our own target for every request is 5 working days.

| Request | Received | Customer | Kind | Legal deadline | Our target | Places to clear | Completed on | Status | Notes |
|---|---|---|---|---|---|---|---|---|---|
| DPC-2026-01 | 1 Dec 2026 (assumed) | From the DPC letter | DPC complaint | 1 Mar 2027 | 8 Dec 2026 | 10 active rows | | Open | Aim to send the signed reply by 26 Feb 2027 |
| TEST-2027-01 | 2 Aug 2027 | Dummy customer | Deletion test | n/a | 9 Aug 2027 | All active rows | | Planned | Roadmap Q3: done everywhere within 5 working days |

### 4. Compliance basics

My reading of Act 843 and the DPC's guidance; not legal advice.

| What Act 843 or the DPC expects | How Nana checks it | Status | Owner | Due |
|---|---|---|---|---|
| Kloud Kart is registered with the DPC (s.27) | Finds Kloud Kart on the DPC's public register with a valid expiry date | Not started | Nana Adjei | 31 Jan 2027 |
| Registration renewed every two years (s.50) | The expiry date is written in this row | Not started | Nana Adjei | From the register |
| A trained, certified data protection supervisor is named (s.58) | The person's name and the date of their DPC certificate | Not started | CEO | 31 Mar 2027 |
| Written retention policy for each kind of customer data (s.24) | One signed page; every copy-register row has a review date | Not started | Nana Adjei | 30 Apr 2027 |
| Breach log and incident plan: who tells the DPC and customers, and how fast (s.31) | A one-page plan and a log, even if the log is empty | Not started | Data engineer | 30 Apr 2027 |
| Outside companies that handle customer data are listed and checked (s.30) | Courier and payment rows have notes from the check | Not started | Operations | 31 May 2027 |
| SMS or email marketing only to customers who agreed | An opt-in field exists, and every export-log line says it used it | Not started | Marketing lead | 31 Mar 2027 |
| Staff who handle customer data are trained at least once a year | Date of the last training for each team | Not started | Nana Adjei | 30 Jun 2027 |

Sources: Act 843 (sections as cited); DPC registration and compliance pages (dataprotection.org.gh); DLA Piper, Data Protection Laws of the World: Ghana.