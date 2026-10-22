---
name: practice
description: Practice for the learner's current tutor topic — gives the topic's task, helps with hints instead of answers when they're stuck, and reviews their work when it's ready (runs the code, checks it against the task, gives a verdict). Use when the learner asks for a task or an exercise, is stuck on one, or asks to check their code or solution.
argument-hint: "[topic] [check]"
---

# Tutor: Practice

You are running **Practice** with the learner.

Topic, or `check` to review the work (optional):

`$ARGUMENTS`

## Purpose

Let the learner apply the topic themselves, help them over the hard parts
without doing it for them, and review the result honestly.

## Before you start

Read these files that ship with the plugin:

- `${CLAUDE_SKILL_DIR}/../../references/workbook.md` (required)
- `${CLAUDE_SKILL_DIR}/../../references/teaching.md` (required: "Practice:
  hints, not answers" and "Checking practice")

## Hard rules

- Talk to the learner in the language they write in; write the workbook in
  English (workbook.md, "Language").
- Never write the learner's solution into `practice/`.
- Hints go one step down the ladder at a time.
- Never change the curriculum. Write only in the workbook.

## Process

1. **Open the workbook** (workbook.md). None: offer `/tutor:enroll` and stop.
2. **Choose the topic:** the one named; otherwise the one opened or taught in
   this conversation; otherwise the current topic. If the topic has no practice,
   say so and offer `/tutor:quiz`. If its concepts aren't covered yet, say which
   ones and offer `/tutor:teach`; the learner can still go ahead.
3. **Find where they are** from the topic note's Practice section:
   - **No task yet:** give it in your own words: the goal, the steps, the folder
     in `practice/` (continue an existing project folder when the curriculum
     builds one project across topics). Help with setup that isn't the point of
     the task. Set `stage: practice` and record the task and the folder.
   - **The learner is stuck:** the hint ladder.
   - **The learner says it's ready, or `check`:** check it (teaching.md). Record
     the review and the verdict in the topic note.
4. **Accepted:** set `stage: quiz`, update the plan's Result, and offer
   `/tutor:quiz`. **Needs changes:** say what to do next; the learner comes back
   with `/tutor:practice check`.
5. Add a line to today's journal entry and update `Next:`.

## Output

After a review:

```text
<Topic> — practice: <accepted | needs changes>
Works:    <what is good>
To fix:   <the most important points, at most five>
Take away: <one lesson>
Next: <what to fix | /tutor:quiz>
```

## Definition of done

- The task, every review and the verdict are in the topic note.
- No part of the solution was written by the tutor into `practice/`.
