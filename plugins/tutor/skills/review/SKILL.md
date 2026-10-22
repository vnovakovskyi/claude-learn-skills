---
name: review
description: Review what the learner has already covered in their tutor course — the topics due by the spaced-repetition schedule, a finished topic or module, the last topics after a break, or their weak spots. Asks first and then recaps what was missed (or recaps first for a quick reminder), and records the next review date. Use when the learner wants to review, repeat or recap what they've studied, asks to be reminded of a topic, or asks what's due for review.
argument-hint: "[topic | module | recent | weak] [recap]"
---

# Tutor: Review

You are running a **Review**: the learner goes back over what they've already
covered.

What to review (optional):

`$ARGUMENTS`

## Purpose

Keep finished topics from fading: bring them back at the right time, find what
has slipped, and fill it in.

## Before you start

Read these files that ship with the plugin:

- `${CLAUDE_SKILL_DIR}/../../references/workbook.md` (required)
- `${CLAUDE_SKILL_DIR}/../../references/teaching.md` (required: "Reviews",
  "Open questions", "Asking with choices")
- `${CLAUDE_SKILL_DIR}/../../references/sync.md` — if `/tutor:open` hasn't
  run in this conversation

## Hard rules

- Talk to the learner in the language they write in; write the workbook in
  English (workbook.md, "Language").
- Questions before reminders, unless the learner asks for a recap first.
- One question at a time; wait for the answer.
- Review only what the learner has covered: `done`, `known` and `refresh`
  topics, or concepts already ticked in a topic in progress. Nothing new.
- Never change the curriculum. Write only in the workbook.

## Choose what to review

| Argument | Review |
|---|---|
| none | The topics that are due (`Next review` today or earlier), most overdue first, at most three. None due: say when the next one is, and offer the last finished topics, the weak spots, or a module. |
| a topic | That topic, due or not. |
| a module | The module's finished topics: one or two questions each, mixed. |
| `recent` | The last three finished topics. The natural choice after a break. |
| `weak` | The weak spots recorded in the topic notes, newest first. |
| `recap` (with any of the above) | Recap first, then a question or two (teaching.md, "How a review goes"). |

The learner can ask for questions with answer options instead of open ones.

## Process

1. **Open the workbook** (workbook.md). None: offer `/tutor:enroll` and stop.
   If `/tutor:open` hasn't run in this conversation, sync with the curriculum
   (sync.md).
2. **Choose** what to review (above), and announce it: the topics, about how
   many questions, about how many minutes.
3. **Prepare.** Read the curriculum files of those topics and their topic
   notes: objectives, weak spots, earlier questions and how they went.
4. **Review** as teaching.md, "How a review goes", describes: questions mixed
   across topics, a verdict and the key point after each answer, then a short
   recap of what was missed. Record each answer in its topic note as you go.
5. **Record the result** for each topic:
   - a line in the topic note's Reviews: date, score, next review date;
   - the weak spots: add what was missed, remove what has healed;
   - in `plan.md`: `Next review` by the interval rules;
   - a line in today's journal entry, and `Next:`.
6. **A topic below 50%:** offer a short re-teach of what was missed, now or in
   the next session (`/tutor:teach <concept>`).

## Output

```text
Review — <score>/<total>
  <topic>    2/3  → next review <date>
  <topic>    3/3  → next review <date>

Recapped:   <what was missed, in a few words>
Still weak: <weak spots that remain>
Next due:   <date: topics> | nothing scheduled yet
```

## Definition of done

- Every question and verdict is recorded in its topic note.
- Each reviewed topic has its new `Next review` date in the plan.
- Weak spots are up to date.
