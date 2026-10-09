# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I'm the new PM for **Rook Dispatch** (started after Priya Raghunathan left, 21 Aug 2026; no overlap). Sources: `00-rook/company/notes/handoff-from-priya.docx` and the Company section of the rook-wiki connector (read 6 Oct 2026). Priya's handoff is her opinion, not data — check her claims before repeating them.

### Company
Rook Industries (founded 2014, 241 staff, HQ Site Aleph) sells coordination and provisioning software to independent masked responders and their handlers/quartermasters. Subscription, priced per active responder. Responders never employed by Rook. **Confidentiality:** cover identities are never stored or mapped to legal identities (Security Policy 4.1). Never design anything that assumes such a mapping; never try to work out who anyone is.

### Products
- **Rook Dispatch (mine, release 4.2):** incident enters console → rank available responders → ping top-ranked responder's phone → taken, turned down, or missed → next in order. Handlers use the web console; responders use the phone app. Routing config ships in the release, not as a runtime setting.
- **Rook Supply (sibling, 4.2):** requisitions → quartermaster approval → fulfillment → maintenance schedule from service intervals; field failure reports feed item history. **Dependency:** Supply reads the Responder Availability Record that Dispatch writes, and schedules maintenance into low-callout-load windows. Any change in how Dispatch calculates availability or load flows into Supply with no change on their side.
- Monthly release train, 4.x numbering. Actual ships: 4.0 on 7 Apr, 4.1 on 16 Jun, 4.2 on 12 Aug (gaps are ~2 months, not monthly). No 4.3 in the notes yet.

### People
| Who | Role | Note |
|---|---|---|
| Helen Achebe | Director of Product (my boss) | Owns roadmap and commitments; gives room |
| Marcus Oyelaran | Eng Manager, Dispatch | Candid; start here when unsure; can pull numbers |
| Wen Li | Staff Engineer, Berlin | Built the ranking logic; was away 14–24 Aug, right after 4.2 shipped |
| Nadia Hoffmann | Support Lead, Berlin | Hears handler complaints first; standing 15 min |
| Sofia Marino | Product Designer | Console and phone app; ran the September interviews (not yet seen) |
| Ravi Menon | Data Analyst, Singapore | Weekly acceptance-rate reporting |

### Vocabulary
- **Callout:** request for a responder to attend an incident. **Ping:** a callout offered to one responder.
- **Taken / turned down / missed:** missed = ping wait expired. Turned down and missed are recorded separately but both pass the callout on.
- **Ping wait:** time before a ping counts as missed (same for everyone, set in the release).
- **Acceptance rate (headline metric):** taken ÷ pings. Missed pings count against it. **Time-to-accept:** median seconds ping → taken. **Coverage gap:** no available responder had the required tags (nobody *could*, not nobody *would*); don't mix it with low acceptance.
- **Routing priority:** score ranking responders. Inputs: proximity (travel-time estimate), availability, capability match, recent acceptance history. Turning down or missing a ping lowers later rank.
- **Capability tags:** flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management, de-escalation.
- **Mutual aid:** cross-area cover; not supported, on the Q4 exploration list.
- Supply: requisition, field failure report, service interval, quartermaster.

### Where things stand
- **4.2 (12 Aug) changed three things at once:** proximity weighted up vs recent acceptance; ping wait cut 90 → 60 s; console filters now persist. Since then fewer pings are taken and more handlers complain.
- **Priya's read:** mostly seasonal (August is always soft), recovers in September; don't let it become a revert debate. Proximity change was a long-requested ask. **Untested.** Seasonality, the shorter ping wait and the ranking change are confounded. Hypothesis to check: a shorter wait creates more "missed" pings, which lower recent-acceptance, which lowers rank, which compounds. Ask Ravi for weekly acceptance rate by year, and split taken/turned down/missed before and after 12 Aug.
- **Q3 roadmap** (last reviewed 30 Jun, all owned by Priya, so stale): ranking change, Availability Confidence, ping timeout tuning (all 4.2); requisition approval chains (4.3, Supply); handler phone app and shared cover between responders (Q4, exploring). **Availability Confidence is committed for 4.2 but absent from the 4.2 release notes.** Priya says a couple of items were squeezed out of 4.2. Settle with Helen which are still Q3 commitments (not yet discussed).
- **Gaps:** no written description of how pings are decided; Priya asked me to write it, and Wen Li is the person to ask. Console filter-persistence tickets are cosmetic noise, so deprioritise them.
- **Not yet read:** the rook-database connector, the September interview findings, support ticket data.
- **Update 6 Oct (supersedes the line above and the "cosmetic noise" and "recorded separately" notes):** all three are now read. Data is in rook-database (callouts, pings, responders, handlers, support_tickets; 29 Jun–6 Sep, 16 responders). Interviews are in the wiki under Research → Customer interviews (four handler calls, 2–5 Sep, all console-redesign research).
- **Metrics:** acceptance ran about 77% until 4.2, hit 54% the week of 10 Aug and was 72.7% by 31 Aug. Fill rate went from 94–97% to about 86–87%, then back to 94.5%. The drop is missed pings (about 2% to 21%, a step on 12 Aug), not turned-down ones, so it isn't a seasonal drift. The 60 s timeout and the ranking change can't be separated without ping response times. There is no prior-year data.
- **Starved responders:** Farlight, The Undertow, Meteor Mite and Vesper fell from 70–86 pings to 11–17 after 12 Aug and miss over half of what they get. Meteor Mite and The Gale share Eastgate but got 14 vs 69 pings. Likely cause, not confirmed: `history.py` charges 0.12 for a decline or miss and credits 0.08 for a yes (break-even 60%), with no decay (2019 TODO). Ask Wen Li for their actual scores.
- **Tickets:** 107 since 12 Aug against about 7 a week before. 15 are "gone before he could answer" and 30 are "starved of pings"; none of those 45 is closed. About 14 are filter tickets, and four report silent resets, so they aren't pure noise. Code facts: capability match is a score, not a filter (an unqualified nearby responder can outrank a qualified one), and a missed ping is scored like a decline.
- **Still open:** Ravi's refresh from 7 Sep, ping response times, any responder-side research, the cost of unfilled callouts, Security Policy 4.1 (unread), and which deferred 4.2 items are still Q3 commitments. Helen has asked for a one-pager on the quiet-responder problem (`05-super-speed/director-request.txt`, a later module).

