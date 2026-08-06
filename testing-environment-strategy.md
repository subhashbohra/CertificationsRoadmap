# Testing & Environment Strategy for a 40-Team / 80-Microservice Estate

**Context:** ~40 teams, ~500 people, ~80 microservices, 2-week sprints.
**Current estate:** 1 dev environment, int1–int2, sit1–sit4, uat1–uat4.
**Current pain:** ~1,500 defects per release; teams cannot get environment time; a team recently skipped UAT testing entirely and deployed to prod, causing a full outage.

---

# Part 1 — The Problem in Simple Language

## Your question, restated

You actually have **two separate problems** that everyone keeps mixing together. They need different solutions.

### Problem A: "My team changed one service. Where do we test it?"

Today the answer is: go to the shared dev environment.

But 40 teams are doing the same thing. So the dev environment contains:

- Your new version of Service X
- Team B's half-broken Service Y
- Team C's Service Z that is mid-deployment right now
- Team D's database migration that just ran

When your test fails, **you have no idea whose fault it is.** So you re-run it. It fails differently. You debug for two hours and discover Team C was deploying. You give up and raise a defect. Multiply by 40 teams.

This is not a testing problem. It is a **blast radius** problem. Everyone is standing in the same room.

### Problem B: "Everything is merged. Does the whole system still work?"

This is real integration testing plus regression. It genuinely needs a stable, prod-like environment with prod-like data. This is what UAT should be for.

**The mistake most large orgs make:** they try to solve Problem A and Problem B with the same environments. Then Problem A traffic (unstable, half-finished code) destroys the stability that Problem B needs. And Problem B's booking calendar starves Problem A of capacity.

Your int/sit/uat estate is trying to do both. That is why neither works.

---

## The maths behind your pain

You have roughly 10 fixed lower environments and 40 teams.

- **4 teams per environment**, permanently.
- If each team needs an environment for even 2 days per sprint, you need ~80 team-days of environment capacity per sprint. You have ~100 environment-days total — but only if utilisation is perfect, nothing breaks, no environment is reserved for a release, and no data refresh is running. Utilisation is never perfect.
- So in practice you are at or above 100% booked. **Queues form. Teams skip steps.**

Adding more environments does not fix this. If you go to 20 environments, you now have 20 environments to keep in sync, 20 sets of config drift, 20 data refreshes, and double the infrastructure cost — and teams will still queue, because demand expands to fill capacity.

**The number of fixed environments is the wrong lever.**

---

## What actually happened last week

The postmortem said "we did not have an environment to test."

That is the *symptom*, not the cause. The real chain was:

1. Environment scarcity created a queue.
2. Sprint deadline pressure meant someone had to choose between "miss the sprint" and "skip testing."
3. A human made a rational-under-the-circumstances decision to skip testing.
4. **Nothing in the system stopped them.** There was no gate that said "this change has not passed X, it cannot go to prod."
5. The change went to 100% of production traffic instantly. There was no canary, no feature flag, no automatic rollback.
6. So one bad change took down everything.

There are **four independent failure points** there. "No environment" is only the first one. Even with unlimited environments, points 4, 5 and 6 mean this will happen again.

**The single most important lesson:** a system where one untested change can reach 100% of production traffic in one step is fragile regardless of how much testing you do. Testing reduces the *probability* of a bad change. Progressive delivery reduces the *impact*. You need both, and impact-reduction is faster to implement.

---

# Part 2 — How Large Tech Companies Handle This

The counter-intuitive answer: **the biggest companies have fewer environments than you do, not more.**

Meta, Google, Netflix and Amazon all operate at far greater scale than 80 services and 40 teams. None of them run a chain of int → sit → uat environments. They deleted that model deliberately.

## The core philosophy shift

| Traditional model (yours) | Big tech model |
|---|---|
| Prove correctness by testing in a copy of prod | Prove correctness in isolation, then **release safely into prod** |
| Environments are the safety mechanism | **Deployment mechanics** are the safety mechanism |
| Deploy = release (same moment) | Deploy and release are **separate events** |
| Big batch, coordinated release | Small batch, continuous, independent |
| Detect defects in a staging environment | Detect defects in prod, in seconds, at 0.1% exposure |

