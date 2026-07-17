---
name: ai-portfolio-manager
description: Manage a whole portfolio of projects with AI — a persistent master tracker of all projects, plus weekly sprint plans that allocate tasks across AI models, subscriptions, agents, and the human, with model routing, usage-budget management, mid-sprint check-ins, remediation, and end-of-sprint cost/usage reports. Use this skill whenever the user wants to organize or track multiple projects, set up a project tracker or task list, plan their week or sprint with AI assistance, asks which AI model or tool to use for which task, mentions hitting usage limits or wanting more from their AI subscriptions, or reports being behind on a plan and needing to re-plan. Trigger even without the words "sprint" or "tracker" — phrases like "help me get all my projects under control", "I have too many things going on", "plan my AI work", "I keep running out of Claude usage", "which model should do what", or "I'm behind, what do I drop" all qualify.
---

# AI Portfolio Manager

*Implements the **SprintFleet Method** by Adrian Ionescu (AEI Digital Solutions).*

Manage a portfolio of projects with AI, in two layers. The **portfolio layer** is a persistent master tracker — every project, its status, its next step — that survives across months. The **sprint layer** cuts a one-week plan from it, allocating every task to the right resource — a specific AI model, an agent, a background job, or the human — under real usage-budget constraints; then monitors the sprint, fixes it when it drifts, reports where the resources went, and syncs results back to the tracker.

The central insight this skill encodes: **the scarcest resource is the human's attention, not compute or tokens.** Most AI-assisted work plans fail not because the AI ran out of capacity, but because every AI workstream has a human-gated step (a decision, a review, a login, a send button) and those steps pile up unscheduled. A good sprint plan schedules the human first and the models around them.

## Lifecycle overview

0. **Settings** (first time, adjustable per sprint) → optimization mode + constraints
1. **Resource assessment** (first time, then quarterly) → produces a routing rule
2. **Master tracker** (first time, then continuously) → the portfolio source of truth
3. **Sprint setup** (weekly, pulls from the tracker) → verify budgets, select tasks, route, schedule
4. **The plan document** → budget rule, routing table, daily schedule, scoreboard, sprint rules
5. **Monitor → remediate** (daily/mid-sprint check-ins) → keep the plan true
6. **Sprint-end report** → outcomes, cost/usage per lane, carry-over, sync back to tracker
7. **Portfolio review** (monthly/quarterly) → are these still the right projects?

If the user only wants part of this (e.g., just "which model for which task", just the tracker, or just a mid-sprint rescue), do that part well and offer the rest.

## The master tracker (portfolio layer)

Before the first sprint, establish the source of truth. If the user has a tracker or task list, adopt it; if not, build one from a brain-dump interview — the goal is that *no project lives only in the user's head*. One xlsx workbook (or markdown equivalent): a master sheet with one row per project (number, category, necessary-vs-want, priority, status, complexity, confidentiality tier, target date, and a concrete **Next Step** — the most important cell, since a project without one is stalled by definition), plus a Roadmap sheet per complex project whose task rows carry an **AI Prompt column**: a ready-to-paste prompt that lets any future session execute the task cold. Sprints pull candidate tasks from the tracker and write statuses back at sprint-end, so the two layers never drift apart. The tracker is accompanied by a correlated file system it must keep in sync: one markdown **memory file** per substantial project (objective, work plan, dated session log — the project's long-term memory) and optionally an index that mirrors the tracker; exactly one file is declared source of truth, new project numbers come only from it, and mirrors are re-synced at sprint-end. Read `references/portfolio-tracker.md` for the full structure, bootstrap interview, sync and correlation rules, and the monthly portfolio-review checklist.

## Step 0 — Settings and optimization mode

Ask (or infer from context and confirm) what this sprint optimizes for. The mode changes routing behavior throughout; state the active mode in the plan header.

| Mode | When | Routing behavior |
|---|---|---|
| **Quality-first** | Deadlines with reputation attached: submissions, filings, client deliverables | When in doubt, route UP. Premium shortlist expands; budget is a constraint to watch, not to economize |
| **Balanced** (default) | Normal weeks | Route by task class; when in doubt, route down |
| **Budget-first** | User keeps hitting limits, or between critical pushes | Route DOWN aggressively. Premium by exception with one-line justification; maximize the background/batch lane; default model is the workhorse |
| **Deadline-first** | A date is at risk | Optimize elapsed time: parallelize agent runs, stars go to whatever unblocks the critical path, non-deadline lanes frozen for the sprint |

Also record as settings: subscriptions and reset cycles, human focus blocks per day, protected lanes (deadlines that outrank everything), and reporting preference (markdown vs xlsx).

**Confidentiality tiers.** Every project (or individual task) carries one of three tiers, recorded in the tracker and honored by routing:

