# Renewal Defender: write the challenger's attack sheet before the challenger does

Reshmi Nandy · 15 November 2026

Losing a new strategic deal is bad. Losing the renewal of an account we already run is the loss we should never accept, because we had years of access, data and relationships that no competitor had. That is why the renewal analysis has to start at least 12 months before the contract expires.

I worked on several renewal deals over the past year, and I am working on more now. Renewal Defender is the skill I built from that work. Client partners, CRMs and Market Delivery Unit (MDU) leads can use it to run a full analysis of an account well ahead of its renewal. If I am lucky, others will take it and make it better in the coming days.

## A renewal is decided on OpEx and a 6 to 12 month baseline

A typical managed-services term runs five years. When it comes up for renewal, the customer's first question is how much lower the operating cost can go. The challenger answers with a number. We answer with our track record, and the customer weighs only the past year of it, sometimes only the past six months.

The challenger's number is an estimate, because they have never seen the estate. We hold the tickets, timesheets and governance minutes. We are the only bidder who can show where the customer's money goes today.

Renewal Defender is a Claude skill that reads the account's delivery data and reconciles the hours we deployed against the work that was produced. The difference is leaked effort: hours that were paid for and cannot be traced to a ticket, a release or a reported activity. The skill finds where those hours went and who owns the cause, whether that is us, the customer or a third party. It then writes the attack sheet a challenger would build from the same facts, with our defence for each line.

A customer who wants lower OpEx needs that X-ray of their operations before agreeing a price. If we do not provide it, the challenger will offer a reduction without it, and we will be asked to match a number nobody can explain.

## What the market evidence says

Incumbency stopped being a safe position about a decade ago, and the latest data shows no recovery. I keep the published evidence apart from my own observations.

Published evidence:

