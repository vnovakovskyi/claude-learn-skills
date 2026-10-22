# The workbook

The workbook is the learner's folder. Everything about the learner lives here:
the plan, the results, the code, the history. The curriculum (the roadmap the
learner follows) stays untouched, so its author can keep changing it.

## Where it lives

Anywhere the learner chooses, **outside the curriculum's folder and outside its
git repository**. The default is next to the curriculum:

```text
learning/
  java-roadmap/                   ← the curriculum (read only)
  java-roadmap-workbook/          ← the workbook: the learner's home for the course
```

The workbook is where the learner starts Claude Code for the course. Its
`.claude/settings.json` gives those sessions access to the curriculum, wherever
it is.

## Language

- **Talk in the learner's language**: the language of their latest message.
  Everything the learner sees in the conversation is in that language —
  questions, explanations, quiz questions and options, reviews, reports. The
  output formats in the skills show the structure, not the wording.
- **Nothing to go by yet** (the session started with a bare command): use
  `language` from `workbook.md`. At enrollment, before the learner has written
  anything, start in English and switch as soon as they write.
- **The learner switches language**: switch with them, and update `language`.
- **Write the workbook in English**: every file, note and journal line,
  including the summaries of the learner's answers, whatever language the
  conversation is in.
- Technical terms, code and API names stay as the curriculum writes them;
  titles of resources stay as they are.

## Finding the workbook

A folder is a workbook when it has a `workbook.md` whose frontmatter contains
`tutor-workbook`. Look in this order:

1. The folder or course the learner named.
2. The current folder.
3. Its direct subfolders.
4. If the current folder is a curriculum: a sibling folder whose `workbook.md`
   points at it.

One found: use it. Several: take the one the learner named; otherwise the one
with the most recent journal entry, and say which one you took. None: ask the
learner where their workbook is, or offer `/tutor:enroll`.

If Claude Code was started outside the workbook, suggest starting it in the
workbook next time: that is where the settings and `CLAUDE.md` work.

## Files

```text
java-roadmap-workbook/
  workbook.md              the course, the learner, and a map of the curriculum
  plan.md                  every topic in order with its status — the source of truth for progress
  journal.md               one entry per day of study, newest last
  topics/                  one note per topic the learner has started
  modules/                 one note per finished module
  practice/                the learner's code and other practice work
  CLAUDE.md                tells every session in this folder that it's a tutor workbook
  .claude/settings.json    access to the curriculum, and a ban on editing it
```

Create `topics/`, `modules/` and `practice/` when their first file appears.

### workbook.md

