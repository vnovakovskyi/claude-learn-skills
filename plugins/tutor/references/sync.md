# Syncing with the curriculum

The curriculum can change at any time: its author fixes topics, adds new ones,
reorders modules. Sync before choosing what to study, so the plan matches the
curriculum as it is now. The curriculum is never changed by syncing.

## 1. Find what changed

Read `curriculum` and `revision` from `workbook.md`. If the curriculum folder is
missing, tell the learner and stop.

**Git curriculum.**

```bash
git -C <curriculum> rev-parse HEAD
```

- Same as `revision`: nothing changed. Go to step 2.
- Different: list what changed since `revision`.

  ```bash
  git -C <curriculum> log --oneline <revision>..HEAD
  git -C <curriculum> diff --name-status -M <revision> HEAD
  ```

  If `revision` is no longer in the history (the history was rewritten), compare
  the whole curriculum with the plan as for a non-git curriculum, and treat every
  `done` topic as possibly modified.

Only committed changes count. Uncommitted edits in the curriculum are picked up
once they are committed.

**Not a git repository** (`revision: none`). Re-read the curriculum's order (see
the curriculum map) and compare its topics with the plan. This finds added,
removed and reordered topics, but not edits inside a topic.

## 2. Check upstream (git, at most once a week)

If the curriculum has an upstream branch and `upstream-checked` is more than 7
days old:

```bash
git -C <curriculum> fetch --quiet
git -C <curriculum> rev-list --count HEAD..@{u}
```

If the count is above zero, tell the learner that the curriculum has new commits
upstream and offer to update it. Only on a yes:

```bash
git -C <curriculum> pull --ff-only
```

Then go back to step 1. If the pull fails (local changes, diverged history), say
so and leave it to the learner.

After a successful fetch, set `upstream-checked` to today. A fetch that fails
(offline, no access) is not an error: skip it without fuss and try again next
session.

## 3. Apply the changes to the plan

If topics were added, removed, renamed or reordered, or the file that defines
the order changed, re-read the order and rebuild the plan's order. Statuses
travel with their topics.

| Change in the curriculum | Change in the workbook |
|---|---|
| Topic added | Add it as `todo` at its place, Result `added <date>`. Before the frontier, it is a gap: tell the learner. |
| Topic removed | First check whether it was replaced (below). If not: status `retired`; move the row to `## Retired`. Keep its note. |
| Topic renamed or moved (`R` in the diff, or a replacement) | Change the key in the plan; rename `topics/<old>.md` to `topics/<new>.md`; update its `topic` and `source`, and set `previous-source` to the old path. Then judge it as a modified topic. |
| Topic modified | Read `git -C <curriculum> diff <revision> HEAD -- <file>` and judge it (below). |
| Order changed | Reorder the rows; statuses stay. |
| Modules added, renamed or split | The same rules, topic by topic; update the module sections. |
| Structure changed (new section names, new metadata, the order moved to another file) | Update the curriculum map in `workbook.md`. |

**Replaced topics.** Git reports a rename only when the file kept most of its
content. A rewritten topic shows up as one file deleted and another added, even
with a near-identical name. For every deleted topic the learner has progress in
(`doing`, `done`, `known`, `refresh`), look among the added topics for its
successor: a similar name, overlapping objectives.

- One clear successor: treat it as a rename (table above).
- Split into several topics, or merged into another one: the successors that
  cover what the learner studied become `refresh`, with Result
  `was <old key>`. If the learner's progress was `doing`, the successor becomes
  `doing`. The old topic is `retired`.
- Not sure: don't guess. Show the learner the candidates and ask.

To see what changed in a renamed or replaced topic, compare the old file with
the new one:

```bash
git -C <curriculum> diff "<revision>:<previous-source>" "HEAD:<source>"
```

**Judging a modified topic.** A change is **material** when it adds or changes
what the learner must know or do: an objective, a concept, a practice step, a
self-check question, a definition-of-done item. Wording, typos, links and
formatting are not material.

| Status | Material change | Not material |
|---|---|---|
| `done`, `known` | Status `refresh`; Result says what's new, in a few words. | Nothing. |
| `refresh` | Add the new part to the Result. | Nothing. |
| `doing` | Note it in the topic note; teach the new part in this topic. | Nothing. |
| `todo`, `skipped`, `retired` | Nothing. | Nothing. |

## 4. Record

- Set `revision` in `workbook.md` to the new commit, and update the "Synced to"
  line in `plan.md`.
- In the session's journal entry, write a `Curriculum:` line, for example
  `a624939 → b12c3d4: +2 topics (1 gap), 1 refresh, 1 renamed.` or
  `no changes.`

## 5. Tell the learner

Only when something changed, in two to five lines: what changed and what it
means for them.

```text
The curriculum was updated (3 commits):
- New topic "Recursion" in Module 2. You're past it, so I'll fit it into the next sessions.
- "Generics Basics" got a part on wildcards. A short catch-up when it fits.
- "JUnit 5 Basics" is now "JUnit Basics". Only renamed; your progress is kept.
```
