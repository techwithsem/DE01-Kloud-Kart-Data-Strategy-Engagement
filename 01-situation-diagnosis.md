# Kloud Kart: situation diagnosis

For Nana Adjei, Head of Operations. 13 November 2026.


## Root causes, ranked by risk

| Rank | Root cause | Symptom in the brief | Cost over a year |
|---|---|---|---|
| 1 | Customer data leaves the system as files nobody tracks or can delete | Marketing's untracked list copies; the 90-day DPC complaint | GH¢30k to 150k exposure |
| 2 | Reporting runs on the same machine as the shop | Two Monday slowdowns last month | ≈ GH¢240k lost sales |
| 3 | "Revenue" has no single written definition | Finance never matches Nana | ≈ GH¢40k staff time |
| 4 | "Real-time" was promised before anyone named the decision | Investors' question, Nana's nod, Snowflake request | GH¢210k to 350k misspend risk |
| 5 | Nobody owns data work; a person is the scheduler | Nana rebuilds numbers by hand; one developer | Multiplies the other four |

I ranked by three main questions: 

1. Can the damage be undone?
2. Is a deadline running? 
3. Is the loss already happening? 

Root cause number 1 comes first because the deadline is fixed and a failed answer to a regulator, during a funding round, cannot be taken back. Root cause 2 is the loudest problem but comes second because a lost sale can be won back next week.

### Root Cause 1. Customer data leaves the system as files nobody tracks

**Evidence:** "Marketing exports customer lists with no tracking of how many copies exist." Nana's Monday numbers come from CSV exports. Kloud Kart has 90 days to answer a complaint about a deleted customer's data.

**Causation Mechanism:** Each export copies customer records out of MySQL onto a laptop, an inbox or a drive. Deleting a customer from MySQL leaves every copy in place. No one keeps a list of copies, so no one can prove a deletion, and that is what the complaint asks for.

**Cost:** Act 843 fines are small (150 penalty units, about GH¢1,800, for ignoring an enforcement notice). The real costs are legal advice (A7: GH¢20k to 80k), about 80 hours of management time (A8), and, if a 300,000-person list leaks, notifying the DPC and every person affected: about GH¢18,000 in SMS alone (A9). Total GH¢30k to 150k, before any damage to reputation.

### Root Cause 2. Reporting runs on the same machine as the shop

**Evidence:** "MySQL and the live website… share one box." "Twice last month, a Monday export slowed the live site to a crawl."

**Causation Mechanism:** An export of a few hundred thousand rows competes with checkout for the same disk, memory and processor. Nothing stops it running at a busy hour, and growth of 15 to 20 per cent a year plus Nigeria makes each export bigger.

**Cost:** About 200 orders an hour (A2). One hour of slowdown (A3) with 1 in 4 customers giving up (A4) loses 50 orders, or GH¢10,000 at GH¢200 an order (A1). Twice a month that comes to GH¢240,000 of sales a year, about GH¢60,000 of margin (A6). Halve the drop-off rate and it is still GH¢120,000, more than double the plan's running cost.

### Root Cause 3. "Revenue" has no single written definition

**Evidence:** "Finance's spreadsheet never matches hers." Each side builds its own numbers from exports.

**Causation Mechanism:** Each team exports at a different time and applies its own formulas. Most likely one counts an order when it is placed and the other when it is paid, and they treat refunds and delivery fees differently (A5). Both numbers can be correct and still disagree. Cleaning the data will not fix this; writing the definition down once and calculating it in one place will.

**Cost:** Nana spends about 4 hours every Monday (A10), 200 hours a year, which is five working weeks. Finance spends about a day a month reconciling (A11). At A12 rates that is about GH¢40,000 a year. The larger cost: every number the board sees can be argued with.

### Root Cause 4. "Real-time" was promised before anyone named the decision

**Evidence:** Investors asked about "real-time analytics" and Nana nodded. A board member wants live dashboards. The developer wants to "just buy Snowflake or BigQuery", and Finance flinched. The brief also says "Stock counts can't go stale… everything else can refresh daily."

**Causation Mechanism:** The phrase came from a pitch meeting, not from a decision. Ask of each number "would anyone act differently if it were ten seconds old instead of a day old?" and only stock passes, and stock is already live at checkout.

**Cost:** No loss yet. A cloud warehouse, an ingestion tool, a paid reporting tool and a streaming service would cost about GH¢210k to 350k a year (A13), 4 to 10 times this plan, and need more staff than two hires.

### Root Cause 5. Nobody owns data work

**Evidence:** One PHP developer; "nobody else has run a warehouse or written Python professionally"; Nana rebuilds the numbers by hand.

**Causation Mechanism:** No owner, no schedule, no checks. The developer keeps the shop running, so data work always comes second. I have not costed this one separately because it makes the other four worse, and the two hires fix it.

**One thing to protect: stock freshness.** The shop reads stock live today, so it is not broken. The risk is a new dashboard or reporting copy, refreshed overnight, being used for selling. Every recommendation keeps stock on the live database.


## Discovery questions I would have asked Nana

| # | Question | What I assumed | If wrong, what changes |
|---|---|---|---|
| Q1 | Last Monday at 07:00, what did you look at first, and what did you decide? | Weekly revenue, category sales and stock to reorder | If mostly stock, the Q2 report leads with stock |
| Q2 | When does an order count as revenue for you, and for Finance? Refunds? Delivery fees? | A5: placed for Nana, paid for Finance | Agreeing one definition becomes a negotiation, not one meeting |
| Q3 | If this morning's report showed yesterday's numbers, what would you do differently? | Nothing, except for stock | A real hourly decision gets a near-live figure from the replica |
| Q4 | When exactly did the DPC complaint arrive? | 1 December 2026, so the reply is due 1 March 2027 | If earlier, the customer data work starts the day the plan is approved |
| Q5 | Is Kloud Kart registered with the DPC, and who signs data protection letters? | Status unknown; Nana is accountable | If registered with a supervisor, two Q1 tasks drop out |
| Q6 | Which wrong number would embarrass you in front of the board? | Revenue and order counts | Changes which checks are built first |
| Q7 | Will the Nigeria pilot become a separate company? | No; under 10% of orders this year | A separate company brings the cloud warehouse decision forward |
| Q8 | When did the shop last oversell, and how did you find out? | A few times a week, found by complaints | If frequent, the stock accuracy check moves to Q2 |

The assumption I am most exposed to is A5. If Finance and Nana use different but defensible rules, agreeing one could take weeks, so the roadmap gives it a full meeting and a signed page.

## Assumptions

NB: I have named the assumptions with the representing letter A before the assumption number.

- A1. An average order is GH¢200: about GH¢290m a year, far above the GH¢5m "large controller" line even if A1 is ten times too high.
- A2. 75% of orders arrive 08:00 to 22:00, so about 200 an hour.
- A3. Each slowdown lasted about an hour.
- A4. 1 in 4 customers gives up on a slow checkout.
- A5. Nana and Finance use different revenue rules.
- A6. Gross margin is 25%.
- A7. Legal advice on the complaint costs GH¢20k to 80k.
- A8. About 80 hours of management time without a copy inventory.
- A9. SMS costs about GH¢0.06 a message.
- A10. Nana spends 4 hours every Monday.
- A11. Finance spends one day a month reconciling.
- A12. Loaded cost is GH¢145 an hour for Nana and GH¢100 an hour for Finance.
- A13. Cloud list prices are converted at GH¢11.65 to the US dollar.