### Module 2 notes (customer interview analysis)
- Release 4.2 shipped 12 Aug 2026: ping wait cut from 90 to 60 seconds, proximity weighted more than recent acceptance history when ranking who gets pinged, console filters persist, three defect fixes (wiki Releases database, "4.2" page and its comments).
- Two symptoms since 4.2: callouts vanish before the responder can answer (15 tickets, 11 handlers) and some responders go quiet (30 open tickets from 4 handlers: Okafor, Pruitt, Fischer, Demir). That is about 2 to 1, which matches the support lead's read. Likely caused by the ping wait and the proximity change, but unconfirmed.
- Sources used: wiki Customer interviews (4 handlers, 2 to 5 Sep), rook-database support_tickets (147 tickets, 29 Jun to 7 Sep), wiki Glossary. All feedback comes from handlers, never responders directly, and the data stops 7 Sep.
- Mostly noise for this problem: Supply and gear (26 tickets), account and admin, saved filters, dark mode, text size, alert sounds.
- Still open: the engineering manager's 14 Aug question (should the proximity change apply to responders who turn jobs down?) was never answered; the "August seasonal dip" was never tested; Priya, who owned both 4.2 items, left 21 Aug; no ping or routing data checked yet.

- Missed pings step up on 12 Aug itself (about 5% a day before, 28% on 12 Aug, 48% on 13 Aug). Turned-down pings stay flat at about 21%. The 10 Aug week figure mixes two normal days with five bad ones. Callout volume also dipped about 20% from 12 Aug (cause not checked). Still no prior-year data, so "seasonal" is unproven, but a step on release day does not look seasonal.
- Four responders collapsed: Farlight (Pruitt), Vesper (Aunt Dot), The Undertow (Okafor), Meteor Mite (Kip). They went from about 12 pings a week to under 1 and miss 53 to 64% of pings. The other 12 miss about 14% (up from 2%). Aunt Dot, Kip and Halloran filed no tickets, and the four interviewed handlers are the low-ticket ones. Always break metrics out by handler.
- Farlight: missed 4 of her first 7 pings after 12 Aug, then 11 pings in three weeks and none after 28 Aug. Uptown callouts stayed busy and were taken by Falkirk, Cindermark and Bulwark. Likely cause: shorter wait plus the miss penalty (0.12 against 0.08 credit), but unconfirmed. Scores I modelled are not Wen Li's.
- Headline metric chosen: missed-ping rate (2% to 21.5%, 12.7% by 31 Aug), with a supporting line that four responders get under 3 pings a week. Unanswered callouts recovered to 5.5% and hides the problem.
- Still open: Wen Li's real scores and travel times (why was Farlight outranked in Uptown despite the 0.60 proximity weight?), last year's August from Ravi and his definition of "missed", whether Linda Pruitt's 11 open tickets got any reply (ask Nadia), the 17 to 18 Aug dip in misses, and the callout volume drop.

