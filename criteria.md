# Acceptance criteria — The Unofficial Guide

Five criteria that say what "working" means for this system, written in unit 1
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. *"Retrieval works"* is an opinion. *"For at
least 4 of my 5 test questions, the top results include a chunk containing the
answer"* is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter or looser one. A reason that says something about your corpus or your
pipeline earns credit; *"80% seemed reasonable"* does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## 1. Retrieved chunks contain the answer

For at least 4 of my 5 test questions, the retrieved chunks include one that
contains the answer.

**Why this target:**
Each answer sits in one short post on a single topic, so I expect retrieval to find most of them. I'm allowing one miss because the Kestrel Commons question has to compete with six other dining posts that use the exact same 'Wait times: …' wording, and only the hall's name tells them apart.

---

## 2. Every answer names a source

Every answer the system produces names at least one source document.

**Why this target:**
The grounding instruction tells the model to name the file, and every file name says what it's about. The only way this fails is if the model ignores the instruction, and that's exactly what I want to catch.

---

## 3. The relevance gate stops out-of-corpus questions

When I ask a question my documents clearly don't cover, the relevance gate
stops it and the system returns "I don't have enough information about that" —
in at least 4 of 5 tries.

<!-- The five questions are the ones in `OUT_OF_SCOPE` at the bottom of
     `questions.py`, and `run_eval.py` puts them through the gate and writes
     what happened into your run log. Swap them for your own if you'd rather —
     just keep five of them, or the "4 of 5" above has nothing to be 4 of. -->

**Why this target:**
We have a 0.6 cutoff, and the out-of-scope questions come from completely different subjects, so I expect a clear gap. I'm allowing one miss for the ibuprofen question, since health_center.txt is the closest thing in my corpus to a medical question.

---

## 4. Something about your chunks

Every one of the 5 chunks printed by python app.py chunks -n 5 names the building, dining hall, course, or office it describes.



**Why this target:**
21 of my documents are near-duplicates: the laundry, noise and dining posts differ only in the name and the numbers. If my chunker splits off the title line, a chunk like '$2.00 wash, $1.75 dry' can't be attributed to any building, and retrieval can't tell it apart from the other six.


---

## 5. Your choice

At least 4 of my 5 answers contain their expects phrase from questions.py, in each of the three runs.



**Why this target:**
Criteria 1 and 2 only check that the right material came back and that a source was named. Neither checks that the answer is actually correct. I wrote the expects phrases before seeing any results, so this checks correctness against a standard I couldn't adjust afterwards.


---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 2 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 1. Retrieved chunks contain the answer

         For at least 4 of my 5 test questions, the retrieved chunks include
         one that contains the answer.

         **Why this target:** ...

         > **Revised in unit 2:** For at least 4 of 5 questions, the top three
         > results contain the answer.
         >
         > **Why revised:** I couldn't judge "the chunks include one that
         > contains the answer" the same way twice — I scored two questions
         > differently on Monday than on Wednesday. The new version is
         > something I can actually check.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "I said 4 of 5 but got 2 of 5, so 2 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.

     The whole reason the originals stay visible is so someone can see what you
     said before you knew the answer.
     ───────────────────────────────────────────────────────────────────────── -->
