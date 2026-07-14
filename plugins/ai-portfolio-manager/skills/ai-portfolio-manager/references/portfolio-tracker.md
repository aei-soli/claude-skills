# The Master Tracker (Portfolio Layer)

The master tracker is the permanent asset; sprint plans are disposable weekly documents cut from it. It holds the user's entire portfolio — every project, its status, and its next step — so that no project is ever lost, and every sprint starts from the same source of truth and writes its results back.

Default format: one xlsx workbook. A markdown equivalent (one index file + one file per project) works for users who prefer plain text; same structure.

## Workbook structure

**Sheet 1 — Project Tracker (the master sheet).** One row per project:

| Column | Purpose |
|---|---|
| # | Stable project number — never reused, referenced everywhere ("P07") |
| Project | Short name |
| Category | User's own groupings (Work, Business, Creative, Finance, Ideas…) |
| Type | `Necessary` vs `Want to Do` — makes obligation vs. aspiration visible when priorities collide |
| Priority | High / Medium / Low |
| Status | Not Started / In Progress / On Hold / Completed |
| Complexity | Simple / Medium / Complex — a scoping hint for sprint selection |
| Next Step | ONE concrete next action. The most important cell in the row: a project with an empty Next Step is stalled by definition |
| Target Date | Only for real deadlines; leave blank rather than invent |
| Notes | One line of context |

**Sheet 2 — Legend & Guide.** Status/priority definitions and the tracker's own usage rules, so the workbook is self-explaining months later.

**One Roadmap sheet per active project.** Task-level detail, one row per task:

| Column | Purpose |
|---|---|
| # / Task | Stable task ID and name |
| Category/Phase | Grouping within the project (e.g., "Core build", "Session 3") |
| Status | Todo / In Progress / Completed / Blocked / Dropped |
| Priority | Within-project priority |
| Dependencies | Which task(s) must precede this one |
| Description / Next Step | What done looks like |
| **AI Prompt** | A ready-to-paste prompt that lets any AI session execute this task cold — context, inputs, expected output. Writing it at planning time (when context is fresh) is cheap; reconstructing it months later is expensive. This column is what makes the tracker executable rather than just descriptive |

Not every project needs a roadmap sheet — the test is: *would a single fresh Next Step cell always suffice to know what to do next?* If yes (a tax filing, a habit, a background batch job), the project lives entirely in its master-sheet row, no matter how important it is. Roadmap sheets are for genuine multi-task structure with dependencies (a build, a renovation, a publication). Over-building sheets for simple projects is a real failure mode: it creates maintenance burden that kills the tracker within a month. When in doubt, start without the sheet — one can be added the day the Next Step cell stops being enough.

## Bootstrapping (first use)

Most users don't arrive with a tracker; build it from a brain-dump interview:

1. "List everything you're working on or wanting to work on — projects, obligations, ideas. Don't filter." Aim for exhaustive; the tracker's value is that *nothing lives only in the user's head*.
2. For each item, fill the master row in one pass: category, necessary-vs-want, priority, status, and — non-negotiable — a concrete Next Step.
3. Create roadmap sheets only for the 3–6 projects with real multi-task structure.
4. Read the totals back honestly: "You have 24 projects, 9 high-priority. At your sprint capacity that's N weeks of high-priority backlog" — the tracker's first gift is usually an honest overload picture.

## Sync protocol (tracker ↔ sprint)

- **Sprint setup pulls from the tracker**: candidate tasks come from roadmap sheets and Next Step cells of in-progress + high-priority projects, filtered by the sprint's optimization mode and deadlines. The sprint plan references tracker IDs, so the two documents never drift apart silently.
- **Sprint-end writes back**: statuses updated, completed tasks marked, every touched project gets a fresh Next Step, and the carry-over list becomes next sprint's candidate pool. The sprint report's "sync with master tracker" step is this.
- **Version safety**: before any bulk update, save a dated backup copy (`Tracker.xlsx.bak-<tag>-<date>`). Trackers accumulate months of judgment; treat them like a database, not a scratch file.

## The correlated file system (tracker + memory files + index)

The tracker holds status; it cannot hold depth. The full system has four correlated elements per substantial project:

1. **Tracker row** (always) — status, priority, Next Step.
2. **Roadmap sheet** (complex projects) — task-level detail with the AI Prompt column.
3. **Memory file** — one markdown file per project (`memory/projects/NN_short_name.md`) containing objective, key deliverables, work plan, resources, a dated **session log**, and next steps. The session log is the memory: it's what lets any future session (human or AI) pick a project up cold after three months.
4. **Project folder** (projects that produce artifacts) — a working directory named with the project number first (`Projects/<NN> <Project Name>/`, e.g. `Projects/27 LPR parking/`) so folders sort in tracker order and the ID is visible at a glance. The memory file records its path under a "Project Folder" heading, and the folder is where session outputs get copied *before the session ends* — anything left in a temporary session workspace is lost. When a session produces a deliverable, saving it into the project folder is part of finishing the task, not an optional extra.

Create memory files and folders for projects with continuity needs; one-line projects need only their tracker row. Optionally, a markdown **INDEX** mirrors the tracker's project list for fast reading at session start (useful when sessions begin by reading text files rather than opening xlsx).

Correlation rules — these prevent the drift that quietly kills multi-file systems:

1. **Declare exactly one source of truth** for project numbers, priorities, and statuses (normally the tracker), and write that declaration into every mirror ("synced from <file> on <date>; if they disagree, the tracker wins").
2. **New project numbers are allocated only from the source of truth**, never from a mirror or from memory — a stale mirror is how two projects end up sharing a number.
3. **Project IDs are permanent.** If numbering ever drifts between tracker and filenames (it happens), do not mass-rename files; add a mapping column ("Memory file") to the index instead.
4. **Sync at sprint-end**, as part of the existing protocol: statuses tracker→mirrors, new session-log entries into the touched projects' memory files, and a quick cross-check that every In Progress project still has both a Next Step and (if it needs one) a memory file.
5. **Backup before bulk edits**: dated `.bak-<tag>-<date>` copies of any file about to be rewritten.

When adopting an *existing* user system, first map what they have (tracker? index? project files? which is freshest?), confirm the source of truth with them, and reconcile mirrors to it before adding anything new.

## Portfolio review (monthly or quarterly — separate from sprints)

A sprint asks "what this week?"; the portfolio review asks "still the right projects?":

- Stale scan: In Progress projects untouched for 2+ sprints → downgrade to On Hold honestly, or give them a sprint slot.
- Priority check: do High ratings still reflect reality? (Deadlines passed, interests moved, clients changed.)
- Completion pass: mark finished projects Completed — visible wins matter for morale.
- Intake: new ideas captured since last review get rows, even at Low priority; capture is cheap, forgetting is not.
- Resource echo: if the settings review (sprint reports) shows chronic over/under-use of a subscription or lane, this is where the portfolio-level fix happens.

In agentic environments, offer the portfolio review as a scheduled monthly task.
