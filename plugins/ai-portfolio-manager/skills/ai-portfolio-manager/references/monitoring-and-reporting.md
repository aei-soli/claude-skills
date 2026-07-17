# Monitoring, Remediation, and Reporting

How to run a sprint after the plan is written: the check-in script, the remediation playbook, and the sprint-end report.

## Step zero of every check-in: establish the actual date and time

Never assume what day or time it is from conversation flow — sessions pause and resume across hours or days without any visible seam, and every gauge in this system is time-anchored (budget cycles have reset timestamps; "which day's ★ is due" depends on the date; session logs are worthless if misdated). Before any check-in, monitoring action, or log entry: get the real clock from the environment (run a shell `date`, or use the platform's current-date context). If the environment offers no clock, ask the user for the date and time — one short question beats a silently wrong sprint day. Timestamp every session-log entry and plan revision with the verified date, and when a session resumes after a gap, recompute where the sprint actually stands (elapsed days, passed resets, missed stars) before saying anything about progress.

## Daily check-in (2 minutes)

Ask the user for exactly three things — nothing that requires opening more than one screen:

1. Usage-screen percentages (per bar) — or "didn't check" (acceptable on non-premium days).
2. Did yesterday's ★ happen? (yes / partly / no + one word why)
3. Anything new landed? (new tasks, moved deadlines)

Then answer in four lines or fewer: on/off track per gauge, today's ★ restated, and any single adjustment. Also scan the plan for `[? — Awaiting Human Decision: …]` markers and surface any that block other lanes — an unmade decision is the most expensive kind of slippage because it stalls AI lanes that would otherwise finish on their own. A daily check-in that produces a wall of text will be abandoned by day three — brevity is the feature. In environments with scheduled tasks, offer to schedule this each morning.

## Mid-sprint review (10 minutes, around the midpoint or after a budget reset)

Compare actuals to plan on the three gauges:

| Gauge | On track looks like | Off track looks like |
|---|---|---|
| Budget burn | Burn roughly proportional to cycle elapsed; premium spent on shortlist items only | Premium bar ahead of schedule, or spent on non-shortlist work; a cycle's burn-down missed before reset |
| Task status | Scoreboard % roughly even across protected lanes | A protected lane at 0%; the default lane consuming attention meant for the deadline lane |
| Human blocks | Stars happening, even if partly | Two or more consecutive stars missed — the leading indicator of sprint failure |

If two or more gauges are off, revise the plan (bump the title to "rev.N", keep the old daily rows struck through or archived — the history is next sprint's pacing evidence).

## Remediation playbook (symptom → action)

Apply the pre-agreed machinery in this order; improvised mid-week priority changes are how sprints die.

- **Premium budget burning too fast** → freeze the premium lane to shortlist-only (re-read the shortlist aloud), push borderline tasks to the default model, and re-check at the next reset. If quality-first mode: instead pull forward the highest-value premium work before the bar empties, and accept the freeze afterwards.
- **Premium budget gone / extension not granted** → execute the degradation path exactly as written in the plan. If none was written, that's the lesson for next sprint; for now, default model + tighter human review on the affected tasks.
- **Human stars slipping** → do not reschedule all missed stars forward (that stacks them, which is what causes slippage). Apply the slippage order: the lowest-priority starred items are dropped or delegated, not deferred. Check the energy pattern — if high-drain stars are stacked on consecutive days, re-space them.
- **A protected deadline at risk** → switch to deadline-first behavior for that lane only: its next human-gated step gets tomorrow's star; its AI tasks may jump the premium shortlist queue; other lanes freeze until the risk clears.
- **New work landed mid-sprint** → it enters the carry-over list for next sprint by default. It may displace a current task only by taking its slot explicitly (name what leaves).
- **A lane finished early** → pull from the carry-over/stretch list in slippage-priority order; don't invent new work.
- **AI output quality below the gate** (e.g., a sign-off criterion failing) → escalate that one task up a tier (this is what the premium reserve is for, in every mode), and record which model produced the passing version.

## Sprint-end report template

```markdown
# SPRINT <N> REPORT — <date range> · mode: <mode>

## Outcomes
| Project | Goal | Result (hit / partial / missed) | One-line note |
|---|---|---|---|

## Resource usage
| Lane | Planned | Actual | Note |
|---|---|---|---|
| Premium model | <shortlist, N tasks, ~X% of bar per cycle> | <tasks done, % consumed per usage screen> | <e.g., "cycle 1 burn-down executed; cycle 2 ended at 60%"> |
| Default model | <N tasks> | <N done> | |
| Research / other models | | | |
| Background/batch | <jobs> | <completed?> | |
| Human blocks | <7 planned, 7 stars> | <5 happened> | <which slipped and why — this is pacing evidence> |
| API spend (if any) | <est.> | <from provider console, real currency> | |

## Carry-over list
- <task ID — one-line status — proposed lane next sprint>

## Pacing evidence (feeds next sprint)
- <e.g., "high-drain stars failed on consecutive days again — space them">
- <e.g., "research-model lane was empty by Wednesday — it can take more">

## Settings review
Mode for next sprint: <keep / change + why>. Subscription notes: <bars chronically unused or chronically dry — candidates for downgrade/upgrade>.
```

Percent-of-bar is the honest unit for subscription usage — vendors don't expose tokens per task. Convert to money only for API usage (console numbers) or when annualizing a subscription decision ("this bar sat 70% unused two sprints running" is grounds to discuss a downgrade in the settings review).

## Automation notes (agentic environments)

- Offer the daily check-in as a scheduled task (each morning, sprint days only).
- The scoreboard in an xlsx plan updates itself via formulas when Status cells change; in markdown, update counts at session-end per the protocol.
- If the environment can read the user's task tracker or project files, pre-fill gauge 2 (task status) and only ask the human for gauges 1 and 3.
