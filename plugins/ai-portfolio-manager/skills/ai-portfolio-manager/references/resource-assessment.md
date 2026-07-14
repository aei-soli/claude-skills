# Resource Assessment Method

How to build an honest inventory of a user's AI resources and derive a routing rule. Run this the first time the skill is used with a user, and refresh quarterly or when subscriptions change.

## 1. Inventory questions

Ask only what you can't read from the user's files. Group the questions so it feels like one short interview, not a form.

**Subscriptions and models**
- Which paid AI plans? (Claude Pro/Max, ChatGPT Plus/Pro, Gemini, Grok/SuperGrok, Copilot, API credits…)
- For each: what are the usage limits, and when do they reset? If the user doesn't know, ask them to open the usage/limits screen and read it out — this number anchors the whole plan.
- Any model tiers inside a plan (e.g., a premium model with a separate, smaller bar than the default models)?

**Surfaces / harnesses**
- Chat apps, agentic desktop tools (Cowork), CLI agents (Claude Code), IDE integrations, schedulers/automations.
- The same model in an agentic harness can do multi-hour autonomous work; in a chat window it can't. Note which surfaces the user actually uses.

**Local compute**
- Any machines capable of running local models (Apple Silicon unified memory, GPUs)? Pooled clusters (e.g., Exo)?
- Any always-on machine (mini PC, old laptop, NAS) usable as a scheduler/ops node?
- Old hardware: usually worth more as dedicated single-purpose nodes (build machine, ops box, edge testbed) than as cluster members — heterogeneous slow nodes drag a distributed-inference pipeline down to the slowest link.

**Human constraints**
- Realistic deep-focus blocks per day (most people: one).
- Energy pattern: which kinds of work are draining vs. mechanical for *this* user (it varies — some find coding restful and writing draining, some the reverse).
- Hard deadlines and fixed commitments in the planning window.

## 2. Honest capability tiers

Assign each resource a tier based on what it genuinely does well. The common self-deceptions to correct, gently and with numbers where possible:

| Resource | Honest tier | Typical trap |
|---|---|---|
| Frontier cloud model (premium bar) | Hard reasoning, architecture, high-stakes writing, judgment calls | Being burned on routine tasks out of habit |
| Default/mid cloud model | The workhorse: drafting, formatting, standard coding, summaries | Under-used because user reflexively picks premium |
| Long-context/research model | Deep research passes, huge-document digestion, multimodal | Used as a general chat model, wasting its niche |
| Live-data model | Real-time market/social/news signal | Trusted for reasoning tasks it's weaker at |
| Local 70B-class (pooled/cluster) | Privacy tier + overnight batch (captioning, OCR, embeddings, RAG). ~6–12 tok/s: fine unattended, painful interactive | Treated as a frontier replacement; it is not |
| Local small model (7–12B, always loaded) | Instant local utility tasks, drafts on private data | Expected to do reasoning |
| Always-on box | Schedulers, scrapers, monitors, recurring jobs | Forgotten entirely |
| The human | Decisions, taste, relationships, logins, physical world | Scheduled last instead of first |

Two structural notes worth passing on when relevant:
- **Distributed local inference** buys the ability to *fit* a bigger model, never speed. Big-model sessions on a pooled cluster should be deliberate (queued jobs), not an always-on service, especially if the thinnest node is near its memory limit.
- **Confidential data** (client documents, financials, taxes, family photos) is the strongest reason to route to local — it converts the local tier from "worse model" to "only acceptable model" for those tasks.

## 3. Derive the routing rule

Compress the assessment into one rule the user can apply without thinking. Format: *task class → resource*, five or six lines maximum. Test it against the user's actual project list — it should route ~90% of tasks without discussion. The remainder get decided in sprint planning.

Also derive:
- **Premium shortlist criteria** — what earns a task access to the scarcest bar (e.g., "architecture decisions, hardest analysis, final-quality prose — nothing else").
- **Background candidates** — anything batch-able, recurring, or overnight-able. These are the tasks most likely to stall for years on "the human's time" and the only ones compute can fully absorb.

## 4. Subscription rationalization (offer quarterly)

Multiple AI subscriptions overlap heavily. A defensible default:
- Keep the primary (deepest integration into the user's workflow) permanently.
- Keep a research/multimodal complement if the user does document-heavy work.
- Tie specialist subscriptions (live-data, image-gen) to the workstreams that justify them — cancel when the workstream pauses.
- Upgrade to the top plan for sprint-heavy months; downgrade after. Plans are monthly; usage isn't.

Present this as options with reasoning, not a verdict — it's the user's money.

## 5. Security note for agent experiments

If the user wants to run third-party autonomous agent frameworks on their machines, recommend isolation as a default posture: dedicated machine, LAN-only, authentication on, no third-party plugin/skill marketplace content, and no credentials to email, banking, or the main filesystem. Agent frameworks have a track record of exposed instances and malicious marketplace content; the "isolated old laptop" is exactly the right home for such experiments. For agent work touching sensitive files, a first-party agentic tool from the model vendor is the safer default.