### Module 4 notes (reading the dispatch-routing code)
- **How scoring works:** `history.py` keeps one running tally per responder (start 0.5, +0.08 for a yes, -0.12 for a decline or a miss, capped 0 to 1). It is a tally, not a rate, so about 60% yes is break-even. Nothing else adds points, nothing decays (2019 TODO), and there is no reset. Scores live only in memory, keyed by name, so a restart sets everyone to 0.5.
- **Why some responders go quiet:** pings go one at a time, best-ranked first, stopping at the first yes. A miss lowers rank, and a low rank means few pings to earn points back. Correction to an earlier idea: the 4.2 weights make history count less (0.40 to 0.25), so the shorter wait creates the misses and the heavier proximity weight (0.45 to 0.60) widens the starting gap. They can't be separated yet.
- **Data check (callout-history.csv, weekly pings):** the four quiet responders went from about 49 pings a week to 3 by 31 Aug, while The Gale rose from 13 to 21. Before 4.2 their yes-rates were 69 to 82%, so they weren't habitual decliners. My replay of the ping log puts all four at about 0.00 now. That is an estimate, not the real scores.
- **Response times:** the pings table has no response-time column. Gaps between pings show missed pings waited about 92 s before 4.2 and about 62 s after, and turned-down pings were answered in 40 s or less. How late the missers would have answered is unknown. Tickets 3130 and 3140 ("first in 5 or 9 days, and gone before he could answer") fit the loop.
- **Marcus's 14 Aug question:** the code does not treat decliners differently, so it applies to everyone. My reply to Marcus is drafted, asking for current scores, travel times, response times and whether scores are held only in memory or restarted since 12 Aug. Still open: whether it was a deliberate decision (Wen Li or Priya), and whether any manual reset exists.
- **Availability:** `availability.py` is placeholders only, so how "available" and travel time are calculated isn't visible in this folder. Availability is set by responders and handlers, and handlers say theirs was correct for the quiet responders, so it looks ruled out as the cause. Travel-time estimates for the four are still unchecked.
- **Helen's request (`05-super-speed/director-request.txt`):** she wants what we'd build instead of changing a number, from the point of view of a handler like Kip and a quiet responder, and ideally something clickable. She says engineering could ship "the actual fix" this afternoon but never says what it is. My guess is the miss-cost change; ask Marcus.
- **The brief (`05-super-speed/brief.md`) is a one-pager:** four points (handler can see it, a miss isn't a "no", a way back, a late tap counts), a readiness table, success measures and two unknowns. Wen is unavailable, so I assumed a miss costs 0.06 (half a decline), scores below 0.5 drift up 0.02 a day and stop at 0.5, and after 7 quiet days a responder goes second in line in their own area with the full 90 s. All three are for Wen to confirm. The 60 s wait stays until response times arrive.
- **The prototype (`05-super-speed/prototype.html`)** is a superhero-themed handler console for Kip, with Meteor Mite and The Gale side by side and a catch-up ping flow on a phone mock. It doesn't show the fading score or the late tap. Kip hasn't seen it.
- **Farlight (Uptown, handler Linda Pruitt):** 11 pings after 12 Aug (2 taken, 2 turned down, 7 missed), and 0 of the 5 pings from outside her area. Her last two pings (23 and 28 Aug) were both missed. The four quiet responders and their handlers are Farlight (Pruitt), The Undertow (Okafor), Vesper (Aunt Dot) and Meteor Mite (Kip).
- **Still open:** Wen's answers on miss cost and forgiveness, the real scores, ping response times and push delivery logs, whether callouts have an urgency field, Ravi's data refresh since 7 Sep, what Helen's "small fix" is, and Kip's reaction to the prototype.

- **Module 6 session (9 Oct):** I turned my brief-review habit into a skill, `.claude/skills/review-checklist/SKILL.md`: six checks (owner, how we'll know it worked, scope start vs end, problem before fix, metrics with baselines, open questions), Pass/Partial/Fail with quoted evidence, verdict Ready / Fix first / Not ready. It judges only what the brief says and never edits it.
- **My brief after review:** first run was Not ready (no named owner, pilot covered only 2 of the 4 responders). Now fixed: owner is "Dispatch PM", the pilot covers all four quiet responders with The Gale as the load comparison, tickets baseline is 30 "starved of pings" + 15 "gone before he could answer" (none closed, as of 7 Sep), and the missed-ping rate (12.7%) is stated as all responders. The "under 3 new tickets a week" target and "answers needed before the pilot starts" are my proposals, not data. Open questions now each have a suggested owner (Marcus, Wen, Ravi, Sofia) but no dates.
- **Kristen Mayer's brief** (public classmate repo, `briefs/dispatch-quiet-responder-brief.md`) came out Fix first: five of six pass, but missed-ping rate and time-to-accept have no baseline, and a phase 2 measure sits under "every phase". I drafted a note to her; it was not sent.
- **Habits and housekeeping:** a Monday 8:40 review schedule (`monday-brief-review-checklist`) was created and then disabled. This session runs in a git worktree, so run the setup check from the repo top folder, and the local `main` checkout is behind GitHub `main`.
- **Still open:** current median time-to-accept (ask Ravi), whether Marcus agrees the under-3 ticket target, dates for the open questions, whether to send Kristen the note.
