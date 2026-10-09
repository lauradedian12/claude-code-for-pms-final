# Quiet responders: give them a way back

For Helen · Owner: Laura, PM, Rook Dispatch · Draft

**The problem.** Since 4.2, four responders (Farlight, The Undertow, Meteor Mite, Vesper) dropped from about 49 pings a week combined to 3. They weren't turning jobs down: before 4.2 they said yes 69–82% of the time. Today a missed ping costs the same as a no, scores never fade, and nothing gives a low-ranked responder a way back.

**What it's like now.** The handlers keep asking us: "I would rather know than guess" (ticket 3089). One responder, via their handler: "starting to wonder if im still even in the system" (tickets 3120 and 3121).

## What we'd build instead of changing a number

1. **The handler can see it.** The console flags "No pings in 6 days" with the plain reason: "Ranked low after missed pings. Availability and skills are fine." Kip, the handler for Meteor Mite and The Gale, stops guessing.
2. **A missed ping isn't a "no".** Someone who didn't answer in time isn't treated as someone who turned the job down.
3. **A way back.** After a quiet spell, the responder gets a catch-up ping with the full wait, and their standing drifts back toward neutral over time. This is Wen's 2019 question, answered.
4. **A late tap still counts.** If the ping has moved on but nobody has taken the callout, a late tap takes it. This is aimed at the 15 "gone before he could answer" tickets.

**What a quiet responder would notice:** a ping arrives again, and the app tells them it's theirs if they want it.

**Why not just revert the 60 s wait, or lower what a miss costs?** Either may be the small fix engineering has in mind, and either may be worth doing first. But on their own, neither gives a low-ranked responder a way back or tells the handler why the phone went quiet. This is my reasoning, not tested. To check with Marcus.

## Where each piece stands

Wen is unavailable, so items 2 and 3 rest on assumptions for her to confirm.

| Piece | Status | Assumption or open question |
|---|---|---|
| 1. Handler can see it | Needs scores saved | Scores live only in memory today. Needs a definition of "quiet" and the wording handlers see |
| 2. Miss isn't a "no" | Assumed | A miss costs 0.06, half a decline. First miss not forgiven |
| 3a. Standing fades | Assumed | Scores below 0.5 drift up 0.02 a day and stop at 0.5 |
| 3b. Catch-up ping | Assumed, plus open | After 7 quiet days, second in line in their usual area, full 90 s wait. Open: callouts have no urgency field, and we don't know if missed pings reached the phone |
| 4. Late tap counts | Needs a feasibility check | Can the system honour a tap after the ping has moved on? |

## How we'd know it worked

Proposed aims, to agree with Marcus. We'd start with a four-week pilot on all four quiet responders (Farlight, The Undertow, Meteor Mite, Vesper). The Gale is not in the pilot: it shares Eastgate with Meteor Mite and is our load comparison.

Why these targets (proposed): about 8 pings each is two-thirds of the roughly 12 each they got before 4.2, a realistic first month rather than full recovery. Under 5% missed sits near the 2–5% before 4.2.

Baselines are from the ping data, which stops 6 Sep, and the tickets as of 7 Sep.

- **Pings to the four:** from 3 a week combined (week of 31 Aug) to about 32 combined, about 8 each. Before 4.2 they got about 49 combined, about 12 each.
- **Missed-ping rate (all responders):** from 12.7% (by 31 Aug) to under 5%.
- **Guardrails:** fill rate stays at 94% or above (proposed stop line: pause the pilot if it drops below 94%, to agree with Marcus), and The Gale's load moves back toward 13 pings a week from 21 (by 31 Aug).
- **Handler tickets** (this also tests item 1, handler can see it): from 30 open "starved of pings" and 15 "gone before he could answer" tickets, none closed, to new ones under 3 a week (proposed, to agree with Marcus).

## Still unknown

Owners are suggestions to confirm. Each answer is needed before the pilot starts; exact dates to be set once owners agree.

- Whether the shorter wait or the heavier proximity weighting did more damage. The 60 s wait stays until we have response times. Owner: Marcus (response times), Wen (scores).
- Our ping data stops on 6 Sep. A refresh from Ravi is requested. Owner: Ravi.
- Do callouts have an urgency field? Needed for the catch-up ping (3b). Owner: Wen.
- Did missed pings actually reach the phone? Needs push delivery logs. Owner: Marcus.
- Can the system honour a late tap after the ping has moved on (item 4)? Owner: Marcus.
- Can scores be saved so the console can show them (item 1)? Owner: Marcus.
- What counts as "quiet", and the exact wording handlers see (item 1)? Owner: Dispatch PM with Wen for the definition, Sofia for the wording.

**If we do nothing:** the four stay at about 3 pings a week combined, and the 45 open tickets stay unanswered. Unanswered callouts have recovered to 5.5%, which hides the problem. The cost of unfilled callouts is unknown (no revenue figure).

**Watch-out:** Supply schedules servicing into low-callout windows, and giving pings back to these four changes their load.

**Decision needed:** agree the direction and the assumptions before anyone touches the code.
