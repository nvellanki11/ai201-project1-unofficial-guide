# The Unofficial Guide

<!-- Replace this line with your name and which corpus you picked. -->

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

This is a RAG system that ingested, chunked, embedded and stored information from a corpora of documents about personal advice, campus life or city guides for many different places. It has been tested on a series of prompts and (mostly) grounded to prevent fabrication of data. To use the model, run "python app.py ask "Question in here"" and it will output a formatted response to answer your request, or say that it does not have enough information. 

## Chunking Strategy

**Chunk size:**
**Overlap:**

     At the moment the strategy is taking chunk size up to 800 chars. Documents smaller then this in length will be kept together while larger documents get chunked. I chose this approach because when I scrolled through the different corpus documents, I noticed longer docs with paragraphs of nearly this length, but also much more confined docs, which could pass as one context chunk. These longer docs with multiple chunks have an overlap of 120 chars (15% of max size).

## Sample Chunks


     ======================================================================
Chunk 1  |  source: admin_add_drop_deadline.txt#0  |  produced by: chunker.py::split_documents
======================================================================
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.

======================================================================
Chunk 2  |  source: course_biol_160.txt#0  |  produced by: chunker.py::split_documents
======================================================================
BIOL 160 Cell Biology

I lived here my sophomore year. Format is lecture three times a week with a weekly lab. Assessment: four unit tests and a cumulative final. Not curved.

Expect 9 to 11 hours a week, the heaviest first-year course by reputation.

The one piece of advice: the unit tests come fast, roughly every three weeks; falling behind once is very hard to recover from.

======================================================================
Chunk 3  |  source: course_hist_118_workload.txt#0  |  produced by: chunker.py::split_documents
======================================================================
Workload for HIST 118 Modern World History

People keep asking so: a lot of reading, about 120 pages a week, but no problem sets. That's real time, not optimistic time.

It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.

======================================================================
Chunk 4  |  source: dining_pellew_dining_hall_followup.txt#0  |  produced by: chunker.py::split_documents
======================================================================
Re: Pellew Dining Hall

Adding to what people have said about Pellew Dining Hall. The wait figure of 12 to 18 minutes at peak matches what I've seen. If you're trying to eat between classes, go before 11:45 and it's a different building entirely.

Also worth saying: the furthest hall from anywhere, next to the athletics centre. Nobody tells you this at orientation.

======================================================================
Chunk 5  |  source: housing_innisfree_hall.txt#0  |  produced by: chunker.py::split_documents
======================================================================
Innisfree Hall — what it's actually like

Transferred in last year, so take this with a grain of salt. Built 1991, renovated 2022. Rooms are doubles arranged as pairs sharing one bathroom between two rooms.

The good: the shared-bathroom-between-two-rooms arrangement is the best compromise on campus.

The bad: no air conditioning, which matters for the first three weeks of September.

Laundry costs $1.75 wash, $1.75 dry, app-based. On noise: moderate; the building is L-shaped and the short wing is much quieter.


## Sample Answer

**Question:**

How many hours a week does CS 340 Databases take once the project lands? — run 3

**Answer:**

CS 340 Databases takes 15 hours a week in the last three weeks when the project lands. This information comes from the documents `course_cs_340_workload.txt` and `course_cs_340.txt`.

**My relevance cutoff:**

What are the wait times at Kestrel Commons during lunch?
  run 1: —  (best distance 0.173)
  run 2: —  (best distance 0.173)
  run 3: —  (best distance 0.173)

How many washers and dryers are in Calder Annexe, and when should I avoid doing laundry there?
  run 1: —  (best distance 0.204)
  run 2: —  (best distance 0.204)
  run 3: —  (best distance 0.204)

How many hours a week does CS 340 Databases take once the project lands?
  run 1: —  (best distance 0.163)
  run 2: —  (best distance 0.163)
  run 3: —  (best distance 0.163)

How often does the campus shuttle run on weekdays?
  run 1: —  (best distance 0.425)
  run 2: —  (best distance 0.425)
  run 3: —  (best distance 0.425)

What advice do students give about commuting an hour each way?
  run 1: —  (best distance 0.595)
  run 2: —  (best distance 0.595)
  run 3: —  (best distance 0.595)

Out-of-scope questions (the gate should refuse these):
  refused  (best distance 0.825)  What is the capital of Mongolia?
  refused  (best distance 0.934)  How do I change the oil in a diesel engine?
  refused  (best distance 0.886)  Who won the 1994 World Cup?
  refused  (best distance 0.844)  What is the recommended dosage of ibuprofen for a headache?
  refused  (best distance 0.896)  How do I write a for loop in Rust?


  The gap is at about 0.6, which would prevent fabrication for well out-of-scope questions, but the model would attempt some more challenging, slightly ambiguous questions. I prefer it this way, but may eventually reduce to 0.5 as the final question was not answered by the model.

## How I Used AI

I brainstormed a chunking strategy based on my own intuition and the context of the documents, and refined it with Claude (it suggested the overlay amount). 
I also used it regularly for code generation, and would choose to either accept, deny or modify the suggested changes based on the specs of the assignment.

**1.**

**2.**

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 |  |  |  |
| 5 |  |  |  |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
