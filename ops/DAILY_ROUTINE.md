# Eswatini Daily Routine

Timezone: Africa/Johannesburg (SAST). Eswatini uses the same offset.

This is the operating rhythm while the programme is in **BOOTSTRAP**.
It does not assume Buzz, the dashboard, or a live agent runtime.
When those exist, keep the same hours and move the logs onto the dashboard.

## Who does what

| Person | Job in this rhythm |
| --- | --- |
| Tumelo | Approves outreach, spend, travel, public claims. Picks the day's top 3. |
| Keenan | Field lead. Relationships, qualification gates, meeting capture. |
| Rio | Plan owner. Daily brief, research packets, writing, source catalogue, close-out. |
| OpenMausBot specialists | Called by Rio for one named task, not for a standing meeting. |

An organisation is not qualified until Keenan has all five: named sponsor, measurable problem, decision/procurement route, plausible budget route, dated next step.

## Weekday clock

| Time | Block | Owner | Done when |
| --- | --- | --- | --- |
| 07:30–08:00 | Morning brief | Rio | One-page brief in `ops/logs/YYYY-MM-DD.md` |
| 08:00–08:15 | Scan and lock top 3 | Tumelo | Top 3 written on the log. Anything else waits. |
| 08:15–12:00 | Deep work 1 | Rio + assigned specialist | One P0 moved, with evidence in the log |
| 12:00–13:00 | Field / prospect window | Keenan (Rio supports) | Meeting notes or a written outreach draft. No send without Tumelo. |
| 13:00–16:00 | Deep work 2 | Rio | Second P0 or the day's research packet |
| 16:30–17:00 | Close | Rio | Log closed: shipped, blocked, tomorrow, approval queue |

If Keenan is in meetings, Rio still runs brief and close. Do not skip the log.

## Morning brief (07:30)

Rio writes, in this order:

1. Date, weekday, phase (`BOOTSTRAP` until BUILD_STATUS says otherwise).
2. Calendar for Tumelo today (Eswatini items only).
3. Open P0s from `ops/BUILD_STATUS.md`.
4. Pipeline: how many prospects have 0 / 1–4 / 5 qualification gates.
5. Blocks that need a human today.
6. Proposed top 3.

Do not mix StudEx Meat / Shopify into this brief.

## Deep work rules

Pick from the current P0 list. One owner per item.

Bootstrap P0s (until replaced in BUILD_STATUS):

1. Catalogue existing Eswatini strategy sources into this repo.
2. Turn the ten-prospect shortlist into a working tracker (gates, not slogans).
3. Confirm or kill the 28 Sep–3 Oct travel window.
4. Draft one-page 30-day pilot scopes for RSTP/NDC, Ubombo or RES, UNESWA/SEDCO.
5. Inventory Buzz agents for group `90bbcbba-9a1e-4b9a-9442-b4227ef16421` once API access exists.

No parallel new strategy docs until those five move.

## Field window (12:00)

Use the loop: Discover → Qualify → Engage.

After every conversation, capture before the next one:

- Organisation
- People named
- What they said the problem is
- What is still unknown
- Owner
- Dated next action
- Confidence: verified / hypothesis / proposal

Rio files it into the day's log and, once it exists, the prospect tracker.

## Close (16:30)

Rio must leave four lines:

```
Shipped:
Blocked:
Tomorrow:
Approvals needed from Tumelo:
```

Update `ops/BUILD_STATUS.md` if a work-queue row changed.

## Weekly overlay

| Day | Extra | Owner |
| --- | --- | --- |
| Monday | Set the week's three outcomes. Check BUILD_STATUS. | Rio + Tumelo |
| Wednesday | Pipeline review. Count qualification gates, not meetings held. | Rio + Keenan |
| Friday | Evidence pack. What can be shown without claiming a partner? | Rio |
| Sunday | Light scan only. No new outreach. Prep Monday brief. | Rio |

## What never happens in the daily loop

- External messages, spend, travel booking, or public claims without Tumelo.
- Treating the ten-prospect list as confirmed interest.
- Mixing drone racing/education with agricultural spraying.
- Putting credentials in the log, Notion, Git, or chat.
- Autonomous mining control, lending decisions, or government-endorsement language.

## Start tomorrow (Thu 10 Sep 2026)

Copy `ops/daily-log-template.md` to `ops/logs/2026-09-10.md` and run the 07:30 brief.

Until Buzz is connected, the log file is the system of record.
