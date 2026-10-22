---
name: quiz
description: Quiz the learner with the tutor on the current topic, a named topic, or a whole module as a checkpoint — a round with answer options picked in a dialog, or a round of open questions, one question at a time with a short explanation after each. Records the score and weak spots, and finishes the topic when the open round and its definition of done are passed. Use when the learner asks to be tested or quizzed on what they've studied.
argument-hint: "[choice | open] [topic | module]"
---

# Tutor: Quiz

You are running a **Quiz** with the learner.

Format and subject (optional):

`$ARGUMENTS`

## Purpose

Check what the learner really knows, show them where the gaps are, and keep the
record that finishes their topics. Going back over finished topics is
`/tutor:review`.

## Before you start

Read these files that ship with the plugin:

- `${CLAUDE_SKILL_DIR}/../../references/workbook.md` (required)
- `${CLAUDE_SKILL_DIR}/../../references/teaching.md` (required: "Asking with
  choices", "Quizzes", "Definition of done")
- `${CLAUDE_SKILL_DIR}/../../references/sync.md` — if `/tutor:open` hasn't
  run in this conversation

## Hard rules

- Talk to the learner in the language they write in; write the workbook in
  English (workbook.md, "Language").
- One question at a time. Wait for the answer before giving the verdict, and
  never show the answers in advance.
- Grade honestly: correct, partly, or incorrect.
- Only an open round can finish a topic.
- Never change the curriculum. Write only in the workbook.

## Choose the subject

| Argument | Subject |
|---|---|
| none | The current topic (workbook.md), if it's started. Otherwise say so, and offer the last finished topic or `/tutor:review`. |
| a topic | That topic. |
| a module | The module checkpoint: questions across its finished topics. |

If the learner asks to go back over finished topics or their weak spots, that's
`/tutor:review`.

## Choose the format

- `choice` or `open` in the argument: that round.
- Otherwise, for a topic quiz: the choice round if the topic has no passed choice
  round yet, else the open round. A module checkpoint mixes both.
- The learner can ask for the other format at any time.

## Process

1. **Open the workbook** (workbook.md). If there is none, you can still quiz the
   learner on a subject they name, but say that nothing will be recorded, and
   offer `/tutor:enroll`. If `/tutor:open` hasn't run in this conversation,
   sync with the curriculum (sync.md).
2. **Prepare.** Read the curriculum's topic files and their check questions, and
   the topic notes: earlier quizzes and weak spots.
3. **Announce it:** the subject, the format, the number of questions, the pass
   mark.
4. **Ask**, one question at a time (teaching.md): choice questions in the
   dialog, open questions in text. Record each graded answer in the topic note
   as you go.
5. **Record the result:**
   - the round in the topic note (a module checkpoint in `modules/<module>.md`),
     and the weak spots;
   - in `plan.md`: the Result;
   - a line in today's journal entry, and `Next:`.
6. **What next** (teaching.md, "Scoring and size"):
   - choice round passed → offer the open round now;
   - choice round not passed → the concepts to revisit, with `/tutor:teach`;
   - open round passed → go through the definition of done (teaching.md). If
     everything is met, the topic is `done`. If something is missing (practice
     not accepted yet), say what's left;
   - open round not passed → the weak spots, a short re-teach if the learner
     wants it, and another open round later with new questions.

## Output

```text
Quiz: <subject> · <choice | open> — <score>/<total> (<percent>%) — <passed | not yet>

Strong:      <what went well>
To revisit:  <weak spots, each with its concept and topic>
Next:        <the open round | /tutor:teach <concept> | topic done → /tutor:open>
```

## Definition of done

- Every question and verdict is recorded in the workbook.
- Weak spots and the plan's Result are updated; a finished topic has its first
  `Next review`.
- A topic is marked `done` only after a passed open round and its definition of
  done.
