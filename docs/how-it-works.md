# How the tutor works

Three parts, like a school:

- **The curriculum** is the program and the textbook: a roadmap folder such as
  [java-roadmap](https://github.com/vnovakovskyi/java-roadmap). Its author keeps
  improving it. The tutor only reads it.
- **The tutor** is the teacher: this plugin. It reads the curriculum, shows the
  learner what to learn, teaches, reviews practice, quizzes, and adapts to
  changes.
- **The workbook** is the learner's own folder, anywhere outside the
  curriculum: the plan, the results, the code, the history.

Keeping them apart means the curriculum can change at any time without breaking
anyone's progress, a learner can take the author's updates with `git pull`, and
the workbook stays readable Markdown that belongs to the learner.

## Enrolling

Run `/tutor:enroll` in the curriculum's folder, or anywhere and name the
curriculum. The tutor asks where the workbook should go, in a dialog: next to
the curriculum (the default), in the current folder, or any path you type. It
refuses a place inside the curriculum or its git repository. Then it asks about
your goal, background and time, offers a placement check for what you
already know, and creates the workbook.

The workbook becomes your home for the course: start Claude Code there. Its
`.claude/settings.json` gives the session access to the curriculum and makes
Claude Code refuse any edit inside it, so "the tutor never changes the roadmap"
is enforced, not only promised.

## A topic, step by step

```text
> /tutor:open
Module 4 — Essential Java APIs · topic 8 of 10
Optional and Null-Safety · must · ~3 h · in progress: concepts

Concepts:
  1. ✓ null means absence; NPE
  2. ✓ Creating an Optional: of / ofNullable / empty
  3. → Consuming: orElse, orElseThrow, isPresent / isEmpty
  4.   Optional for return types only
  5.   Other guards: requireNonNull, empty collections

Practice: findByName over a map, without .get()

Plan:
  1. Learn the concepts — I explain, or you read or watch first   /tutor:teach
  2. Practice — findByName with Optional                        /tutor:practice
  3. Quiz — options first, then open questions                  /tutor:quiz

Reviews due: Enums — /tutor:review
Say "teach me" to start, or name a concept to begin with.
```

1. **Open** — `/tutor:open` shows where you are, or any topic or module you
   name. It changes nothing.
2. **Teach** — `/tutor:teach` ("teach me") says exactly what to learn and asks
   how: the tutor explains each concept briefly, or offers two to four resources
   — the roadmap's own first, then articles, videos or books found on the web,
   each link checked — or both. Every concept ends with a check question.
3. **Practice** — `/tutor:practice` gives the task, answers "I'm stuck" with
   hints rather than solutions, and reviews your work: runs the code, checks it
   against the task, gives a verdict.
4. **Quiz** — `/tutor:quiz` first runs a round with answer options, picked in a
   dialog; then a round of open questions in your own words or code. Only the
   open round can finish a topic, together with the roadmap's definition of
   done.
5. **Review** — `/tutor:review` brings finished topics back at growing
   intervals (3, 7, 21, 60, 150 days): questions first, mixed across topics,
   then a recap of what was missed. Ask for a reminder and it recaps first.
   After a break of two weeks or more, it's the first thing the tutor
   suggests.

Every result is written down as it happens: the resources you used, which
concepts you've covered, each practice review, each quiz question with your
answer and the verdict, your weak spots. The journal's last line says where to
continue, so a new session starts where the last one stopped.

The tutor talks in the language you write in and switches when you do. The
workbook is always written in English.

## When the curriculum changes

Before it shows or teaches anything, the tutor compares the curriculum with the
revision it last synced to (through git), and once a week checks whether the
curriculum's upstream has new commits; it offers to pull them and pulls only if
you agree.

| The curriculum… | The tutor… |
|---|---|
| adds a topic ahead of you | puts it in the plan at its place. |
| adds a topic behind you | treats it as a gap and fits it into the next sessions. |
| changes a topic you've finished (new concept, new task, new question) | marks it `refresh` and teaches just the new part, with a short quiz. |
| fixes wording, typos or links | does nothing. |
| renames or moves a topic | keeps your progress under the new name. |
| rewrites a topic under a new name, or splits it | finds the successor, keeps your progress, and teaches what's new (asks you when unsure). |
| removes a topic | marks it `retired`; your notes stay. |
| reorders topics | follows the new order for what's left. |

Without git the tutor still notices added, removed and reordered topics, but not
edits inside a topic.

## Statuses

| Status | Meaning |
|---|---|
| `todo` | Not started. |
| `doing` | Started, not finished. |
| `done` | Finished: definition of done met. |
| `known` | You already knew it; a placement check confirmed it. |
| `skipped` | You chose to skip it. |
| `refresh` | Finished before, but the curriculum has added something to it since. |
| `retired` | Removed from the curriculum. |

## Tips for learners

- Start Claude Code in the workbook folder.
- The tutor writes to the workbook often. At enrollment you can let it write
  there without asking each time; it's the `defaultMode` line in the workbook's
  `.claude/settings.json`, and you can remove it later.
- With the tutor, don't track progress inside the curriculum (statuses,
  checkboxes in an Obsidian vault). The tutor doesn't read it, and local edits
  get in the way of `git pull`.
- The workbook is plain Markdown. Open it in Obsidian, or put it under git — the
  practice code makes a portfolio.

## For roadmap authors

Any folder of Markdown works. These make it work well:

- **One topic per file**, with a stable file name. Renames are followed through
  git, but stable names are simpler.
- **One place that defines the order**: an index note, or numbered file names.
- **The same sections in every topic**: objectives, resources, concepts,
  practice, self-check questions with answers, definition of done. Self-check
  questions are the base of the tutor's quizzes, and the definition of done is
  the bar for finishing a topic.
- **Metadata, if you have it**: priority (must, good, optional), effort in
  hours, prerequisites.
- **A setup guide** for the tools the practice needs.
- **A git repository with clear commits**: the tutor reads the history to tell
  learners what changed.

[java-roadmap](https://github.com/vnovakovskyi/java-roadmap) follows all of
these.
