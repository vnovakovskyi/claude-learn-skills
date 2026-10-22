# Claude Learn Skills

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

**A personal tutor for [Claude Code](https://code.claude.com) that teaches you
from any learning roadmap.**

You bring the roadmap: a folder of Markdown notes, such as an Obsidian vault.
The `tutor` plugin turns it into a course. It knows where you stopped and what
comes next, explains topics or finds you something to read or watch, gives you
practice and reviews your code, quizzes you, and brings old topics back before
you forget them. Your progress lives in a workbook folder of your own; the
roadmap itself is never touched.

```text
  roadmap                      tutor                        workbook
  the textbook      ──reads──▶  the teacher     ──writes──▶  your notebook
  read only; its author        open · teach · practice      plan, results,
  can change it any time       quiz · review · progress     code, journal
```

## Contents

- [What it does](#what-it-does)
- [Requirements](#requirements)
- [Install](#install)
- [Quick start](#quick-start)
- [How a topic goes](#how-a-topic-goes)
- [Commands](#commands)
- [Just talk to it](#just-talk-to-it)
- [Your workbook](#your-workbook)
- [When the roadmap changes](#when-the-roadmap-changes)
- [Tips and questions](#tips-and-questions)
- [Found a problem?](#found-a-problem)
- [Stay up to date](#stay-up-to-date)
- [Uninstall](#uninstall)

## What it does

- **Knows where you are.** Every session starts where the last one stopped.
  `/tutor:open` shows the current topic, what's inside it, and the plan.
- **Teaches your way.** Short explanations, or two to four hand-picked
  resources — the roadmap's own first, then articles, videos and books from the
  web, every link checked — or both.
- **Checks understanding, not just reading.** A question after every concept,
  then a quiz: first with answer options, then open questions. A topic is done
  only when you pass the open round and meet the roadmap's definition of done.
- **Reviews your practice.** Gives you the task, answers "I'm stuck" with hints
  rather than solutions, runs your code, and reviews it like a senior colleague.
- **Brings topics back.** `/tutor:review` asks you about finished topics after
  3, 7, 21, 60 and 150 days, then recaps what you missed — or just reminds you
  of a topic when you ask.
- **Keeps up with the roadmap.** When the author adds or changes topics, the
  tutor updates your plan: a new topic behind you becomes a gap to close, a
  changed topic gets a short catch-up, a renamed one keeps your progress.
- **Never changes the roadmap.** The roadmap is read only, and Claude Code
  itself enforces that in sessions started in your workbook.
- **Plain Markdown.** Your plan, results and notes are files you can read any
  time, open in Obsidian, or keep in git.

## Requirements

- **Claude Code**: in the terminal, the desktop app, or an IDE extension.
- **A roadmap**: a folder of Markdown notes. A git repository works best: the
  tutor reads its history to see what changed. For example,
  [java-roadmap](https://github.com/vnovakovskyi/java-roadmap).
- **Git**, to clone roadmaps and follow their updates.
- **Whatever the practice needs**, such as a JDK for java-roadmap. The tutor
  helps you set it up from the roadmap's setup guide.
- **Optional: web search** in Claude Code, for finding resources beyond the
  roadmap.

## Install

In a Claude Code session:

```text
/plugin marketplace add vnovakovskyi/claude-learn-skills
/plugin install tutor@novakovskyi-learn-skills
```

From a shell, the same two commands start with `claude plugin` instead of
`/plugin`. Once installed, the plugin is also there in the desktop app and the
IDE extensions on the same machine. Type `/tutor:` and the seven commands appear.

The plugin adds skills only: no hooks, no servers, nothing running in the
background.

## Quick start

The example uses java-roadmap; any roadmap works the same way.

### 1. Get a roadmap

```bash
git clone https://github.com/vnovakovskyi/java-roadmap.git
```

### 2. Enroll

Start Claude Code in the roadmap's folder and run:

```text
/tutor:enroll
```

You can also run it from anywhere and name the roadmap:
`/tutor:enroll ~/learning/java-roadmap`, or give a git URL and the tutor clones
it for you.

The tutor then:

1. **Asks where to keep your workbook**, in a dialog: next to the roadmap (the
   default), in the current folder, or any path you type. It never goes inside
   the roadmap or its git repository.
2. **Reads the roadmap**: its modules, topics, order and sections.
3. **Gets to know you**, one question at a time: your goal, what you already
   know, hours per week, the scope (for example, without optional topics), and
   whether it may write in the workbook without asking each time.
4. **Offers a placement check** if you already know part of the course. Topics
   you pass are marked `known`, and you skip them.
5. **Creates the workbook** and shows your plan.

Claude Code may ask once for permission to create the workbook, since it is
outside the folder the session started in.

### 3. Move into your workbook

From now on, start Claude Code in the workbook folder:

```bash
cd java-roadmap-workbook
claude
```

In the desktop app or an IDE, open that folder instead. The workbook's settings
give the session access to the roadmap and protect the roadmap from edits.

### 4. Study

```text
/tutor:open        where am I, and what's in this topic?
/tutor:teach       "teach me"
/tutor:practice    the task; "check" when your work is ready
/tutor:quiz        answer options first, then open questions
/tutor:review      later: bring finished topics back before they fade
```

Next time, run `/tutor:open` or just ask "what's next?" and you continue where
you stopped. It tells you when reviews are due.

## How a topic goes

### 1. Open: see what's ahead

```text
> /tutor:open
Module 4 — Essential Java APIs · topic 8 of 10
Optional and Null-Safety · must · ~3 h · new

Why: NullPointerException is the most common Java runtime error; Optional turns
"it crashed" into "I handled the missing case on purpose".

You'll be able to:
- Explain why null is dangerous and how to minimize it
- Use Optional to represent "maybe a value"
- Apply defensive null habits

Concepts:
  1. null means absence; dereferencing it throws NPE
  2. Creating an Optional: of / ofNullable / empty
  3. Consuming it: orElse, orElseThrow, isPresent / isEmpty
  4. Optional for return types only
  5. Other guards: requireNonNull, empty collections

Practice: findByName over a map, returning Optional
Done when: idiomatic Optional, no null returns, knows when not to use Optional

Plan:
  1. Learn the concepts — I explain, or you read or watch first   /tutor:teach
  2. Practice — findByName with Optional                        /tutor:practice
  3. Quiz — options first, then open questions                  /tutor:quiz

Reviews due: Enums — /tutor:review
Say "teach me" to start, or name a concept to begin with.
```

`open` is the map before the lesson. It catches up with changes in the roadmap,
picks the topic you're on (or the one you name), and shows what's in it and how
you'll go through it. It doesn't start the topic and changes nothing, so use it whenever you want to look around:
`/tutor:open Streams`, `/tutor:open module 5`.

### 2. Teach: learn it your way

Say "teach me" or run `/tutor:teach`. The tutor names what you're about to learn
and asks how you want to learn it:

- **Explain it.** A few sentences and a small example per concept, then a
  question to check it.
- **Something to read or watch.** Two to four options, each with its type,
  length and why it's worth it. You study, come back, and explain each concept
  in your own words; the tutor fills the gaps.
- **Both.** A short explanation first, then resources to go deeper.

```text
For "Optional and Null-Safety":
1. Article · dev.java — Using Optionals · ~10 min · from the roadmap; the official guide
2. Book · Effective Java (3rd ed.), Item 55 "Return optionals judiciously" · one item · when to use it and when not
3. Video · a short walkthrough found on the web · ~12 min · link checked
Which one?
```

You can switch at any moment: "just explain it", "find me a video on this", "is
there an article in Russian?". Start from a specific concept with
`/tutor:teach orElseGet`.

### 3. Practice: do it yourself

- `/tutor:practice` gives you the task, the steps, and a folder for your work,
  such as `practice/m4-optional/`. When the roadmap builds one project across
  topics, you keep working in that project.
- **Stuck?** Say so. Hints come one step at a time: what did you try → which
  concept or API → a concrete hint → the solution only if you ask for it
  outright. Then you type it yourself, and the topic note says "solved with
  help".
- **Ready?** Run `/tutor:practice check` or say "check my code". The tutor runs
  the code, checks it against the task, and reviews it: what works, what to fix,
  one thing to take away. The verdict is **accepted** or **needs changes**.

The tutor never writes your solution into `practice/`.

### 4. Quiz: prove it

1. **Choice round**: 5–8 questions, one at a time, with options you pick in a
   dialog. After each answer you get the verdict, why the right option is
   right, and what misconception a wrong one stands for.
2. **Open round**: 4–6 questions you answer in your own words or code: explain
   it, predict the output, find the bug, what happens if, write a few lines.

The pass mark is 80%. Pass the choice round and the open round follows. Pass the
open round and the tutor goes through the roadmap's definition of done; when
every item is met, the topic is **done**. Below 80%, your weak spots are
recorded, the tutor re-teaches them briefly, and you try again later with new
questions.

### 5. Review: keep it

A finished topic comes back after 3 days, then 7, 21, 60 and 150. Pass a review
and the next one is further away; fail it and the topic comes back in 3 days.
`/tutor:open` tells you when reviews are due, and `/tutor:review` runs them:

- **Questions first**, two or three per topic, aimed at your weak spots, with
  the questions of different topics mixed together. Recalling is what makes it
  stick.
- **Then a recap of what you missed**: the key point and a small example, not
  the whole topic.
- **Just want a reminder?** Say "remind me about Optional" or run
  `/tutor:review Optional recap`: a short summary with your own weak spots,
  then a question or two.

A weak spot you answer well in two reviews in a row disappears from your list.
After a break of two weeks or more, the tutor suggests `/tutor:review recent`
before anything new.

```text
/tutor:review                the topics that are due
/tutor:review Optional       one topic, due or not
/tutor:review module 4       a module's finished topics, mixed
/tutor:review recent         the last three finished topics
/tutor:review weak           only your weak spots
/tutor:review Streams recap  remind me first, then ask
```

## Commands

| Command | Arguments | What it does | Writes |
|---|---|---|---|
| `/tutor:enroll` | `[roadmap folder or git URL] [workbook folder]` | Picks the roadmap and the workbook folder, gets to know you, offers a placement check, creates the workbook with your plan. | The new workbook |
| `/tutor:open` | `[topic \| module]` | Catches up with the roadmap, then shows the current topic (or the one you name) and the plan, or a module with its topics. | Nothing but the sync |
| `/tutor:teach` | `[topic \| concept]` | Says what to learn, explains or offers resources, checks each concept with a question. | Topic note, plan, journal |
| `/tutor:practice` | `[topic] [check]` | Gives the task, hints when you're stuck, reviews your work. | Topic note, plan, journal |
| `/tutor:quiz` | `[choice \| open] [topic \| module]` | A choice or open round on a topic, or a module checkpoint. The open round can finish a topic. | Quiz results, plan |
| `/tutor:review` | `[topic \| module \| recent \| weak] [recap]` | Brings back finished topics: questions first, then a recap of what was missed; or a recap first, on request. | Review results, next review dates |
| `/tutor:progress` | `[module]` | Topics by module, the current topic, gaps, refreshes, reviews due, weak spots, recent activity. | Nothing but the sync |

Every argument is optional. Without one, each command works on where you are
now. A topic or module can be named loosely — "Optional", "module 4",
"Streams" — and the tutor asks if it's ambiguous.

More quiz examples:

```text
/tutor:quiz                  the next round of the current topic
/tutor:quiz open             the open round, now
/tutor:quiz Generics         a quiz on another topic
/tutor:quiz module 4         the module checkpoint, across all its topics
```

## Just talk to it

Every command also starts from plain words, in any language:

| You say | The tutor |
|---|---|
| "Let's continue", "what's next?", "where did I stop?" | opens the current topic |
| "Open module 5", "show me the Streams topic" | opens that module or topic |
| "Teach me", "explain orElseGet", "give me a video on streams" | teaches |
| "Give me a task", "I'm stuck", "check my code" | runs practice |
| "Quiz me", "test me on Optional", "open questions please" | quizzes you |
| "Let's review", "what's due?", "remind me about Optional" | runs a review |
| "How far am I?", "what changed in the course?" | shows your progress |

The tutor talks in the language you write in, and switches when you do. Your
workbook is always kept in English, so it reads the same whatever language you
study in. Technical terms and code stay as the roadmap writes them.

## Your workbook

```text
java-roadmap-workbook/
  workbook.md              the course, you (goal, time, scope), how the roadmap is organized
  plan.md                  every topic in order, with status, result and next review
  journal.md               one entry per day of study; the last "Next:" line is where you stopped
  topics/                  one note per topic: resources, concepts, practice reviews, quizzes, weak spots
  modules/                 one note per finished module
  practice/                your code and other practice work
  CLAUDE.md                tells Claude that this folder is a tutor workbook
  .claude/settings.json    gives access to the roadmap and blocks edits to it
```

Topic statuses in `plan.md`: `todo`, `doing`, `done`, `known` (you knew it
already), `skipped`, `refresh` (the roadmap added something to a topic you'd
finished), `retired` (removed from the roadmap).

It's all plain Markdown:

- Read it any time, or open the folder as an Obsidian vault.
- Change your goal, time or scope in `workbook.md`, or just tell the tutor.
- Keep it in git if you like: the practice code makes a portfolio.

## When the roadmap changes

Before it shows or teaches anything, the tutor compares the roadmap with the
version your plan was built on, and adapts:

| The roadmap… | The tutor… |
|---|---|
| adds a topic ahead of you | puts it in the plan at its place. |
| adds a topic behind you | marks it as a gap and fits it into the next sessions. |
| adds a concept, task or question to a topic you've finished | marks it `refresh` and teaches only the new part, with a short quiz. |
| renames, moves or rewrites a topic | keeps your progress under the new name; asks when unsure. |
| removes a topic | marks it `retired`; your notes stay. |
| fixes wording, typos or links | does nothing. |

Once a week it also checks whether the roadmap has new commits upstream, and
pulls them only if you agree. You can always update it yourself:

```bash
git -C ../java-roadmap pull
```

Don't track progress inside the roadmap itself (statuses or checkboxes in an
Obsidian vault): the tutor doesn't read them, and local edits get in the way of
`git pull`.

## Tips and questions

**Claude Code asks before every write to the workbook.** Allow it at enrollment,
or add `"defaultMode": "acceptEdits"` under `permissions` in the workbook's
`.claude/settings.json`. The roadmap stays protected by the `deny` rule there.

**I study several roadmaps.** Each roadmap gets its own workbook. Start Claude
Code in the workbook of the course you're studying.

**I already know part of the course.** Say so at enrollment: a short placement
check marks what you know as `known`. Later, when a topic feels familiar, tell
the tutor while it teaches: it checks each concept with a question and moves on
quickly.

**I want to skip a topic.** Tell the tutor: the topic is marked `skipped` and
the plan moves on. You can come back to it any time.

**I moved the roadmap.** The tutor asks for the new path and updates the
workbook.

**My roadmap isn't in git.** It still works. The tutor then notices added,
removed and reordered topics, but not edits inside a topic.

**Can I use my own roadmap?** Yes: any folder of Markdown. It works best with
one topic per file, one place that defines the order, and the same sections in
every topic. See
[For roadmap authors](docs/how-it-works.md#for-roadmap-authors).

## Found a problem?

- **A mistake in the roadmap** — a wrong fact, a dead link, an unclear task:
  report it to the roadmap itself, the way its README asks (for java-roadmap,
  an issue or a pull request in
  [its repository](https://github.com/vnovakovskyi/java-roadmap)). The tutor
  can check the point with you, but it never edits the roadmap.
- **A problem with the tutor** — a command that misbehaves, a wrong plan, a
  messed-up workbook: open an issue in
  [claude-learn-skills](https://github.com/vnovakovskyi/claude-learn-skills/issues).
  Say which command, which roadmap, and what you expected.

## Stay up to date

You keep the version you installed until you update it.

**Turn on auto-update (recommended).** Run `/plugin`, open the **Marketplaces**
tab, select `novakovskyi-learn-skills`, and choose **Enable auto-update**.

**Or watch the releases.** On the
[repository page](https://github.com/vnovakovskyi/claude-learn-skills), click
**Watch → Custom → Releases**, and update by hand when a version comes out:

```bash
claude plugin marketplace update novakovskyi-learn-skills
claude plugin update tutor@novakovskyi-learn-skills
```

Then start a new session, or run `/reload-plugins` in an open one.
[CHANGELOG.md](CHANGELOG.md) says what changed in each version.

## Uninstall

```text
/plugin marketplace remove novakovskyi-learn-skills
```

This removes the marketplace and the plugin. Your workbooks and roadmaps are
left untouched.

## More

- [docs/how-it-works.md](docs/how-it-works.md) — the curriculum, the tutor and
  the workbook in detail; how roadmap changes are handled; notes for roadmap
  authors
- [java-roadmap](https://github.com/vnovakovskyi/java-roadmap) — a roadmap from
  zero to a junior Java developer, made to work with the tutor
- [CHANGELOG.md](CHANGELOG.md) — what changed in each version
- [claude-dev-skills](https://github.com/vnovakovskyi/claude-dev-skills) —
  sibling skills for a research → plan → implement workflow

## License

Licensed under the [MIT License](LICENSE): free to use, copy, modify, and
distribute for any purpose, including commercial use. © 2026 Vadym Novakovskyi.
