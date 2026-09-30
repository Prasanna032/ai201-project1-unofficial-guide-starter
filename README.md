# The Unofficial Guide

Prasanna Konyala

Corpus I picked: campus_life

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

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->

## Chunking Strategy

**Chunk size:** no fixed character count, one chunk per paragraph, with the
document's title line copied onto the top of each. On `campus_life` that gives
183 chunks, 63–397 characters, 167 on average.

**Overlap:** 0

The `campus_life` posts are short, 178 to 549 characters,  and every one is a
title line followed by one to four paragraphs. The starter's 800-character
windows never split anything (88 documents became 88 chunks), so the real
question was whether one post should stay one chunk.

I considered keeping it that way, since each post is mostly about one subject.
What decided it was reading the posts that hold two separate thoughts:
`dining_halden_hall.txt` has wait times and what's worth eating in one
paragraph, and hours and price in the other. As a single chunk, a question
about Halden's closing time has to match text that's half about soup. Split on
the paragraph break, each piece is one thought.

Splitting on its own caused a second problem, which is why every chunk carries
its title. Many posts are near-duplicates: the seven laundry posts are word for
word the same apart from the building name, prices and payment method, and the building name
only appears in the title. A paragraph like "Machines take $2.00 wash, $1.75
dry" with no title can't be tied to any building, and retrieval can't tell it
from the other six. With the title copied on, every chunk says what it's about.

No overlap, because overlap exists to rescue a sentence cut in half, and
splitting on paragraph breaks never cuts a sentence. The repeated title already
carries the context overlap would have.

<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: `admin_add_drop_deadline.txt#0` — produced by: `chunker.py::split_documents`

```
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

**Chunk 2** — source: `course_cs_340_exams.txt#1` — produced by: `chunker.py::split_documents`

```
CS 340 Databases — assessment

Start the term project in week three, not week eight; everyone learns this the hard way.
```

**Chunk 3** — source: `course_phys_130_workload.txt#0` — produced by: `chunker.py::split_documents`

```
Workload for PHYS 130 Mechanics

People keep asking so: 7 hours a week, plus 3 on lab weeks. That's real time, not optimistic time.
```

**Chunk 4** — source: `dining_verrill_street_grill_followup.txt#1` — produced by: `chunker.py::split_documents`

```
Re: Verrill Street Grill

Also worth saying: one register, so the queue is a single line no matter how busy. Nobody tells you this at orientation.
```

**Chunk 5** — source: `housing_morrow_house.txt#1` — produced by: `chunker.py::split_documents`

```
Morrow House — what it's actually like

The good: cheapest housing tier by about $900 a year, and the singles are real singles.
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:** What do I need to withdraw from a course after week six?

**Answer:**

```
$ python app.py ask "What do I need to withdraw from a course after week six?"
  (best distance 0.514, cutoff 0.7)

To withdraw from a course after week six, you need an adviser's signature (admin_withdrawal_deadline.txt).

Sources retrieved: admin_add_drop_deadline.txt, admin_graduation_requirements.txt, admin_pass_fail_option.txt, admin_withdrawal_deadline.txt, advising_registration.txt
```

**My relevance cutoff:** 0.70

My five in-corpus questions scored between 0.173 and 0.593; the five
`OUT_OF_SCOPE` questions scored between 0.787 and 0.923. That's a clean gap from
0.593 to 0.787, and 0.70 sits roughly in the middle of it, about 0.1 from each
side.

I moved it off the starter's 0.6 because the shuttle question scored 0.593 —
it only got through by 0.007, and a slightly different wording would have been
refused even though the answer is in the corpus. Going higher than 0.70 would
put the cutoff within 0.09 of the Mongolia question (0.787).

Two things I didn't expect. First, I guessed the ibuprofen question would land
close to `health_center.txt`; it scored 0.849 and the health centre post
wasn't even in its top five. Second, a campus-sounding question the corpus
doesn't cover — "What time does the campus gym open?" — scored 0.421, lower
than two of my real questions, so the gate let it through. The grounding
instruction caught it instead: the model answered "I don't have enough
information to answer your question, as the provided documents do not mention
the campus gym." The gate only stops questions from a different world; near
misses depend on the prompt.

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

| Question | In corpus? | Best distance |
|---|---|---|
| How long is the wait at Kestrel Commons between 12:15 and 1:00? | Yes | 0.173 |
| How are juniors and seniors ordered in the housing lottery? | Yes | 0.225 |
| What grade do I need to get a pass in a course taken pass/fail? | Yes | 0.327 |
| What do I need to withdraw from a course after week six? | Yes | 0.514 |
| Which shuttle stop gets skipped when the driver is running behind? | Yes | 0.593 |
| What is the capital of Mongolia? | No | 0.787 |
| Who won the 1994 World Cup? | No | 0.847 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.849 |
| How do I write a for loop in Rust? | No | 0.860 |
| How do I change the oil in a diesel engine? | No | 0.923 |

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

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
