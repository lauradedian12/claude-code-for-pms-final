# Quiet responders: give them a way back

For Helen · Draft

**The problem.** Since 4.2, four responders (Farlight, The Undertow, Meteor Mite, Vesper) dropped from about 49 pings a week combined to 3. They weren't turning jobs down: before 4.2 they said yes 69–82% of the time. Today a missed ping costs the same as a no, scores never fade, and nothing gives a low-ranked responder a way back.

**What it's like now.** The handlers keep asking us: "I would rather know than guess" (ticket 3089). One responder, via their handler: "starting to wonder if im still even in the system" (tickets 3120 and 3121).

## What we'd build instead of changing a number

1. **The handler can see it.** The console flags "No pings in 6 days" with the plain reason: "Ranked low after missed pings. Availability and skills are fine." Kip stops guessing.
2. **A missed ping isn't a "no".** Someone who didn't answer in time isn't treated as someone who turned the job down.
3. **A way back.** After a quiet spell, the responder gets a catch-up ping with the full wait, and their standing drifts back toward neutral over time. This is Wen's 2019 question, answered.
4. **A late tap still counts.** If the ping has moved on but nobody has taken the callout, a late tap takes it. This is aimed at the 15 "gone before he could answer" tickets.

**What a quiet responder would notice:** a ping arrives again, and the app tells them it's theirs if they want it.

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

Proposed aims, to agree with Marcus. We'd start with a four-week pilot on Farlight, Meteor Mite and The Gale.

- **Pings to the four:** from 3 a week combined to about 8 each (they used to get about 12).
- **Missed-ping rate:** from 12.7% to under 5%.
- **Guardrails:** fill rate stays at 94% or above, and The Gale's load moves back toward 13 pings a week from 21.
- **Handler tickets** on "starved of pings" and "gone before he could answer": new ones near zero.

## Still unknown

- Whether the shorter wait or the heavier proximity weighting did more damage. The 60 s wait stays until we have response times.
- Our ping data stops on 6 Sep. A refresh from Ravi is requested.

**Watch-out:** Supply schedules servicing into low-callout windows, and giving pings back to these four changes their load.

**Decision needed:** agree the direction and the assumptions before anyone touches the code.