- **Open** — any lane.
- **Restricted** — approved cloud vendors only; no third-party plugins, marketplaces, or experimental tools.
- **Strict** — the data never leaves machines the user controls. Routing locks to the local/offline lane (e.g., local open models on the user's own hardware, reachable over their private network), *overriding the optimization mode*. If no local lane exists, Strict tasks route to the human, and the plan says so honestly rather than quietly downgrading the tier.

The override direction matters: modes optimize preferences, tiers enforce boundaries. A boundary always beats a preference.

A note on what is being optimized: the token cost of *writing* a plan is trivial; the plan exists to allocate the expensive things — premium-model budget and human hours — across the week. Mode choices should be explained to the user in those terms.

## Step 1 — Resource assessment

Inventory what the user actually has — and only what *this* user actually has: resources they state, or that their own files show. Never assume hardware, subscriptions, or tools from examples or from other contexts. Ask, or read from their files/memory if available:

- **Subscriptions**: which AI plans (Claude Pro/Max, ChatGPT, Gemini, Grok, API credits…), what usage limits, and — critically — **when the limits reset**.
- **Surfaces**: chat apps, agentic tools (Claude Code, Cowork), IDEs, schedulers. The same model behaves differently in different harnesses.
- **Local compute**: any machines that can run models or scripts unattended (overnight batch is a free lane).
- **Human constraints**: hours per day of real focus, energy patterns, fixed commitments, deadlines.

Then be honest about capability tiers. Do not flatter the user's hardware or subscriptions: a local 70B model is a privacy-and-batch tier, not a frontier replacement; a cheaper model tier is fine for routine drafting but not for the hardest reasoning. Overclaiming here produces plans that quietly fail.

The output of this step is a **routing rule** — one sentence per resource, by task class, so the user stops deciding per task. Example shape:

> Strict-tier data or bulk/overnight → local lane. Hard reasoning, coding, high-stakes writing → strongest model, budgeted. Routine drafting, formatting, medium tasks → default mid-tier model. Long-context research / multimodal → research-tier model. Real-time market or social signal → live-data model.

A good routing rule covers ~90% of the portfolio. Read `references/resource-assessment.md` for the full assessment method, capability-tier guidance, and subscription-rationalization advice.

## Step 2 — Sprint setup (weekly)

### Verify the budget — never assume it

Ask the user to check their actual usage screen and report: percent used per limit, and the exact reset time. Plans built on assumed budgets fail on day one. Label the numbers honestly in the plan header: `VERIFIED (usage screen, <when>)` only when the user actually checked; otherwise `ASSUMED — verify before D1`. Never upgrade an assumption to "verified" because the user supplied a number hypothetically. Two facts matter enormously:

- **Unused budget usually does not carry over.** If 47% of a premium-model allowance remains and it resets tomorrow at 15:00, that 47% is a *burn-down*: schedule the premium shortlist into it today, deliberately.
- **Reset boundaries create cycles.** A week may contain two or three budget cycles for a scarce model. Plan each cycle's shortlist separately; front-load the most important premium work into the earliest cycle (a cycle in hand is worth two in the schedule).

### Inventory and classify the tasks

Pull candidate tasks from the master tracker: roadmap-sheet tasks and Next Step cells of in-progress and high-priority projects, filtered by mode and deadlines (plus last sprint's carry-over list). If no tracker exists yet, build it first — even a minimal one — rather than planning from an ad-hoc list. Give each task a short stable ID (e.g., `RX-1`, `Q2`) so plans, sessions, and carry-over lists can refer to them unambiguously. Classify each task on two axes:

1. **Which resource class it needs** — premium model / default model / research model / background-batch / human-only. When in doubt, route down, not up: premium budget spent on a routine task is budget stolen from the task that needed it.
2. **Human involvement** — fully delegable, human-gated (needs a decision/review/send), or human-only (logins, phone calls, physical tasks).

### Schedule the human first

The evidence from real sprints: **only human-gated work slips.** So invert the usual order:

- **One starred must-do per day.** Each day gets exactly one ★ task — the single thing that must happen. More than one star per day is a plan for slippage.
- **Energy-aware pacing.** Ask the user which work classes drain them and which don't, and don't let high-drain classes (deep analysis, big decisions) carry consecutive daily stars. Mechanical work (paste-work, builds, phone-camera inventory) can fill low-energy days.
- **Separate digestion from decision.** Schedule *reading days* (absorb AI outputs, explicitly "no decisions") during the week and a *decision day* near the end, when everything has been digested. Decisions made fresh on accumulated material beat decisions forced mid-stream. Mark every pending decision in the plan with the scannable convention `[? — Awaiting Human Decision: Option A vs Option B]` so check-ins can find bottleneck decisions mechanically instead of by re-reading the plan.

### Fill the model lanes

- **Premium shortlist**: an explicit, ordered list of tasks allowed to touch the scarcest model, front-loaded into budget cycles. Everything else is banned from it by default.
- **Default lane**: the mid-tier model handles the routine bulk. This lane is rarely budget-constrained; it's ordered by project priority.
- **Background lane**: batch jobs, scheduled tasks, overnight local runs — work that "runs itself". Every task moved here is pure gain.
- **Degradation path**: state what happens if a premium budget disappears (extension not granted, limit hit early). E.g., "if the premium bar runs out, the checklists + the default model take over by design." A plan without a degradation path is a bet, not a plan.

### Decide the slippage order in advance

Write an explicit priority order for when (not if) things slip: "A > B > C; the deadline lane is protected." Deciding this while calm prevents deciding it while behind.

## Step 3 — The plan document

Default output: a markdown plan. If the user prefers a spreadsheet (or already keeps one), produce an xlsx with a Dashboard sheet plus one sheet per project — read `references/sprint-plan-format.md` for both templates and a worked example.

Every plan contains, in order:

1. **Header** — sprint window (start → end, with times), active optimization mode, budget stamp (VERIFIED or ASSUMED, with source and time).
2. **Core budget rule** — the cycles, what remains in each, what burns when.
3. **Tool routing table** — one row per resource, listing its assigned task IDs.
4. **Daily schedule** — one row per day: ★ human block FIRST, then premium-model slot, then other AI lanes, then background. Mark light days and the decision day.
5. **Sprint scoreboard** — one row per project: task count, done count, % done, one-line sprint goal. Use live formulas if xlsx.
6. **Rules of the sprint** — numbered, explicit: pacing rules, slippage priority, quality gates ("X is mandatory before Y ships"), degradation path, session-end protocol, the `[? — Awaiting Human Decision: …]` marker convention, and a **session-hygiene rule**: work in one session per task or task-cluster rather than one endless thread — long threads re-send their whole history every turn, quietly taxing both budget and quality (15–20 substantial turns or many large file reads is a reasonable warning sign, not a law). The session-end protocol is what makes starting fresh cheap: statuses and briefs are already written down, so no context is lost by forking.

Keep the plan honest about scale: a sprint with 60+ tasks and 7 projects is normal for a heavy user; a first-time user might have 10 tasks and 2 projects. Same structure, smaller table.

## Step 4 — Monitor and remediate (during the sprint)

The plan is a living document. Offer the user a **check-in** cadence — a 2-minute daily version and a fuller mid-sprint review. In agentic environments with scheduled tasks, offer to automate the daily check-in. The check-in compares actuals to plan on three gauges:

1. **Budget burn** — usage-screen percentages vs. where the plan expected them to be at this point in the cycle.
2. **Task status** — scoreboard counts per lane; which lane is ahead/behind.
3. **Human blocks** — did the ★ tasks happen? (This is the leading indicator: AI lanes rarely slip on their own.)

When something is off, remediate from the pre-agreed machinery — never improvise priorities mid-week. Read `references/monitoring-and-reporting.md` for the check-in script, the remediation playbook (symptom → action), and the plan-revision convention (bump "rev.N", keep history).

## Step 5 — Sprint-end report

On the last day, produce a short report: sprint goals hit/missed per project, resource usage per lane (percent-of-bar consumed per model, human blocks used vs. planned, batch jobs completed), the carry-over list, and pacing evidence for next sprint (which lanes ran dry, which stars slipped and why). Template in `references/monitoring-and-reporting.md`.

Be straight about measurement limits: subscription vendors expose percent-of-bar, not token counts — report what the usage screen shows and label estimates as estimates. API usage, where present, can be reported in real currency from the provider console.

## Protocols (continuity)

- **Session-start time check**: verify the actual date and time from the environment (shell `date` or platform clock) at the start of every session and before every check-in — never infer it from conversation flow, because sessions pause and resume across days invisibly, and budget cycles, daily stars, and logs are all date-anchored. No clock available → ask the user.
- **Session-end protocol**: at the end of every working session, update the task statuses in the plan and the project's brief/notes, with a verified date stamp. This is what makes next session start warm instead of cold.
- **Sprint-end protocol** (last day): the report above, plus sync with the user's master tracker/task list.
- **Recurring reviews**: anything that repeats (monthly reviews, quarterly checks) should become a scheduled task, not a sprint line item.

## Tone and honesty requirements

- Never present an unverified budget as fact; label assumptions.
- Record *which* model did safety- or quality-critical work when the user's rules require sign-off.
- Validated negatives are wins — record them (especially for research/analysis tasks), don't hide them.
- If the user's portfolio is overloaded, say so with numbers (tasks × realistic human blocks per week) rather than compressing the schedule into fiction.