- In a 2017 review of nearly 200 ISG-advised deals involving an incumbent, buyers took about two-thirds of expiring contracts to competitive bid. The incumbent lost the whole deal in 47% of cases and part of it in a further 7%. A decade earlier, the same firm put incumbent retention at about 80% ([CIO Dive, March 2017](https://www.ciodive.com/news/the-incumbents-dilemma-enterprises-changing-service-providers-at-an-accel/437495)).
- The top three reasons buyers gave for reopening a contract were better terms, better delivery performance, and getting out of a deteriorating relationship.
- In the first quarter of 2026, ISG reported renewal and extension contract value of $3.2 billion, down 24% year on year, while new-scope value rose 20% to a record $8 billion. ISG's reading is that incumbency has become a disadvantage, as clients take scope to market to cut cost and consolidate providers ([ISG Index Insider, May 2026](https://isg-one.com/articles/index-insider--the-incumbents--disadvantage)).

Two observations from my own deal work shape the design. We generally receive about three months of detailed data to evaluate a 60-month engagement. And the renewal decision turns on the past year of delivery, sometimes only the past six months, whatever the earlier years looked like.

The 2017 figures are dated and come from advisor-led deals, which skew competitive. I use them for direction only. Our own retention rate on eligible renewals is the benchmark that matters.

## What Renewal Defender does

Renewal Defender is a markdown instruction file that a pursuit lead runs in an AI chat window against the account's own documents. It needs no new platform and no integration.

| | |
| --- | --- |
| **Who runs it** | Client partner or pursuit lead, with the delivery head in the room |
| **When** | 12 to 18 months before expiry, then every quarter on a rolling 3 to 6 month window |
| **What goes in** | Monthly SLA and governance reports for the last 12 months, a ticket extract for the last 3 to 6 months, headcount and timesheet hours by role, release and deployment records, the original contract, and the productivity norm in force today |
| **What comes out** | 12-month baseline, effort reconciliation, leak map by cause and owner, challenger attack sheet with defence plan, and an operations X-ray that can be shared with the customer |
| **What it never does** | Assume a productivity norm, invent a counter-proof, name an individual as the cause, or leave the account boundary |

The productivity norm is the one input the skill will not estimate. Norms differ by work type and they change, sometimes within a year. The skill asks for the norm in force today, in hours per unit of work for each category, and stops if it is not supplied.

## Three windows: 12, 6 and 3 months

The skill reads the last 12, 6 and 3 months, and nothing older unless the user asks for it. Each window has its own job.

| Window | What the skill reads | What it answers |
| --- | --- | --- |
| Last 12 months | Monthly SLA and governance reports. MTTR, headcount and volume by month. DORA measures where we run development: deployment frequency, lead time for changes, change failure rate, time to restore | The baseline and the direction of travel, with seasonal peaks and release cycles visible |
| Last 6 months | Ticket extract, timesheets, release log | The steady state the customer experiences now, and whether the latest quarter is typical |
| Last 3 months | The same extract, most recent quarter | The reconciliation of hours against work, and the leak |

I leave the earlier years out on purpose. Over a 60-month term the technology, the customer's programme plan, its objectives and the leadership on both sides have usually changed, so year 1 is not a fair baseline for year 5. DORA is tracked over long periods in product companies. In BAU development under a managed-services contract, vendors read it over 3 to 6 months, and a year at most.

The original contract is still read once, for the scope and the commitments that remain in force. It is not used as a performance baseline.

## The data form

The skill starts from one form. It has a minimum version that lets the skill run, and a best version that lets it run with full confidence. The account team fills in what it has and marks the rest as not available.

| Section | Minimum template | Best template |
| --- | --- | --- |
| A. Engagement | Contract start and end dates, commercial model (fixed price, time and material, managed capacity), towers in scope, analysis window | The same, plus scope boundaries with other vendors and any change requests in the last 12 months |
| B. Productivity norms | Hours per unit for each work type, the owner of the norm, the date it took effect | The same, split by priority and by application complexity |
| C. Volume | Monthly counts by work type and priority for the last 3 months | Ticket-level extract for the last 6 to 12 months, with the columns listed below |
| D. Effort | Headcount by role and month, with standard hours per person | Timesheet hours by role, week and activity code, including non-ticket codes, and the shift roster |
| E. Metrics | SLA report for the last 3 months | Monthly MTTR, SLA attainment and backlog for 12 months. The four DORA measures where we run development |
| F. Non-ticket work | A list of recurring activities with estimated hours per month, marked as estimates | Logged hours by activity, and whether each one is reported to the customer |
| G. Context | Joiners and leavers in the window, tool changes in the window | Change-board calendar, known problems with funding status, other vendors and their releases |

The best version of section C is a ticket extract with these columns:

```
ticket_id, work_type, priority, application, opened_at, resolved_at, closed_at,
pending_hours, assignment_group, reassignment_count, reopen_count,
resolution_code, effort_hours_booked
```

## What happens when data is not available

Missing data lowers the confidence of the result. It stops the run in one case only, the productivity norm.

| Missing item | What the skill does | Effect on the result |
| --- | --- | --- |
| Productivity norm | Reports actual hours per unit for each work type and stops there | No leak figure. The owner of the norm can compare and supply it |
| Ticket-level extract | Works from monthly counts | No anomaly tests and no hand-off analysis. Affected rows tagged `[GAP]` |
| Timesheets | Uses headcount multiplied by standard hours | Non-ticket work cannot be separated. The leak is reported as an upper limit |
| 12 months of reports | Works on the quarter alone | No baseline and no direction of travel |
| 6-month extract | Works on 3 months | Cannot say whether the quarter is typical. Flagged at the top of the output |
| Non-ticket log | Asks the delivery head for estimates | Estimates tagged `[INFERRED]` |
| Release records | Skips the DORA measures | Development throughput not assessed |
| Contract | Skips the scope check | Work absorbed outside scope not assessed |

Every output opens with one of three confidence levels. Full means the best template was supplied. Partial means the minimum template was supplied. Indicative means something in the minimum template was missing, and the result should not be shown to a customer.

## How it works

The method is a reconciliation that is run more than once. Each pass removes one source of error, and the gap that survives is the finding.

1. It sets the baseline. From the last 12 months of reports the skill tabulates MTTR, the DORA measures, headcount and volume by month, so the first view is whether the metrics moved while the team size did.
2. It takes the productivity norm from the user, in hours per unit for each work type. It does not proceed on an assumed norm.
3. It reconciles hours against work. Hours deployed in the last three months come from timesheets or from headcount. Explained hours are the volume of each work type multiplied by its norm, plus the non-ticket hours that were recorded.
4. It tests the source data before trusting the gap. A gap can be produced by bad data as easily as by wasted effort.
5. It looks for work that was done and never registered, reported or conveyed to the customer.
6. It attributes what remains to a cause and an owner, on our side, the customer's side or a third party's.
7. It repeats steps 3 to 6 with the corrections applied, and reports the residual it still cannot explain as unexplained.
8. It writes the challenger's attack sheet from the result, and a defence for each attack.

**Leaked effort = Hours deployed − Σ (Volume of each work type × Norm for that type) − Recorded non-ticket hours**

An illustration with invented numbers: 40 people over three months deploy 19,200 hours. Their 9,000 tickets, at the norms supplied, explain 11,400 hours. Recorded non-ticket work explains 2,300. The leak is 5,500 hours, or 29% of what the customer paid for in that quarter.

The data tests in step 4 look for these anomalies:

- tickets closed in bulk at month end, or within minutes of being opened
- reopened tickets that the SLA report counts once
- SLA clocks paused in a pending status for most of a ticket's life
- monitoring alerts and duplicates counted as incidents
- priority changed after a breach
- timesheets that book the same eight hours every day whatever the volume
- tickets with no effort recorded, and hours with no ticket

The unregistered work in step 5 is usually real and useful. It includes release and hypercare support, audit and compliance evidence, recurring reports, knowledge transfer after attrition, data fixes, and requests that arrive by email, chat or phone. The customer has paid for it and has never seen it listed.

## Where leaked effort goes

The skill assigns every leaked hour it can to one of the causes below. The list will grow as accounts are run through it.

| Cause | Sits with | How it shows in the data |
| --- | --- | --- |
| Knowledge gap and attrition | Vendor | Effort per ticket rises after a team change; long handling time on repeat ticket types |
| Rework | Vendor | Reopen rate; the same fix on the same item within 30 days |
| Manual work that could be automated | Vendor | High-volume, low-variance ticket types handled by hand |
| Hand-offs between L1, L2 and L3 | Vendor | Reassignments per ticket; time in queue exceeds time in work |
| Shift cover beyond demand | Vendor and contract | Hours rostered at night and weekends against tickets raised then |
| Unregistered non-ticket work | Shared | Hours with no ticket; requests made informally and never reported |
| Wrongly stated requirements or expectations | Customer | Tickets returned for clarification; changes reworked after user testing |
| Governance bottleneck | Customer | Time waiting for change board or sign-off as a share of cycle time |
| Funding approval delays | Customer | Known problems with an identified fix and no budget; incidents recurring against them |
| User behaviour | Customer | How-to and password requests; repeat callers; tickets raised outside the tool |
| Another vendor's scope out of sync | Third party | Tickets pending with another provider; incidents that follow an upstream release |
| Access and environment delays | Customer or third party | Pending-on-access status; environment downtime booked to our tickets |
| Technical debt in the application | Customer and vendor | Top recurring incident categories traced to one root cause |
| Monitoring noise | Shared | Alerts raised and closed without action |
| Scope absorbed without a change request | Shared | Work types outside the contract appearing in the ticket extract |
| Source data errors | Tool owner | The anomalies listed in the previous section |

The split by owner matters commercially. A challenger's price assumes the whole leak is the incumbent's inefficiency. I expect that on most accounts a good part of it sits in the customer's approvals, funding decisions and user behaviour, and a new vendor would inherit that part on day one. This is a prediction the pilot has to test.

## From the X-ray to the attack sheet

The skill turns each finding into the sentence a challenger would put in a proposal or say in orals. It scores every attack from 1 to 5 on how easily the challenger can prove it, how much it costs the customer, and how visible it is to the people who decide. Each attack then gets exactly one defence, and the skill may not invent evidence to reach a stronger one.

| Defence | When it applies | What the plan contains |
| --- | --- | --- |
| Prove | The record contradicts the attack | The cited evidence, ready for the proposal |
| Reframe | The attack is true but incomplete | The missing context, stated without excuse |
| Remediate | The attack is true and still fixable | The fix, an owner, a date before the RFP |
| Concede | The attack is true and time has run out | An admission, and a priced commitment in the new term |

The rows below are invented to show the format. They describe no real account.

| Finding | The challenger's line | Score /15 | Defence | What the plan says |
| --- | --- | --- | --- | --- |
| 29% of last quarter's hours cannot be traced to a ticket | "Close to a third of what you pay for cannot be tied to any work." | 14 | Reframe, then remediate | 12 points are unregistered work the customer asked for. Register and report it monthly. The other 17 points are attributed by cause and owner |
| MTTR flat for 12 months on the same headcount | "A year on, the same team and the same resolution time." | 13 | Concede | State it first. Commit an MTTR target in the new term, backed by service credits |
| Changes wait three days in five for change-board approval | "Their people wait and you pay." | 11 | Reframe | The wait sits in the customer's governance. Propose pre-approved standard changes |
| 18% of incidents trace to one known error with no funded fix | "They keep fixing the same fault." | 10 | Prove | The problem record and fix proposal were raised in year 3 and funding was declined. Re-present it with the cost of recurrence |

Every line carries a source tag: a document reference, `[INFERRED]` where the skill reasoned beyond the record, or `[GAP]` where evidence was not supplied.

Two documents come out. The attack sheet stays internal. The operations X-ray shows effort by cause and owner without the challenger's framing, and is written to be shared with the customer.

## Why this is not the red-team skill again

We already have a deal red-team skill that simulates the client's evaluation panel. Renewal Defender differs on input, persona and timing.

| | Deal red team | Renewal Defender |
| --- | --- | --- |
| **Reads** | Our proposed solution | Our delivery data |
| **Simulates** | The client's evaluators | The competitor's bid team |
| **Runs** | During the bid, at three review stages | 12 to 18 months before the bid exists |
| **Changes** | What we write | What delivery does, and what the customer is shown |
| **Applies to** | Any deal | Renewals only |

The two chain together. The attack sheet becomes an input to the red team's first stage.

## Where this can go wrong

| Risk | Why it is real | How the design handles it |
| --- | --- | --- |
| The norm is missing or out of date | Norms change, and the whole reconciliation rests on them | The skill stops without a norm. The output records the norm used and its date |
| Three months are not representative | A release month or seasonal peak distorts volume | Check the 3-month result against the 6 and 12-month windows, and flag months with unusual volume |
| Timesheets cannot be trusted | Many teams book standard hours whatever the workload | Reconcile from timesheets and from headcount capacity, and report both |
| Service lines will not share the data | It shows their inefficiency, and they own it | The sponsor is the client partner. The minimum pack is the ticket extract, timesheets and 12 months of SLA reports |
| The X-ray weakens our commercial position | In a fixed-price deal, leak on our side is our own margin | Internal first. The client partner decides what is shared. Our share of the leak becomes the price reduction we offer |
| The attack sheet leaks | It is a ready-made brief for a competitor | Approved internal AI workspace only. Marked restricted and never attached to a proposal |
| It becomes a blame document | A list of leaks reads as a list of people | Rows are keyed to causes and clauses. No individual is named |
| The model overstates | A fluent finding with no evidence wastes effort | Every row needs a source tag. Untagged rows are rejected |

One limit stays. The skill knows only what is recorded. Work that left no trace in any system will remain in the unexplained residual, and the delivery head has to account for it in person.

## The funding case

Renewal Defender protects revenue in two ways a sponsor can check. It raises the odds of keeping the renewal, and it lets us fund the customer's OpEx reduction from removed leak instead of a rate cut.

**Revenue protected = Renewal TCV × Change in retention probability**

An illustration with invented numbers: on a renewal worth $50 million over five years, a five-point lift in the odds of retention carries $2.5 million of expected value. I am not claiming the five points, and nobody could before a pilot. The cost is a markdown file and about two days of a pursuit lead's time per account.

Four measures will show within two quarters whether the skill works.

| Measure | What it shows | When it is visible |
| --- | --- | --- |
| Share of deployed hours explained after reconciliation | Whether the method closes the gap or only reports it | First run |
| Leaked hours on our side removed within two quarters | Whether delivery acted on the X-ray | Each quarter |
| Hit rate: objections raised in orals that were already on the attack sheet | Whether we can predict the challenger's case from our own data | At orals |
| Retention and margin on pilot renewals against our own baseline | The outcome | 12 to 24 months |

## The pilot and the ask

I am already running the skill on one of my own renewal accounts this week. Beyond that, I propose a six-week pilot on three renewals that expire in 9 to 18 months.

1. In weeks 1 and 2, I collect the data pack and the current productivity norms for each account and run the reconciliation.
2. In weeks 3 and 4, the client partner and delivery head review the leak map, correct the attributions, and agree a defence for every high-score attack.
3. In weeks 5 and 6, I compare the three accounts, extend the list of leak causes, and publish the skill file with a one-page run guide.

I need two things from a sponsor at VP level: access to the delivery data on three accounts, and the productivity norms in force today from whoever owns them. The skill file is written.

## Get the skill and improve it

The skill is two files: the instruction file and the data form. Anyone on a pursuit or account team can download both, use them as they are, and change them.

I will keep one master copy with a version number and a change log at the top of the file. If you improve your copy, send it back with three lines: what you changed, what kind of engagement prompted it, and what it found that the earlier version missed. I will merge accepted changes into the master copy once a month and credit the contributor in the change log.

The parts I most expect to grow are the list of leak causes, the data anomaly tests and the ticket extract columns for tools other than the one I started from.

Two rules apply to every copy. No client name, client data or account-specific norm goes into the skill file, because the file is shared and the data is not. And nobody removes the rule against inventing a counter-proof.

Download: the skill is in the [`renewal-defender`](../renewal-defender) folder of this repository, and the animated concept is in [`visual-concept`](../visual-concept).

## Try it in your AI chat window

If you have a renewal coming up in the next 18 months, try the skill on it this week. Open your AI chat window, load the Renewal Defender skill file, attach the data form with whatever you can fill in, and ask it to run the reconciliation for the last three months. Start with the minimum template. The first result will be marked Partial or Indicative, and it will still show you which data to ask delivery for next.

Then tell me two things: which leak cause on your account was not on my list, and which line of the attack sheet made your delivery head uncomfortable. Those two answers are how the next version gets better.

## Sources

- [The incumbent's dilemma: Enterprises changing service providers at an accelerating rate](https://www.ciodive.com/news/the-incumbents-dilemma-enterprises-changing-service-providers-at-an-accel/437495), Todd Lavieri (ISG), CIO Dive, 7 March 2017
- [Index Insider: The Incumbents' Disadvantage?](https://isg-one.com/articles/index-insider--the-incumbents--disadvantage), ISG, 15 May 2026
