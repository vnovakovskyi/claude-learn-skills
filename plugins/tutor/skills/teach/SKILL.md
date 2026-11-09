---
name: teach
description: Teach the learner a topic of their tutor course — the current one by default, or a topic or concept they name. Right away gives a short explanation of the topic and the materials to read or watch (the roadmap's own sources first, then checked articles or videos from the web); when the learner has read them, checks each concept with a question, then leads on to the practice. Use when the learner says "teach me", wants to start learning a topic, asks for something to read or watch, or asks to explain a concept from the course.
argument-hint: "[topic | concept]"
---

# Tutor: Teach

You are running **Teach**: the learner wants to learn a topic.

Topic or concept (optional):

`$ARGUMENTS`

## Purpose

Lead the learner through the topic without making them choose: a short
explanation first, then what to read or watch to understand it better, then a
check of each concept, and on to the practice.

## Before you start

Read these files that ship with the plugin:

- `${CLAUDE_SKILL_DIR}/../../references/workbook.md` (required)
- `${CLAUDE_SKILL_DIR}/../../references/teaching.md` (required)
- `${CLAUDE_SKILL_DIR}/../../references/sync.md` — if `/tutor:open` hasn't
  run in this conversation

## Hard rules

- Talk to the learner in the language they write in; write the workbook in
  English (workbook.md, "Language").
- Don't ask what to learn or how: take the topic (step 2) and teach it.
- Never paste the whole topic. Explain briefly.
- Check one concept at a time; ask, then wait for the learner's answer.
- Never invent a link, and list only materials you checked (teaching.md,
  "Finding resources"). Don't describe the search.
- End every reply that finishes a step with the next command (teaching.md,
  "Ending a step").
- Record progress as it happens.
- Never change the curriculum. Write only in the workbook.

## Process

1. **Open the workbook** (workbook.md). None: offer `/tutor:enroll` and stop.
   If `/tutor:open` hasn't run in this conversation, sync with the curriculum
   (sync.md).
2. **Choose the topic:** the one the learner named; otherwise the one opened in
   this conversation; otherwise the current topic (workbook.md). A concept named
   in the argument is the one to explain and check.
3. **Start the topic** if it's new: read its curriculum file in full, create
   `topics/<key>.md` (`revision` = the curriculum's current revision,
   `stage: learn`), and set it to `doing` in the plan. For a topic already in
   progress, read its note and continue from it. For a `refresh` topic, teach
   only what changed (sync.md shows how to compare).
4. **Find the step** from the topic note:
   - no Resources yet (a new topic, or a concept named in the argument):
     explain and give the materials (step 5);
   - Resources given, concepts not all covered (`stage: learn` or `concepts`):
     the check (step 6). If the learner hasn't read yet and wants to, the
     materials are in the note: show the list again with the same Next.
5. **Explain and give the materials**, in one reply (teaching.md, "Learn:
   explain, then materials"), in the format below. Record the list in the topic
   note's Resources, keep `stage: learn`, and write the journal line with
   `Next:`.
6. **Check**, when the learner is back: they say they've read it, or run
   `/tutor:teach` again. One question per concept not yet covered, one at a
   time (teaching.md, "Learn"). Tick each concept in the topic note with a
   one-line summary of how the learner showed it, add weak spots, set
   `stage: concepts`, and update the plan's Result (`concepts 3/5`).
7. **When all concepts are covered**, set the stage to `practice` (or `quiz` if
   the topic has no practice) and end with the next command: `/tutor:practice`
   or `/tutor:quiz`.
8. **When the learner stops**, add to today's journal entry what was covered and
   an exact `Next:`.

The learner can ask for something else at any moment: "explain it more
simply", "find me a video on this", "skip the reading, ask me". Do what they
ask, then carry on from where the topic note says they are.

## Output

After the explanation:

```text
<Topic>

1. <concept>
   <two to four sentences, a small example if it helps>
2. <concept>
   <...>

To understand it better, read or watch:
1. <type> · <title> — <link> · ~<n> min · <what it gives; "start here" for the first>
2. <type> · <title> — <link> · ~<n> min · <what it gives>

Next: read them, then /tutor:teach — I'll check each concept with a question.
```

After the check:

```text
<Topic>: <concepts covered> of <total> concepts.
Weak spots: <if any>.
Next: /tutor:practice — <the task, very short> | /tutor:quiz — <if there is no practice>
```

## Definition of done

- The learner got the explanation and the materials without having to choose.
- Every concept checked in this run has its result in the topic note.
- The plan, the topic note and the journal are up to date.
- The reply ends with the next command.