```markdown
---
tutor-workbook: 1
course: Java Developer Roadmap
curriculum: /Users/alex/learning/java-roadmap
revision: a624939
upstream-checked: 2026-10-22
language: ru
---

# Java Developer Roadmap — workbook

## Learner
- Goal: a junior Java job by summer 2027
- Background: some Python, no Java
- Time: ~10 h/week, sessions of ~60 min
- Scope: `must` and `good` topics; `optional` only on request
- Learns best: a short explanation first, then an article to go deeper

## Curriculum map
- Order: `Home.md` → "Module Index", modules and topics top to bottom
- Topics: `Topics/*.md`, one per file; key = file name without `.md`
- Sections: Learning Objectives · Learn · Concepts · Practice · Self-check · Definition of Done
- Metadata: `priority` (must / good / optional) and `effort` (hours) in frontmatter
- Resources: per topic under "Learn"; books in `Guides/Books & Resources.md`
- Practice: Java 25 code; Maven or Gradle from Module 6; setup in `Guides/Tools & Setup.md`
- Ignore: the curriculum's own progress fields (`status`, checkboxes)

## Notes for the tutor
- Likes a code example before the explanation.
```

- `curriculum`: the absolute path to the curriculum. If it isn't there any more,
  ask the learner where it is now, and update this field and
  `.claude/settings.json`.
- `revision`: the curriculum commit the plan is synced to; `none` when the
  curriculum is not a git repository.
- `upstream-checked`: when updates from the curriculum's upstream were last
  checked (sync.md).
- `language`: the language the learner last wrote in (see "Language"). The
  workbook itself is always in English.
- **Curriculum map**: what the tutor learned about the curriculum's structure at
  enrollment. Read it instead of rediscovering the structure every session, and
  update it when the structure changes.
- **Notes for the tutor**: how this learner learns best. Add a line when you
  notice something that holds across sessions.

### plan.md

```markdown
# Plan

Synced to curriculum revision `a624939` on 2026-10-22.

Statuses: todo · doing · done · known · skipped · refresh · retired

## Module 4 — Essential Java APIs

| Topic | Status | Updated | Result | Next review |
|---|---|---|---|---|
| M4 - Java APIs - Wrapper Types, Autoboxing and BigDecimal | done | 2026-10-25 | open quiz 5/5 | 2026-10-28 |
| M4 - Java APIs - Optional and Null-Safety | doing | 2026-10-27 | concepts 3/5 | |
| M4 - Java APIs - Packages and Project Organization | todo | | | |
```

- One section per module, and rows within it, in curriculum order. A curriculum
  without modules gets a single section.
- **Topic** is the topic key from the curriculum map. It also names the topic
  note: `topics/<key>.md`.
- **Updated** is the date of the last status change.
- **Result** is short: where the learner is in the topic, a quiz score,
  `added 2026-11-01`, what changed for a refresh.
- Retired topics move to a final `## Retired` section.

| Status | Meaning |
|---|---|
| `todo` | Not started. |
| `doing` | Started, not finished. |
| `done` | Finished: met the topic's definition of done (teaching.md). |
| `known` | The learner already knew it, and a placement check confirmed it. |
| `skipped` | The learner chose to skip it. |
| `refresh` | Was done or known, but the curriculum has since changed it materially; the new part still has to be learned. |
| `retired` | Removed from the curriculum. Kept for history, not counted. |

**Frontier**: the last topic in plan order whose status is `doing`, `done` or
`known`. A `todo` or `refresh` topic **before** the frontier is a **gap**:
something behind the learner that they haven't covered, usually a topic the
curriculum added later. A gap is not a status of its own; derive it from the
plan.

**The current topic** is, in this order: the `doing` topic (the most recently
updated, if there are several); a `refresh` topic; the first gap; the next
`todo` topic in plan order within the learner's scope.

### journal.md

```markdown
# Journal

## 2026-10-27
- Curriculum: no changes.
- Opened: Optional and Null-Safety.
- Taught: concepts 1–3 of 5 (short explanations; read the dev.java article).
- Quiz: Enums review 2/2.
- Next: concepts 4–5, then the practice task.
```

One entry per day of study, appended at the end. Every skill that does
something adds its line to today's entry (creating the entry if there is none)
and keeps `Next:` as the last line, up to date. The last `Next:` is where the
learner stopped.

### topics/\<key\>.md

```markdown
---
topic: M4 - Java APIs - Optional and Null-Safety
source: Topics/M4 - Java APIs - Optional and Null-Safety.md
revision: a624939
stage: concepts
started: 2026-10-27
finished:
---

# Optional and Null-Safety

## Resources
- dev.java — Using Optionals (from the curriculum) — read 2026-10-27
- Video: "Java Optional in 12 minutes" (channel, 12 min) — found for the learner

## Concepts
- [x] null means absence; dereferencing it throws NPE — explained it in own words.
- [x] Creating: `of` / `ofNullable` / `empty` — mixed up `of` and `ofNullable` at first.
- [ ] Consuming: `orElse`, `orElseThrow`, `isPresent` / `isEmpty`

## Practice
- Task: `Optional<User> findByName(String)` over a map.
- Work: `practice/m4-optional/`
- 2026-10-28 — needs changes: one unchecked `.get()`; no test for a missing name.
- 2026-10-29 — accepted.

## Quiz
### 2026-10-29 — choice — 6/7 (86%)
1. What does `Optional.of(null)` do? — chose "returns an empty Optional" ✗ → throws NPE.
### 2026-10-29 — open — 4/5 (80%)
1. Why return `Optional<User>` instead of a nullable `User`? — ✓

## Weak spots
- `of` vs `ofNullable`

## Definition of Done
- [x] Uses `Optional` idiomatically — practice review 2026-10-29
- [x] Can explain when not to use `Optional` — open quiz Q4

## Reviews
- 2026-11-01 — 2/2 → next 2026-11-08
```

- `source`: the topic's file inside the curriculum.
- `revision`: the curriculum revision the topic was last studied at. Sync uses
  it to show what changed since.
- `previous-source`: only after the curriculum renamed or replaced the topic —
  its old path, to compare with the new file (sync.md). Remove it, and update
  `revision`, once the learner has caught up with the changes.
- `stage`: where the learner is in the topic: `learn`, `concepts`, `practice`,
  `quiz`, `done`.
- Summarize the learner's answers; don't copy them word for word.
- Sections appear as the learner reaches them and follow the curriculum: a
  topic without practice has no Practice section.

### modules/\<module\>.md

Written when the last topic of a module is finished: the module's topics and
results, the practice work, the module checkpoint (if taken), and the weak spots
carried forward.

### practice/

The learner's work: one folder per task, named after the topic in short form
(`practice/m4-optional/`). When the curriculum's practice builds one project
across topics, keep working in that project's folder (`practice/bookshelf/`).
The learner writes the work; the tutor reads it, runs it, and reviews it.

### CLAUDE.md

Short, so it costs little in every session:

```markdown
# Java Developer Roadmap — tutor workbook

This folder is a study workbook of the `tutor` plugin. The curriculum is
`/Users/alex/learning/java-roadmap`: read it, never change it.

To study: `/tutor:open` (where am I, what's next), `/tutor:teach`,
`/tutor:practice`, `/tutor:quiz`, `/tutor:review`, `/tutor:progress`.
```

### .claude/settings.json

```json
{
  "permissions": {
    "additionalDirectories": ["/Users/alex/learning/java-roadmap"],
    "deny": ["Edit(//Users/alex/learning/java-roadmap/**)"]
  }
}
```

- `additionalDirectories` lets sessions started in the workbook read the
  curriculum.
- The `deny` rule makes Claude Code refuse any file edit inside the curriculum.
  The absolute path follows `//`.
- Add `"defaultMode": "acceptEdits"` under `permissions` only if the learner
  agreed that the tutor may write in the workbook without asking each time.

## Writing rules

- Write only inside the workbook. Never create, edit, move or delete anything in
  the curriculum. Run no git command that changes it, except the
  `git pull --ff-only` the learner agreed to (sync.md).
- Record as you go. When a concept is covered, a quiz answer is graded, or a
  practice review is done, update the topic note and the plan right then. A
  session can end at any moment.
- `plan.md` is the source of truth for status. Change a status there first.
- Dates are ISO: `2026-10-27`.
