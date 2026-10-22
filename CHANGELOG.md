# Changelog

All notable changes to the plugins in this repository are recorded here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and
the versions follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html):

- **Major:** a change that breaks a habit — a skill renamed or removed, a
  workbook format that older workbooks don't fit.
- **Minor:** a new skill or a new capability.
- **Patch:** wording, examples, fixes.

To see which version you have installed, run `claude plugin list`.

## [Unreleased]

## [1.0.0] - 2026-10-22

First release.

### Added

- `tutor` plugin:
  - `enroll` — picks the roadmap and a workbook folder outside it (in a dialog
    or from the arguments), gets to know the learner, optionally checks what
    they already know, and creates the workbook with the plan. The workbook's
    settings give access to the roadmap and block edits to it.
  - `open` — syncs with the roadmap and shows the current (or a named) topic or
    module: what it's about, its concepts, the practice, the plan.
  - `teach` — teaches a topic or concept: explains briefly, or offers resources
    from the roadmap and the web, then checks each concept with a question.
  - `practice` — gives the topic's task, helps with hints instead of answers,
    and reviews the learner's work.
  - `quiz` — a round with answer options or a round of open questions on a
    topic, or a module checkpoint; an open round finishes a topic.
  - `review` — brings finished topics back by a spaced-repetition schedule, or
    on request: questions first, mixed across topics, then a recap of what was
    missed; a recap first when the learner just wants a reminder.
  - `progress` — an overview of the course: topics by module, gaps, refreshes,
    reviews due, weak spots.
  - The tutor talks in the language the learner writes in; the workbook is
    always kept in English.

[Unreleased]: https://github.com/vnovakovskyi/claude-learn-skills/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/vnovakovskyi/claude-learn-skills/releases/tag/v1.0.0
