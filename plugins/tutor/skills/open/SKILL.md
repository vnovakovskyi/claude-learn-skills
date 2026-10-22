---
name: open
description: Open a topic or module of the learner's tutor course, by default the current one. Catches up with changes in the roadmap, then shows what to learn — for a topic, why it matters, its objectives, concepts, practice task, definition of done and the plan; for a module, its topics and their status. Changes no progress. Use when the learner wants to start or continue studying, asks what's next or where they stopped, or asks to open a topic or module.
argument-hint: "[topic | module]"
---

# Tutor: Open

You are running **Open**: show the learner the topic or module they are about to
study.

Topic or module (optional):

`$ARGUMENTS`

## Purpose

Before any teaching, give the learner the picture: where they are, what this
topic or module is about, what's inside it, and how they'll go through it.
Teaching starts when they ask for it, with `/tutor:teach`.

## Before you start

Read these files that ship with the plugin:

- `${CLAUDE_SKILL_DIR}/../../references/workbook.md` (required)
- `${CLAUDE_SKILL_DIR}/../../references/sync.md` (required)
- `${CLAUDE_SKILL_DIR}/../../references/teaching.md` — "The path through a
  topic"

## Hard rules

- Talk to the learner in the language they write in; write the workbook in
  English (workbook.md, "Language").
- Don't teach here, and don't start the topic: no status changes, no topic note.
- Apart from syncing, write nothing.
- Never change the curriculum.

## Process

1. **Open the workbook** (workbook.md, "Finding the workbook"). None: offer
   `/tutor:enroll` and stop. Read `workbook.md`, `plan.md`, and the last entry of
   `journal.md`.
2. **Sync with the curriculum** (sync.md), and tell the learner what changed, if
   anything did.
3. **Choose what to open:**
   - an argument: the topic or module it names (match loosely: a number, part
     of a name; ask if it's ambiguous);
   - otherwise the current topic (workbook.md).

   If the requested topic comes before something it builds on that the learner
   hasn't done, say so in one line; it's still their choice.
4. **Read** the topic's curriculum file in full, and its topic note if there is
   one. For a module, read the plan's rows for it and skim its topics' headings
   and objectives.
5. **Show the overview** in the format below. Add a line about due reviews
   (`Next review` today or earlier) and gaps, if there are any. If the
   journal's last entry is two weeks old or more, suggest
   `/tutor:review recent` before new material.
6. **Offer the next step**: for a topic, `/tutor:teach` (or a specific
   concept), `/tutor:practice` or `/tutor:quiz`, depending on its stage; for a
   module, opening one of its topics.

## Output

For a topic:

```text
Module <n> — <module> · topic <i> of <count>
<Topic title> · <priority> · ~<effort> h · <new | in progress: <stage> | refresh: <what changed> | done>

Why: <one or two sentences>

You'll be able to:
- <objective>
- <objective>

Concepts:
  1. ✓ <concept covered>
  2. → <the concept to continue with>
  3.   <concept>

Practice: <the task in one line>
Done when: <the definition of done, shortened>

Plan:
  1. Learn the concepts — I explain, or you read or watch first   /tutor:teach
  2. Practice — <the task, very short>                          /tutor:practice
  3. Quiz — options first, then open questions                  /tutor:quiz

<Reviews due: <topics> — /tutor:review>
<Gaps: <topics> — added to the course behind you>

Say "teach me" to start, or name a concept to begin with.
```

For a module:

```text
Module <n> — <module> · <finished> of <count> topics · ~<hours left> h left

What it's about: <one or two sentences>

Topics:
  ✓ <topic> — done, <result>
  → <topic> — in progress: <stage>
    <topic>
    <topic> — new, behind you

After the last topic: a module checkpoint — /tutor:quiz <module>
Open a topic: /tutor:open <topic>
```

## Definition of done

- The learner sees the topic or module, what's in it, and the plan.
- Nothing but the sync was written.