They accepted a hard truth: **a staging environment is never really like production.** Different data volume, different traffic patterns, different third-party behaviour, different scale, different concurrency. So a large class of defects is simply undetectable in staging — you find them in prod anyway, just later and more expensively. Rather than spend enormous effort maintaining an imperfect copy, they invested that effort in making production changes safe and reversible.

## Meta / Facebook

Meta's approach is the most extreme example.

**Gatekeeper (feature flags).** Almost all new functionality ships to production behind a flag, turned off. Code being *in* production and code being *live* are completely different things. A release becomes a config change, not a deployment. Turning something off takes seconds and needs no deploy.

**Dogfooding.** New code is enabled for Meta employees first — a large, real population using real production systems. This is their "UAT," and it uses real production infrastructure rather than a copy of it.

**Staged rollout rings.** Employees → small percentage of real users → larger percentage → 100%. Each ring is monitored against health metrics. If metrics degrade, the flag flips off.

**Continuous push.** Code moves to production continuously rather than in coordinated big-bang releases. Small changes, constantly. The blast radius of any single change is tiny because the change itself is tiny.

**Heavy automated gating before merge.** A large automated test suite runs on every change, selecting the tests actually relevant to what changed rather than running everything.

**Key takeaway for you:** Meta does not have a UAT environment that 40 teams queue for. Their UAT *is* production, with exposure control.

## Google

**Testing on the Toilet / test-first culture.** Very heavy investment in unit and small-scale tests owned by the team that writes the code. The expectation is that the vast majority of confidence comes from tests that run in seconds on a developer's change, not from a shared environment.

**Hermetic testing.** Tests bring up their own dependencies in isolation — an in-process or containerised database, fake versions of downstream services. No shared state. Two engineers' tests can never interfere. This is what kills your Problem A.

**Monorepo + a central CI system that only merges green changes.** A change that would break others does not get in. The "integration environment" is effectively the trunk itself, continuously verified.

**Canary deployments** with automated metric comparison before wider rollout.

**Key takeaway for you:** Google's answer to "the shared environment is unstable" is *don't share the environment.* Make every test bring its own world.

## Amazon

**Two-pizza teams own their service end to end**, including the pipeline and the on-call. There is no central release train that 40 teams synchronise to.

**Fully automated deployment pipelines** where a change flows from commit through stages automatically, with automated gates between them. Humans do not schedule deployments.

**One-box deployment.** New code goes to a single host (or a very small slice) first. Real production traffic hits it. Metrics are compared against the rest of the fleet. Only if healthy does it proceed to the wider fleet, then region by region.

**Automatic rollback on alarm.** If a monitored metric breaches, the pipeline rolls back without a human deciding.

**Key takeaway for you:** Amazon's safety comes from *sequencing and reversibility in production*, not from a pre-prod copy.

## Netflix

**No staging environment for most services.** Widely publicised. They deploy to production and control risk there.

**Automated canary analysis.** New version and current version run side by side taking real traffic. Hundreds of metrics are compared statistically and a pass/fail score is produced automatically. No human eyeballing dashboards.

**Chaos engineering.** They deliberately inject failure in production so that the system is *designed* to tolerate a bad instance, a failed dependency, a slow response. The system assumes things will break.

**Key takeaway for you:** if your system goes fully down because one service was bad, that is a resilience gap as much as a testing gap. Circuit breakers, timeouts, bulkheads and graceful degradation would have contained last week's incident even with the untested code deployed.

## The honest caveats for a regulated bank

You cannot copy this wholesale, and anyone who tells you otherwise is selling something.

- **You have regulatory obligations** around change control, segregation of duties, evidence of testing, and auditability. "We test in prod" will not survive an audit conversation framed that way.
- **You have real money and real customer data.** A canary that mis-prices trades for 0.1% of traffic for four minutes is not the same as showing the wrong thumbnail.
- **You have external dependencies** — market data providers, payment rails, regulators' systems — that you cannot spin up on demand or safely canary against.
- **Some testing genuinely must happen pre-prod**: anything touching settlement, regulatory reporting, or customer money movement.

**But the principles translate, even if the implementation is more conservative:**

