# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

What has changed recently within the code? What is different and how would that potentially impact a responder?

### 2.

Did an expired ping always count as a no?

### 3.

can you give me an example of how this would play out in a specific area with the responders so that I can better understand how these changes compound

### 4.

Explain to me how scoring works.
What is meant that acceptance goes up 0.08 and turning it down reduces it by 0.12?
Is it not an average or an overall acceptance "rate"

### 5.

why are some responders getting no pings based on this code

### 6.

if you were to tell me in a single, simple sentence why some responders get no pings, what would it say

### 7.

can you create an interactive visual so that I can better understand

### 8.

based on the data and this hypothesis: Some responders get no pings because the system asks people one at a time, best-ranked first, and stops at the first yes. A missed ping lowers someone's rank, and a low rank means they rarely get another chance to earn it back - can you draft an answer back to Marcus

### 9.

make it three sentences

### 10.

do we have response time data available?

### 11.

is there anything about proximity that I need to call out in this response: Hi Marcus, on your 14 Aug question: you were right that nothing in the code treats decliners differently, and four responders who weren't decliners (69–82% yes before 4.2) went from about 49 pings a week to 3 by 31 Aug. My working theory is that the shorter wait causes missed pings, a miss costs the same as a decline, and a low rank means too few pings to earn it back. Could you pull the current history scores for Farlight, The Undertow, Meteor Mite, Vesper and The Gale, plus ping response times since 12 Aug, so we can confirm before proposing anything??

### 12.

shorten

### 13.

what are current history scores

### 14.

and how is the codebase tracking scores for each responder

### 15.

add that question to the Marcus message

### 16.

Is there any way to reset the score?

### 17.

can you fill in this statement: According to my findings in the code, for someone who's gone quiet, they would need to ___.

### 18.

there must be a way  for them to get pinged?

### 19.

is there any data in the support tickets that actually highlight this finding?
