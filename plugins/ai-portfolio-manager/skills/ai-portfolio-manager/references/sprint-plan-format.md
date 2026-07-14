# Sprint Plan Format

Two output formats: markdown (default) and xlsx (for users who live in spreadsheets or already keep a tracker). Same structure either way. A worked example (fictional user) follows the templates.

## Markdown template

```markdown
# SPRINT <N> — AI Implementation Plan, <date range> · mode: <optimization mode>
Window: <weekday date time> → <weekday date time>.
<VERIFIED (usage screen, <when>) | ASSUMED — verify before D1>: <plan name> — <bar 1> <X>% used, <bar 2> <Y>% used, resets <when>.
→ <number> premium cycles this sprint: <what remains in each, and the burn-down instruction>.

## CORE BUDGET RULE
<2–4 sentences: the cycles, what does not carry over, what to burn first and by when,
and the degradation path if a bar runs out or an unverified allowance disappears.>

## TOOL ROUTING
| Resource | Assigned tasks |
|---|---|
| <Premium model + surface> — shortlist | <task IDs, ordered> |
| <Agentic tool + model> | <task IDs> |
| <Default model> — DEFAULT | <task IDs> |
| <Research model> | <task IDs> |
| <Live-data / other> | <task IDs> |
| Human (the true bottleneck — ONE deep block/day) | ★ tasks, one per day |

## DAILY SCHEDULE (human tasks FIRST — only human-gated work slips)
| Day | Human block (do FIRST) | Premium slot | Other AI | Background (runs itself) |
|---|---|---|---|---|
| D1 <date> | ★ <the one must-do> · <small extras> | <shortlist items> | <default-lane items> | <batch jobs> |
| ... | | | | |
| D<n-1> — LIGHT DAY | ★ reading only (no decisions) | ... | | |
| D<n> — DECISION DAY | ★ the decisions, fresh: <list> | — | wrap-up | sprint-end sync |

## SPRINT SCOREBOARD (priority order)
| Project | Tasks | Done | % | Sprint goal |
|---|---|---|---|---|
| <project> | <n> | <n> | <%> | <one line> |
| TOTAL | | | | |

## RULES OF THE SPRINT
1. <Pacing rule — which work classes may carry daily stars, and why.>
2. <Slippage priority: A > B > C; protected lanes.>
3. <Quality gate(s): "X is mandatory before Y", with sign-off recording if relevant.>
4. <Method gates for research tasks: pre-registered hypotheses; validated negatives are recorded wins.>
5. <Unverified-allowance rule: shortlist only, front-loaded; fallback by design.>
6. <Session-end protocol: update statuses + project brief every session; sprint-end: sync tracker, write carry-over list.>
7. <Marker convention: e.g., yellow/⚠ rows = decisions awaiting the human.>
```

Adjust row counts to reality. Ten tasks and two projects get the same skeleton, shorter.

## xlsx layout

- **Dashboard sheet**: the header, budget rule, routing table, daily schedule, scoreboard, and rules — in that vertical order. Scoreboard uses live formulas: `=COUNTIF('<Project>'!I5:I<n>,"Done*")` for Done, `=IF(B<r>>0,C<r>/B<r>,0)` formatted as % for progress.
- **One sheet per project**, columns: `ID | Task | Depends on | Resource (routed to) | Human-gated? | Day | Effort | Notes | Status`. Status values: `Todo / In progress / Done / Done (carry-over) / Blocked / Dropped`. Keep Status in column I so Dashboard formulas stay uniform.
- Highlight (fill color) rows awaiting a human decision; state the convention in the rules.
- On revision, save as a new file or bump a "(rev.N)" in the title — sprint plans get revised mid-week and the history is useful evidence for pacing.

## Worked example (fictional, compressed)

```markdown
# SPRINT 1 — AI Implementation Plan, Mar 2–8
Window: Mon Mar 2, 09:00 → Sun Mar 8, 23:59.
VERIFIED (usage screen, Mon 09:00): Max plan — all-models bar 22% used, premium bar 61% used, both reset Wed 14:00.
→ TWO premium cycles: 39% left until Wed 14:00 (burn on shortlist front), fresh bar Wed→Sun.

## CORE BUDGET RULE
Cycle 1 (now→Wed 14:00): 39% of premium remains and does NOT carry over — spend it on
A-1 (architecture review, 2h) then B-3 (grant-proposal hard sections). Cycle 2 (Wed→Sun):
fresh bar, reserved for C-2 decisions memo + anything bumped from cycle 1. If the premium
bar runs dry early, the default model + checklists take over by design; A-1 conclusions
are already captured in the project brief after each session.

## TOOL ROUTING
| Resource | Assigned tasks |
|---|---|
| Premium (Cowork) — shortlist | A-1 architecture · B-3 proposal hard sections · C-2 memo |
| Default model (Code/Cowork) — DEFAULT | A-2 refactor · B-1/B-2 proposal boilerplate · D-1 report draft |
| Research model | B-4 funder-landscape deep dive |
| Local (overnight) | P-1 photo-archive captioning batch |
| Human (ONE deep block/day) | ★ see daily schedule |

## DAILY SCHEDULE (human tasks FIRST)
| Day | Human block (FIRST) | Premium slot | Other AI | Background |
|---|---|---|---|---|
| D1 Mon | ★ send B-0 partner email (5 min) + review A-1 inputs | A-1 (burn cycle 1) | A-2 refactor | P-1 batch queued |
| D2 Tue | ★ bank + registry errands (human-only) | B-3 hard sections | B-1 boilerplate | P-1 continues |
| D3 Wed | ★ read A-1 + B-3 outputs (30 min, NO decisions) | — (bar resets 14:00) | B-2, D-1 | — |
| D4 Thu | ★ C-1 client call | C-2 memo (fresh bar) | D-1 continues | — |
| D5 Fri | ★ read B-4 research (no decisions) | — | B-4 wrap | P-1 QA sample |
| D6 Sat — LIGHT | ★ nothing scheduled | — | — | — |
| D7 Sun — DECISION DAY | ★ decide: file B or defer · approve A-1 direction | — | carry-over sync | sprint-end protocol |

## SPRINT SCOREBOARD
| Project | Tasks | Done | % | Sprint goal |
|---|---|---|---|---|
| A (product) | 2 | 0 | 0% | Architecture decided |
| B (grant) | 5 | 0 | 0% | Submission-ready draft |
| C (client) | 2 | 0 | 0% | Memo approved |
| D (report) | 1 | 0 | 0% | Full draft |
| P (personal batch) | 1 | 0 | 0% | Archive captioned |

## RULES OF THE SPRINT
1. Calls and errands are low-drain for this user and may share a day with reading;
   writing decisions are high-drain and get Sunday, fresh.
2. Slippage order: B (deadline Mar 12) > C > A > D > P. B is protected.
3. Nothing from the default lane may touch the premium bar without being added to the
   shortlist here first.
4. Session-end: update Status column + project brief. Sunday: carry-over list + tracker sync.
5. ⚠ rows = awaiting the human (currently: none).
```

## Why the format is shaped this way (context for judgment calls)

- **Verification stamp in the header**: the plan's credibility rests on real budget numbers; the stamp forces the check and dates it.
- **Human column first**: the daily table is read left to right; putting the ★ block first makes the priority physical.
- **One ★ per day**: multiple sprints of evidence behind this — human-gated tasks are the only ones that slip, and they slip when stacked.
- **Reading days vs. decision day**: separating digestion from judgment produces better decisions and removes the mid-week pressure to decide half-informed.
- **Rules section**: rules written during planning survive contact with a bad Wednesday; rules improvised on Wednesday don't.
