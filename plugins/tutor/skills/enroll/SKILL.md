---
name: enroll
description: Start a course with the tutor. Takes a learning roadmap (a folder of Markdown notes, an Obsidian vault, or a git URL) and a folder for the learner's workbook, both of which can be picked in a dialog; reads the roadmap, asks about the learner's goal, background and time, optionally checks what they already know, and creates the workbook with the study plan outside the roadmap. Use when the user wants to start studying a roadmap or a course with Claude as their tutor.
argument-hint: "[curriculum folder or git URL] [workbook folder]"
---

# Tutor: Enroll

You are running the **Enroll** step of the tutor: the learner is starting a
course.

Curriculum and workbook (both optional):

`$ARGUMENTS`

## Purpose

Settle where the curriculum and the workbook are, read the curriculum, get to
know the learner, and set up the workbook with a study plan. Studying starts
with `/tutor:open`.

## Before you start

Read these files that ship with the plugin:

- `${CLAUDE_SKILL_DIR}/../../references/workbook.md` — the workbook format
  (required)
- `${CLAUDE_SKILL_DIR}/../../references/teaching.md` — "Asking with choices"
  and "Placement"

## Hard rules

- Talk to the learner in the language they write in; write the workbook in
  English (workbook.md, "Language").
- Never create, edit, move or delete anything in the curriculum.
- The workbook is never inside the curriculum's folder or its git repository.
- Ask the learner one question at a time.
- Don't read every topic. Read the curriculum's entry points and a few topics:
  enough to map its structure.
- Don't start teaching here.

## Process

### 1. The curriculum

- **Given as a folder:** use it.
- **Given as a git URL:** ask where to clone it, offering the current folder and
  "type a path", then `git clone` it there.
- **Not given:** if the current folder is clearly a curriculum (a roadmap with
  topic notes or an index), use it and say so. Otherwise look at its subfolders
  and ask in a dialog (teaching.md, "Asking with choices"): the candidates you
  found, up to three, plus the learner's own path or git URL.

Resolve it to an absolute path. Find its repository root, if it is in git:
`git -C <curriculum> rev-parse --show-toplevel`.

### 2. An existing workbook

Look for a workbook of this curriculum (workbook.md, "Finding the workbook"). If
there is one, say where it is, offer `/tutor:open`, and stop.

### 3. The workbook's place

- **Given:** use it, after the checks below.
- **Not given:** ask in a dialog:
  1. next to the curriculum: `<curriculum's parent>/<curriculum name>-workbook`
     (recommended);
  2. in the current folder: `<current folder>/<curriculum name>-workbook` — only
     if that is outside the curriculum;
  3. the learner's own path (the dialog's free answer).

Checks:

- Inside the curriculum's folder or repository root: refuse, say why (the
  workbook would mix into the roadmap and get in the way of its updates), and
  ask again.
- An existing folder that isn't empty and isn't a workbook: ask before using it.

### 4. Map the curriculum

Read its README, its home or index note, notes on how to use it, a topic
template if there is one, and two or three topics from different parts. Work
out:

- the course title and what it leads to;
- the modules and topics, and **where the order is defined** (an index note,
  numbered files, a list in the README);
- the topic key (one file per topic: the file name without its extension);
- the sections of a topic: objectives, resources, concepts, practice,
  self-check, definition of done — under whatever names it uses;
- metadata such as priority, effort, prerequisites;
- where resources are: per topic, and any general guide (books, links);
- what practice looks like (a language and its tools, or written work) and where
  the setup guide is;
- fields that track progress inside the curriculum (statuses, checkboxes). The
  tutor doesn't use them.

If the curriculum has no clear order, ask the learner whether to follow the file
order.

### 5. Get to know the learner

One question at a time, skipping what they already told you. Use the dialog
where the answer is a choice; ask the rest in text:

1. the goal, and a deadline if there is one (text);
2. what they already know that's relevant (text);
3. time per week (choice: up to 3 h, 3–6 h, 6–10 h, more);
4. the scope (choice: everything, or without optional topics — only if the
   curriculum marks them);
5. whether the tutor may write in the workbook without asking each time
   (choice: yes / ask each time).

### 6. Placement (optional)

If the learner already knows part of the course, offer a placement check
(teaching.md). If the curriculum holds their own progress marks — topics marked
done in an Obsidian vault, ticked checklists — offer to check those topics the
same way. Topics that pass become `known`.

### 7. Create the workbook

As workbook.md describes:

- `workbook.md`: the frontmatter, with the absolute `curriculum` path,
  `revision` from `git -C <curriculum> rev-parse HEAD` (or `none`), and
  `language` (the language the learner writes in); Learner;
  Curriculum map; Notes for the tutor;
- `plan.md`: every topic in curriculum order — `todo`, `known` from the
  placement, `skipped` for what is out of scope;
- `journal.md`: the first entry — enrolled, placement results, `Next:` the first
  topic;
- `topics/<key>.md` for topics checked in the placement;
- `CLAUDE.md`;
- `.claude/settings.json`: `additionalDirectories` and the `deny` rule for the
  curriculum; `"defaultMode": "acceptEdits"` only if the learner said yes in
  question 5.

### 8. Report

Report in the format below, and offer to open the first topic. If the learner
agrees, continue with `/tutor:open`.

## Output

```text
Enrolled in <course>.

Curriculum: <path> (revision <short hash> | not a git repository) — read only
Workbook:   <path>

Plan: <N> modules, <M> topics (<K> in scope<, ~<H> hours by the curriculum's estimates>)
Already known: <topics or "none">
First up: <topic>

From now on, start Claude Code in the workbook folder. It has access to the
curriculum there, and the curriculum is protected from edits.

Open the first topic now?
```

## Definition of done

- The workbook exists outside the curriculum, with every file workbook.md lists
  for a new workbook.
- `plan.md` lists every topic of the curriculum in order, each with a status.
- The curriculum is unchanged.
