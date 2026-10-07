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
