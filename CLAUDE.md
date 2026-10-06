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

- Release 4.2 shipped 12 Aug 2026: ping wait cut from 90 to 60 seconds, proximity weighted more than recent acceptance history when ranking who gets pinged, console filters persist, three defect fixes (wiki Releases database, "4.2" page and its comments).
- Two symptoms since 4.2: callouts vanish before the responder can answer (15 tickets, 11 handlers) and some responders go quiet (30 open tickets from 4 handlers: Okafor, Pruitt, Fischer, Demir). That is about 2 to 1, which matches the support lead's read. Likely caused by the ping wait and the proximity change, but unconfirmed.
- Sources used: wiki Customer interviews (4 handlers, 2 to 5 Sep), rook-database support_tickets (147 tickets, 29 Jun to 7 Sep), wiki Glossary. All feedback comes from handlers, never responders directly, and the data stops 7 Sep.
- Mostly noise for this problem: Supply and gear (26 tickets), account and admin, saved filters, dark mode, text size, alert sounds.
- Still open: the engineering manager's 14 Aug question (should the proximity change apply to responders who turn jobs down?) was never answered; the "August seasonal dip" was never tested; Priya, who owned both 4.2 items, left 21 Aug; no ping or routing data checked yet.
