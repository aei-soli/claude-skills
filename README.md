# aei-skills

Practitioner-built Claude skills by [Adrian Ionescu](https://github.com/aei-soli) · AEI Digital Solutions

## AI Portfolio Manager — the SprintFleet Method

**Run your whole portfolio of projects across multiple AI models — without drowning, and without hitting the mid-week usage wall.**

Most people juggling many projects with AI hit the same three failures: the premium model budget dies mid-week, human-gated steps pile up unscheduled until everything slips, and projects quietly stall because nothing remembers their next step. The **SprintFleet Method** — developed and battle-tested by a consultant running 30+ concurrent projects across five AI subscriptions and a local model cluster — fixes all three with two correlated layers:

**The portfolio layer** is a persistent master tracker: every project, its priority, its status, and one concrete Next Step — plus roadmap sheets for complex projects whose tasks each carry a *ready-to-paste AI prompt*, so any future session can execute them cold.

**The sprint layer** cuts a one-week plan from the tracker: every task routed to the right resource (premium model, workhorse model, research model, overnight batch, or you), scheduled around verified usage-budget cycles (burn-down before resets — unused premium budget doesn't carry over), with exactly one starred human must-do per day, because the evidence is consistent: *only human-gated work slips*. Then daily 2-minute check-ins, a remediation playbook for when the week goes sideways, and a sprint-end report showing where your subscriptions and hours actually went — synced back to the tracker.

Four optimization modes: **quality-first**, **balanced**, **budget-first** (stop hitting limits), **deadline-first**.

In benchmark tests against unaided Claude on identical planning tasks, plans produced with the skill satisfied 21/21 quality assertions vs 16/21 without — the misses being exactly the expensive ones (budget burn-down scheduled backwards, no degradation path, no slippage order).

### Install

**Claude desktop / Cowork:** Settings → Capabilities → plugins → add marketplace `aei-soli/claude-skills`, install *ai-portfolio-manager*.

**Claude Code:**
```
/plugin marketplace add aei-soli/claude-skills
/plugin install ai-portfolio-manager@aei-skills
```

Then just talk: *"Help me get all my projects under control"* · *"Plan my week"* · *"I keep running out of Claude usage"* · *"I'm behind, what do I drop?"*

### What's coming

The free skill is complete and maintained. A paid edition is planned: deterministic tracker-building scripts, polished template workbook, scheduled-task automation pack, live dashboard, quarterly-maintained vendor tier maps, and a team edition. Watch this repo.

### License

MIT — see [LICENSE](LICENSE). *SprintFleet* is a method name by Adrian Ionescu; attribution appreciated.
