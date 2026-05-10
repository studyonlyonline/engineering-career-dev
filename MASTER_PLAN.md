# Master Plan: SDE2 → Google L5 / Staff within One Year

> Living document. Everything I learn, read, solve, or reflect on during this prep gets tracked here so it compounds over time and serves as a future learning resource.

## Overall Goals

- **Target Role:** Google L5 (Senior Software Engineer) or Staff Engineer
- **Target Comp:** Highest possible total compensation
- **Key Driver:** Become the undeniable, "deal breaker" candidate. No rejections allowed.
- **Current Baseline:** Amazon SDE2, beginning structured preparation.

## Focus Areas

### 1. System Design (Mastery Level)

- Deep dive into distributed system patterns: caching, sharding, replication, consensus (Raft, Paxos), load balancing, partitioning, leader election.
- Practice designing real-world systems end-to-end:
  - Realtime messaging (WhatsApp / Slack)
  - Collaborative editor (Google Docs)
  - Global load balancer
  - Feed / timeline, rate limiter, URL shortener, distributed cache, search, ad serving, etc.
- Emphasize **trade-offs** and **concrete constraints**: latency budgets, QPS, availability targets, consistency models, cost.
- Produce reusable templates: requirements → capacity → API → data model → architecture → deep dives → trade-offs.

Workspace home: [`system-design/high-level-designs/`](system-design/high-level-designs/) and [`system-design/low-level-design/`](system-design/low-level-design/).

### 2. Data Structures & Algorithms (Optimal Problem Solving)

- Daily coding practice with focus on graph theory, dynamic programming, and data modeling.
- Optimize for the right resource: time complexity, space complexity, I/O, cache locality.
- Practice articulating thought process: clarify → examples → brute force → optimize → code → test → complexity.
- Log every meaningful problem with approach, complexity, pitfalls, and the key insight that unlocked it.

Workspace home: [`ds-algo/`](ds-algo/).

### 3. AI Principles & Agentic Systems (Design and Integration)

- Core AI principles for builders: LLM fundamentals, prompting, context windows, tool use, function calling, structured output, evals.
- Agentic AI solutions: single-agent vs multi-agent, planning, reasoning loops (ReAct, reflection, tree-of-thought), memory (short-term, long-term, episodic), tool orchestration, human-in-the-loop.
- Agentic design patterns:
  - Orchestrator / worker, planner / executor, router, supervisor
  - Tool-use and function-calling patterns
  - Retrieval-Augmented Generation (RAG): naive, hybrid, agentic, graph-based
  - Reflection, critique, and self-correction loops
  - Guardrails, policy enforcement, and safety layers
  - Async / event-driven agent workflows
- System design for AI: latency and cost budgeting, caching (prompt, embedding, response), batching, streaming, retries and idempotency, observability (traces, evals, feedback loops), offline vs online evals, drift and regression detection.
- Integration concerns: MCP, agent-to-agent protocols, sandboxing tools, secrets and permissions, rollout and A/B testing of agent behavior.

Workspace home: [`ai-agents/`](ai-agents/).

### 4. Leadership & Impact (Staff Mindset)

- Roleplay scenarios: defending technical decisions to stakeholders, conflict resolution, cross-team alignment, driving consensus.
- Analyze past projects for:
  - Technical judgment and trade-offs made
  - Scope and ambiguity handled
  - Mentorship and multiplier effect
  - Bar-raising contributions
- Build a crisp story bank (STAR format) covering ambiguity, conflict, scale, mentorship, failure, impact.

## Workspace Layout

```
.
├── MASTER_PLAN.md                    # this file — the living plan
├── README.md                         # short overview of the repo
├── ds-algo/                          # DSA notes, patterns, problem logs
├── ai-agents/                        # AI principles, agentic design patterns
└── system-design/
    ├── high-level-designs/           # HLD practice and notes
    └── low-level-design/             # LLD / OOD practice and notes
```

Future additions we will likely want:
- `leadership/` — story bank, behavioral prep, frameworks
- `mastery-log/` — running log of problems solved, designs done, breakthroughs

## Mastery Log (Running Tracker)

Problem statements, system design challenges, and conceptual breakthroughs get logged here as they are covered. Each entry should be small but concrete.

Suggested entry format:

```
### YYYY-MM-DD — <topic / problem name>
- Area: DSA | HLD | LLD | AI-Agents | Leadership
- Summary: one-line what it was
- Key insight: what finally clicked
- Complexity / trade-offs: ...
- Follow-ups: variants to revisit, weak spots to drill
- Links: file in this repo, external references
```

### Entries

_To be filled in as prep progresses._

## Operating Principles

- **Living document.** Refine this plan continuously. Stale plans lose their power.
- **Depth over breadth.** One well-understood system or pattern beats five shallow passes.
- **Explain to learn.** If I can't teach it clearly, I don't know it yet.
- **Track trade-offs, not just solutions.** Senior/Staff signal lives in the "why not the other option".
- **Compound the log.** Every session leaves behind a note future-me can reuse.

## Open Questions / To Refine

- Weekly cadence and time budget per focus area.
- Target companies beyond Google and interview timeline.
- Mock interview plan (peers, paid services, cadence).
- Metric for "ready": number of HLDs done, DSA problems at tier, mock scores.
