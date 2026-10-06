# 12-Week Interview Mastery Program
**Subhash Bohra · Target: Principal / Staff / Director-level platform, data & Kubernetes roles**
**Start date: ____________   ·   Target offer window: ____________**

---

## 0. The diagnosis this program is built on

Three rounds so far (Mastercard, AutoZone, and Citi pending) show a consistent pattern:

| What's working | What isn't |
|---|---|
| Getting calls and screens — positioning on paper is fine | Converting technical rounds |
| Real, rare experience (lakehouse at scale, air-gapped LLM, CI/CD estate) | Communicating it as reasoning rather than as a component list |
| Breadth across AWS, GCP, K8s, data | Depth on demand — going blank, missing layer-4 detail |
| | Targeting roles adjacent to your core (Java app leadership, Azure-primary) |

**So the program has four workstreams:** depth, delivery (storytelling and structure), reps under pressure, and targeting.

### The standard you're being measured against
An enterprise architect or principal-level interviewer is scoring five things, usually without telling you:

1. **Did you quantify before you designed?** (requests/sec, data volume, latency, RPO/RTO)
2. **Did you give options and justify the choice?** (one answer = junior signal)
3. **Did you name the failure modes?** (what breaks, how you'd detect it, what the user sees)
4. **Did you cover non-functionals?** (security, cost, operability, compliance)
5. **Did you show you've actually operated this?** (specific numbers, specific scars)

Write these five on your crib sheet. Every design answer should visibly hit all five.

---

## 1. Workstream A — Depth (the 20-topic scorecard)

### The 5-layer test
For each topic, grade yourself 0–3:
- **0** — can't explain it
- **1** — one-line definition only (layer 1)
- **2** — know how it works and when not to use it (layers 2–3)
- **3** — know what breaks in production and have a story with numbers (layers 4–5)

Anything scored below 2 goes on the study list. Target: all core topics at 3, all secondary at 2+ by week 6.

### Core (must reach 3 — this is your claimed identity)

| # | Topic | Layer-4 question you must be able to answer | Score | Target date |
|---|---|---|---|---|
| 1 | Kubernetes scheduling & resources | What happens on node pressure? Explain requests vs limits, QoS classes, OOMKill vs eviction | | |
| 2 | K8s networking & ingress | Pod-to-pod path, Services vs Ingress vs Gateway, DNS failures, why a service mesh | | |
| 3 | K8s autoscaling | HPA vs VPA vs Cluster Autoscaler vs KEDA; why scaling is too slow for a sudden spike | | |
| 4 | GKE specifics | Autopilot vs Standard, regional clusters, Workload Identity, private clusters, upgrade strategy | | |
| 5 | BigQuery internals | Partitioning vs clustering, slots, shuffle, why a query costs what it costs, MERGE/CDC patterns | | |
| 6 | Data pipeline design | Batch vs streaming, watermarks, late data, exactly-once vs idempotent consumers, backfills | | |
| 7 | Pub/Sub & messaging | Delivery guarantees, ordering keys, dead letters, backlog handling, Kafka comparison | | |
| 8 | Lakehouse / Iceberg / Trino | Table formats, compaction, small-file problem, metadata scaling, catalogue choices | | |
| 9 | Data governance & GDPR | Policy tags, row-level security, DLP, residency, VPC Service Controls, lineage | | |
| 10 | CI/CD at scale | Canary vs blue/green, rollback automation, monorepo vs polyrepo, supply-chain security | | |
| 11 | Observability & SLOs | Golden signals, SLO/error budget maths, cardinality problems, tracing | | |
| 12 | Cost engineering | Unit cost per transaction/tenant, commitment discounts, slot reservations, the top 3 levers | | |

### Secondary (reach 2, because panels wander here)

| # | Topic | Layer-4 question | Score | Target date |
|---|---|---|---|---|
| 13 | Caching & Redis | Cache-aside, stampede protection, atomic ops, hot keys, eviction policies | | |
| 14 | Relational DB internals | Isolation levels, locking, index choice, connection pool exhaustion, zero-downtime migrations | | |
| 15 | Distributed transactions | Saga, outbox, 2PC and why it's avoided, idempotency keys, compensations | | |
| 16 | Identity & auth | OAuth 2.0 flows, OIDC vs OAuth, JWT validation, mTLS, API keys vs tokens | | |
| 17 | HA & DR | RPO/RTO-driven design, 4 strategies, failover vs failback, game days | | |
| 18 | Edge & DDoS | L3/4 vs L7, WAF, rate limiting, CDN, economic denial of service | | |
| 19 | Java / Spring Boot literacy | Enough to review code and design a service: REST, JPA, pooling, resilience, testing | | |
| 20 | Applied AI / agentic systems | RAG patterns, agent orchestration, evaluation, guardrails, inference cost and latency | | |

**Weekly routine:** pick 2 topics. For each, write a half-page covering all 5 layers, then explain it out loud in 3 minutes without notes. If the explanation wanders, the understanding is incomplete.

---

## 2. Workstream B — Stories (C³AR, 10 of them)

**Format:** Context → Constraint → **Choice** → Action → Result (+ what you'd change)

The **Choice** step is what senior panels score. "We evaluated X and Y, chose X because Z, and accepted trade-off W."

### Your story bank

| # | Story | Headline number | 90-sec ready | 4-min ready |
|---|---|---|---|---|
| 1 | Lakehouse built end-to-end on GDC/GKE | 2B+ rows/day | ☐ | ☐ |
| 2 | Oracle → Iceberg migration, EAP decommission | ~£1M+ saved | ☐ | ☐ |
| 3 | Air-gapped LLM RCA platform | MTTR down 40–60% | ☐ | ☐ |
| 4 | CI/CD consolidation on GitHub Actions | 80+ services, 19K branches, PR cycle ~2 days | ☐ | ☐ |
| 5 | SonarQube → CodeQL + JaCoCo | Licence cost to zero, bank-wide rollout | ☐ | ☐ |
| 6 | OpenShift 3 → 4 migration | 80+ microservices, zero-downtime | ☐ | ☐ |
| 7 | Geography-based access control on CLM data | GDPR/residency compliance | ☐ | ☐ |
| 8 | TCS: scaled a QA engagement 0 → 80+ engineers | $10M engagement | ☐ | ☐ |
| 9 | A failure you owned — outage, missed date, wrong call | What you changed afterwards | ☐ | ☐ |
| 10 | Growing or managing out an engineer | The outcome for them | ☐ | ☐ |

### Template to fill for each

```
STORY #__  ·  Use for: [scale / migration / AI / leadership / failure]

CONTEXT (1 sentence, with scale)
CONSTRAINT (what made it genuinely hard — regulation, deadline, legacy, skills)
CHOICE (options considered → what you picked → why → trade-off accepted)
ACTION (what YOU personally did — not "we")
RESULT (number, plus business impact)
REFLECTION (what you'd do differently now)
```

**Rules:** every story has a number. Say "I" for your own work and "we" for the team's, and be precise about which. Never run past 4 minutes without being asked.

---

## 3. Workstream C — Structure (memorise 3 skeletons)

### Skeleton 1 — System design (use for every architecture question)
```
1. CLARIFY        2–3 questions. Users? Volume? Latency? Compliance? Deadline?
2. QUANTIFY       Do the arithmetic out loud. RPS, data/day, peak multiplier, storage.
3. OPTIONS        Two or three, with trade-offs. State your recommendation.
4. DRAW           Users → Edge (DNS/CDN/WAF) → LB → Compute → Data → Async → External
                  Side panel: Observability | Security/IAM | CI/CD | Cost
5. FAIL IT        Zone dies / region dies / DB saturates / dependency down / bad deploy
6. CLOSE          Rollout phases, first milestone, top 3 risks, cost levers, team & skills
```

### Skeleton 2 — Troubleshooting
```
Symptom → Blast radius (who's affected) → Golden signals → What changed recently →
Hypothesis → How I'd verify → Mitigate now → Fix properly → Prevent (detection + test)
```

### Skeleton 3 — Leadership / behavioural
```
Situation → Stakeholders & tension → My decision → How I communicated it →
Outcome (number) → What I learned
```

**Memorise step 1 of each.** Under stress you don't need the answer, you need the first move.

---

## 4. Workstream D — Reps under pressure

Going blank is a stress-retrieval failure, and only exposure fixes it.

- **Record every practice answer.** Watching yourself is the fastest correction loop available.
- **Deliberately interview at 3–4 companies you don't want** during weeks 3–6. Free, realistic pressure, zero cost to fail.
- **Mock with a human weekly.** A peer, a former colleague, or a paid service. Self-practice doesn't reproduce the stress.
- **Crib sheet for remote rounds:** one page with the three skeletons and your story numbers. Keep it beside you.

### In-the-moment tactics
1. "Let me think about that for a few seconds." Then actually pause.
2. Ask a clarifying question to restart your thinking.
3. Start drawing or writing — externalising frees working memory.
4. Narrate your reasoning, including the uncertainty.
5. Fall back to first principles: what's the constraint, where does state live, what fails.
6. If you don't know: "I haven't run that in production. Here's how I'd approach it and what I'd validate first." Then move on without apologising twice.

---

## 5. The 12-week schedule

| Week | Focus | Deliverable | Gate |
|---|---|---|---|
| 1 | Audit | Scorecard filled, 5 stories drafted, crib sheet v1 | Honest scores recorded |
| 2 | Foundations | 5 more stories, 2 topics to layer 4, 3 recorded designs | 5 stories at 90 sec, no notes |
| 3 | Depth + low-stakes reps | 2 topics, apply to 3–4 B-tier companies | First practice interview done |
| 4 | Depth + reps | 2 topics, 3 recorded designs, 1 human mock | Review recordings, list 3 fixes |
| 5 | Depth + reps | 2 topics, 2 real practice interviews | Post-mortem each within 2 hours |
| 6 | **Checkpoint** | All 10 stories ready, core topics at 3 | **45-min design round, no blanking** |
| 7 | Target list live | 8–12 researched companies, warm intros first | 5 applications with referrals |
| 8 | Real rounds | Interviews + 2 reps/week maintenance | Conversion tracker updated |
| 9 | Real rounds | Fix the top 3 fumbles from each round | Technical-round conversion improving |
| 10 | Real rounds | Push for finals; keep the pipeline wide | 3+ processes live simultaneously |
| 11 | Convert | Cluster final rounds into one window | 2+ offers or final stages |
| 12 | Negotiate | Hold the walk-away number | Offer signed |

### Weekly rhythm (~8 hours, non-negotiable)

| When | Duration | What |
|---|---|---|
| Mon–Fri morning | 45 min | One topic to layer 4, explained out loud |
| Tue & Thu evening | 45 min | Timed design question, drawn and recorded |
| Saturday | 2 hours | Story rehearsal + one human mock |
| Sunday | 1 hour | Review recordings, update crib sheet, pipeline admin |

**Protect the weekday mornings above everything.** Consistency beats intensity; a skipped Saturday is recoverable, a skipped week isn't.

---

## 6. Targeting (where the comp actually is)

### Your winning identity
> "Platform and data engineering leader who builds Kubernetes-based data platforms at bank scale, and ships applied AI into production operations."

Apply to roles where the panel's hardest question falls **inside** that sentence. Everything else is an uphill round.

### Fit filter — score each role before applying (max 10)

| Criterion | Weight |
|---|---|
| Core tech matches platform/data/K8s | 3 |
| Level and scope at or above current | 2 |
| Comp band reaches target | 2 |
| Warm intro or referral available | 2 |
| Location workable | 1 |

**7+** apply with full prep · **5–6** apply only with a referral · **below 5** skip.

### Where the 90L+ roles concentrate
- Product companies at Principal or Staff level (Bangalore, Hyderabad; some Pune and NCR)
- GCC Director or Senior Director roles for platform or data engineering
- Fintech and trading platforms, where your regulated-domain depth is a premium
- Data-infrastructure and AI-infrastructure companies

Expect much of the package at that band to be **total comp with stock**, not fixed. Decide early what mix you'll accept, and be realistic that a high fixed-only number narrows the pool sharply.

---

## 7. Conversion tracker

Log every process. The ratios tell you which workstream to escalate.

| Company | Role | Level | Source | Applied | Screen | Tech | Final | Outcome | Top fumble |
|---|---|---|---|---|---|---|---|---|---|
| Mastercard | Principal Cloud Infra | | | | ✓ | ✓ | | Not selected | |
| AutoZone | Tech Mgr / IT Mgr | | Referral | | ✓ | ✓ | | Not selected | Flash-sale design, OAuth |
| Citi | AWS Lead Solution Eng | | | | | | | Pending | |
| | | | | | | | | | |

### How to read your ratios
- **No screens** → targeting or resume problem
- **Screens but no technical rounds** → positioning and pitch problem
- **Technical rounds but no finals** → depth and delivery problem *(your current bottleneck)*
- **Finals but no offers** → leadership-fit or comp-expectation problem

---

## 8. Rules of the program

1. **No unprepared interviews.** Can't give it 4 hours of prep? Move the date.
2. **No applications scoring below 5 on the fit filter.**
3. **Every rejection produces 3 written fixes within 48 hours.** Then stop replaying it.
4. **Record and review.** Unrecorded practice doesn't count.
5. **Three live processes minimum** before any final round, because leverage requires parallelism.
6. **Decide your walk-away number before round one**, and write it here: ____________
7. **Protect sleep the night before an interview.** Retrieval under stress is the first thing fatigue destroys.

---

## 9. Post-mortem template (fill within 2 hours of every round)

```
Company / Role / Interviewer level:
Questions asked (all of them):
What went well:
Where I hesitated or blanked:
What I'd answer differently, written out properly:
Three fixes, with dates:
Topic scorecard updates:
```

The compounding value of this program lives in this template. Six filled post-mortems will teach you more than sixty hours of reading.

---

## 10. Immediate next 7 days

- [ ] Day 1 — Fill the 20-topic scorecard honestly. No grade inflation.
- [ ] Day 2 — Write stories 1–3 in full C³AR form.
- [ ] Day 3 — Write stories 4–6. Build crib sheet v1.
- [ ] Day 4 — Topic deep-dive #1 (pick your lowest core score). Explain it out loud, recorded.
- [ ] Day 5 — Timed design question: "Flash sale, 500 units, 10K concurrent buyers." Record it. Compare against the answer in the generic playbook.
- [ ] Day 6 — Stories 7–10. Rehearse all 10 at 90 seconds.
- [ ] Day 7 — Build the target company list and score each on the fit filter.
```
