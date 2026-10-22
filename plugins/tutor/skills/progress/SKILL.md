---
name: progress
description: Show the learner's progress in their tutor course — finished and remaining topics by module, the topic in progress, gaps and refreshes from roadmap changes, due reviews, weak spots, and recent activity. Use when the learner asks where they are, how far they've come, what's left, or what changed in the course.
argument-hint: "[module]"
---

# Tutor: Progress

You are running **Progress**: an overview of where the learner stands.

Module to detail (optional):

`$ARGUMENTS`

## Purpose

Give the learner a clear picture of the course: what's done, what's left, what
needs attention.

## Before you start

Read these files that ship with the plugin:

- `${CLAUDE_SKILL_DIR}/../../references/workbook.md` (required)
- `${CLAUDE_SKILL_DIR}/../../references/sync.md` (required)

## Hard rules

- Talk to the learner in the language they write in; write the workbook in
  English (workbook.md, "Language").
- Apart from syncing, write nothing.
- Never change the curriculum.

## Process

1. **Open the workbook** (workbook.md). None: offer `/tutor:enroll` and stop.
2. **Sync with the curriculum** (sync.md), so the picture matches the curriculum
   as it is now.
3. **Count** from `plan.md`. A topic counts as finished when it is `done` or
   `known`; `skipped` and `retired` topics don't count at all.
   - Per module and overall: finished out of the topics in scope.
   - Hours, if the curriculum gives an effort per topic: finished out of the
     total in scope.
   - The topic in progress and its stage.
   - Gaps (`todo` before the frontier) and `refresh` topics, with why.
   - Reviews due today and in the next 7 days.
   - Weak spots: the most recent from the topic notes, three to five.
   - Activity from `journal.md`: days of study in the last 7 days, the last one.
4. **Report** in the format below. With a module argument, add a row per topic
   of that module: status, result, next review.

## Output

```text
<Course> — <finished> of <in scope> topics (<percent>%)<, ~<hours> of ~<total> h>

Modules
  1  Foundations & Setup      8/8   ██████████
  4  Essential Java APIs      7/10  ███████░░░  ← now
  5  Modern Java              0/10  ░░░░░░░░░░

Now:        <topic> (<stage>)
Gaps:       <topic — why> | none
Refresh:    <topic — what changed> | none
Reviews:    <due today — /tutor:review> · <next 7 days>
Weak spots: <three to five>
Activity:   <N> days of study in the last 7 days, last on <date>

Next: /tutor:open
```

## Definition of done

- The report matches `plan.md` after the sync.
- Nothing but the sync was written.