| Big tech principle | Regulated-bank version |
|---|---|
| No staging | Fewer *shared* environments, many *ephemeral* ones |
| Test in prod | Progressive delivery with tight, automated, audited rollback |
| Feature flags | Feature flags with approval workflow and audit log |
| Continuous push | Independent per-service deployment, still change-managed but pre-approved as a standard change |
| Canary on real users | Canary on internal users / synthetic transactions / non-critical journeys first |
| Delete the test suite that doesn't earn its keep | Same — but keep a documented regulatory regression pack |

The point is not to become Netflix. The point is that **your defect count is high because you are relying on a mechanism (shared pre-prod environments) that stops scaling somewhere around 10–15 teams, and you have 40.**

---

# Part 3 — What To Do About It

## The simple version

**For Problem A (my team's one service):**

Give every team the ability to spin up an environment **on demand, in minutes, containing only their service running for real** — everything else faked or stubbed. Nobody else can break it. It dies when the pull request is merged. No booking, no queue, no contention.

**For Problem B (does everything still work together):**

Stop testing this by deploying everything and clicking through it. Instead:

1. **Contract tests** catch "Service A now sends a field Service B doesn't expect" — the single largest category of integration defect — and they catch it in the pull request pipeline, in seconds, without any environment at all.
2. A **small** end-to-end suite (15–30 critical business journeys, not hundreds) runs automatically on a permanently-stable integration environment that nobody deploys to manually.
3. **UAT becomes a genuinely stable environment** for business validation and the regulatory regression pack — because all the unstable early-stage testing has moved off it.

**For last week's incident:**

Make it structurally impossible. A change that has not passed its gates cannot deploy — the pipeline refuses, not a person. And every deployment goes out progressively with automatic rollback, so even a change that slips through cannot take the whole system down.

## Rebalancing your environment estate

Current: 1 dev + 2 int + 4 sit + 4 uat = 11 fixed shared environments, all contended.

Suggested target shape:

| Purpose | What it is | Who controls it |
|---|---|---|
| **Per-PR ephemeral** | Namespace on Kubernetes, your service real, all dependencies virtualised. Minutes to create, auto-destroyed. Unlimited. | The team, automatically |
| **INT (1, maybe 2)** | Auto-deployed on merge only. No manual deploys, ever. Runs the core E2E journey suite. | Pipeline only — no human deploy access |
| **SIT (2)** | Cross-domain integration where real downstreams are genuinely needed. Auto-deployed. | Pipeline only |
| **UAT (2)** | Prod-like data volumes. Business validation + regulatory regression pack. Stable by construction. | Release process |
| **Perf (1)** | Prod-shaped. Isolated so perf runs don't disturb functional testing. | Booked, scheduled |
| **Prod** | Progressive delivery, feature flags, canary, auto-rollback | Pipeline + change control |

Note this is *fewer* fixed environments, not more — because the ephemeral tier absorbs the volume. The freed budget and the freed platform-team time pay for the tooling.

**The rule that makes this work: no human ever manually deploys to a shared environment.** The moment someone can hand-deploy to SIT2, SIT2 starts drifting and becomes unreliable, and you are back where you started.

---

# Part 4 — Target Architecture (Detail)

## First, diagnose before you spend

1,500 defects per release with 40 teams and one dev environment is not a testing problem — it is an *architecture-of-testing* problem. You are trying to validate 80 services as if they were a monolith.

Before spending anything, run a **defect taxonomy** on the last 3 releases. In estates of this shape the split is typically:

- 30–40% environment / config / test-data issues (not real defects at all)
- 25–30% contract mismatches between services
- 15–20% genuine functional bugs
- 10–15% duplicates / non-reproducible
- 5–10% performance / resilience

That number tells you whether to fix pipelines or fix people. Most organisations sitting at 1,500 discover only ~300 are real code defects. **Do this first — it is two weeks of work and it will redirect the entire programme.**

## 1. Contract testing as the backbone (highest leverage)

Consumer-driven contracts — Pact broker or Spring Cloud Contract.

- Each consumer publishes what it expects from a provider.
- Each provider verifies those expectations in its own pipeline.
- `can-i-deploy` becomes a hard deployment gate: a service ships only if its contracts are green against what is currently live in production.

This is what lets you *delete* most cross-service integration testing, and it typically eliminates the entire 25–30% contract-mismatch bucket.

## 2. Per-service pyramid, not per-system

| Layer | Coverage | Where it runs | Budget |
|---|---|---|---|
| Unit | ~70% | PR pipeline | < 5 min |
| Component (service real, deps mocked; Testcontainers for its own DB) | ~20% | PR pipeline | < 15 min |
| Contract | All consumers/providers | PR + pre-deploy gate | < 5 min |
| E2E business journeys | 15–30 **globally** | Nightly + pre-prod | < 60 min |

**Rule: an E2E test must justify its existence.** Cap the suite globally and govern it. Forty teams each adding "just a few" E2E tests is exactly how you get a six-hour flaky suite that nobody trusts and everybody re-runs.

## 3. Ephemeral environments — killing the shared dev environment

- Namespace-per-PR on Kubernetes
- ArgoCD ApplicationSets for declarative provisioning
- Service virtualisation (WireMock / Hoverfly / Mountebank) for the 79 services you are *not* changing
- Seeded test data injected at creation
- Auto-destroyed on merge or after N hours

Spins up in minutes. Teams stop queueing. Nobody can break anybody else's test run.

## 4. Test data management (the silent killer in banking)

This is usually where the 30–40% environment-defect bucket actually lives.

Treat test data as a product:

- Synthetic data generation as the default
- Data-as-code, versioned alongside the service, seeded per ephemeral environment
- A test-data service that provisions a clean, known dataset on request
- Masked production extracts reserved for UAT and perf only

At 80-service scale, coordinating masked-prod refreshes across 11 shared environments becomes its own permanent project. Avoid it wherever synthetic data will do.

## 5. Progressive delivery

Decouple **deploy** from **release**.

- Feature flags (LaunchDarkly, Unleash, or in-house with audit logging)
- Canary deployment: 1% → 5% → 25% → 100%
- Automated analysis against SLOs at each step
- Automatic rollback on breach — no human decision in the loop

A defect that reaches 1% of traffic for four minutes and auto-rolls-back is not a release defect. This is the single fastest way to move your reported number — though note it is *masking* rather than *fixing*, so do it alongside items 1–4, not instead of them.

## 6. Resilience (what would have contained last week's outage)

One bad service should not be able to take the system down. Non-negotiables:

- Circuit breakers on every synchronous inter-service call
- Timeouts everywhere, with sensible budgets
- Bulkheads / connection pool isolation
- Graceful degradation paths for non-critical dependencies
- Regular game days / chaos exercises to prove these actually work

---

# Part 5 — The Process Inside a 2-Week Sprint

## The prerequisite: no release train

If 40 teams ship on one coordinated date, you have serialised 40 independent risk profiles into a single event, and you have guaranteed that integration problems surface at the worst possible moment. **Independent deployability is the prerequisite for everything above.**

If a service cannot deploy alone today, fix that before fixing testing. Nothing else will work until it is true.

## Inside the sprint

- **Trunk-based development.** Branches live under 24 hours. PRs under ~400 lines.
- **Definition of Done includes:** contracts published *and* provider-verified; component tests green; observability instrumented; feature flag defined.
- **Days 1–2:** consumer teams publish contract expectations for new work. Providers get early warning instead of a day-9 surprise.
- **Continuously:** merge → ephemeral env → auto-deploy to INT. There is no "integration phase."
- **Nightly:** core E2E journeys + resilience checks on INT.
- **Weekly:** performance regression on the perf environment.
- **Sprint end:** nothing special happens. That is the goal and that is the measure of success.

## Organisation

**Platform / QE Enablement team (8–12 people).** Owns the testing platform *as a product*: pipeline templates, ephemeral environment tooling, contract broker, test-data service, service virtualisation catalogue, shared observability. This is **not** a central QA team that tests things — it is a team that makes 40 teams able to test themselves.

**Testing guild.** One representative per team. Meets fortnightly. Owns standards, the global E2E budget, and the flaky-test kill list.

**Ownership rule.** The team that writes the service owns its tests, its pipeline, and its on-call. No handoff to a separate test team.

---

# Part 6 — Sequencing

| Phase | Timeframe | Focus |
|---|---|---|
| **0 — Diagnose** | Weeks 1–4 | Defect taxonomy on last 3 releases. Environment utilisation audit. Deployment coupling map. |
| **1 — Prove** | Months 1–3 | Contract testing pilot on 3–5 high-traffic services. Ephemeral environment PoC. Feature flag platform selected. Circuit breakers on top 10 critical paths. |
| **2 — Scale** | Months 3–6 | Contract testing across the top 30 services by change frequency. Ephemeral environments GA. Canary deployment on 5 services. Test-data service v1. |
| **3 — Consolidate** | Months 6–12 | Full rollout. E2E suite reduction to the capped number. Decommission the shared dev environment and 2–3 shared SIT/UAT environments. |

**Realistic target: 70–80% defect reduction by month 12** — most of it coming from the environment/config and contract buckets, not from testing harder.

**The uncomfortable part:** this is a 12–18 month programme requiring 40 teams to change how they work broadly in parallel. It needs an executive mandate and a funded platform team. Bottom-up adoption in my experience stalls somewhere around 15 teams, because the value of contract testing only appears once most of your dependencies participate.

---

# Part 7 — Discovery Questions

To turn this into a concrete plan for your estate, these are the things worth establishing.

## Deployment coupling (most important)

1. Can a single service go to production independently today, or is every release a coordinated event across many services?
2. If coordinated — what forces the coordination? Shared database schema? Shared libraries? Synchronous call chains? A change-management process? Something else?
3. How many deployments to production happen per month, and in how many distinct deployment events?

## Environment reality

4. What is the actual utilisation of int1–2, sit1–4, uat1–4? Booked vs genuinely in use?
5. How are they booked — a spreadsheet, a tool, a Slack channel, first-come-first-served?
6. How long does it take to provision a new environment today, and who does it?
7. How different are these environments from production and from each other? Is there a config drift problem?
8. Are they full copies of all 80 services, or partial?
9. What does one lower environment cost per month, all-in?

## Test data

10. Where does test data come from — masked production extract, synthetic generation, hand-crafted, or accumulated over years?
11. How often is it refreshed, and how long does a refresh take?
12. How many defects get raised that turn out to be test-data problems?

## Testing today

13. What is unit test coverage, and is it enforced as a merge gate?
14. Does any contract testing exist? Any API schema governance (OpenAPI/AsyncAPI registry)?
15. How large is the regression suite, how long does it take, and what is the flake rate?
16. Who executes regression — the teams, or a separate QA function?
17. Is regression automated, manual, or mixed? What percentage manual?

## The defects themselves

18. Where does the 1,500 number come from — which tool, and what counts as a defect?
19. What is the severity split? What percentage are closed as "not a defect," "duplicate," or "cannot reproduce"?
20. How many escape to production per release, and what is the mean time to detect and to restore?

## Production safety

21. Do feature flags exist anywhere today?
22. Is there any canary or blue/green capability, or is it deploy-to-all?
23. How long does a production rollback take, and is it automated?
24. Are circuit breakers and timeouts in place on inter-service calls? (Last week's total outage suggests gaps.)

## Architecture

25. Are the 80 services mostly synchronous REST call chains, or is there meaningful event-driven decoupling?
26. Do services share databases, or is each one's data private to it?
27. How many of the 80 are on the critical path for the top 5 business journeys?

## Organisation and constraints

28. Is there a platform or SRE team today? How large, and what does it own?
29. What are the hard regulatory constraints on change — segregation of duties, evidence requirements, approval gates?
30. Is there executive sponsorship for a 12–18 month engineering-practice change programme, or does this need to be delivered incrementally without a formal mandate?

---

## The one-line summary

You are trying to buy confidence with shared environments. That mechanism stops scaling at roughly 10–15 teams and you have 40. Large tech companies solved this by **removing** shared environments and moving the safety mechanism into isolated per-change testing, contract verification, and controlled production rollout. You cannot copy that wholesale in a regulated bank — but the direction of travel is the same, and every step in that direction reduces both your defect count and your outage risk.
