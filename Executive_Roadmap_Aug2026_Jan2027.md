# Executive Roadmap — Certification, Job Pivot & Consulting Foundation

**Owner:** Subhash Bohra
**Window:** 02 Aug 2026 → 30 Jan 2027 (26 weeks)
**Daily commit:** 2 hrs weekdays (1 hr Track 1 / 1 hr Track 2) + 3.5 hrs each weekend day
**Weekly budget:** ~17 hrs

> [!IMPORTANT]
> **Three corrections to the original plan — read these before booking anything.**
>
> 1. **The DevOps Pro exam code is `DOP-C02`, not `DOP-C01`.** DOP-C01 was retired. Buying a DOP-C01 course means studying a dead blueprint.
> 2. **CKA + CKAD + CKS does *not* make you a Kubestronaut.** CNCF requires **five active certifications simultaneously**: KCNA, KCSA, CKA, CKAD, CKS. KCNA and KCSA are multiple-choice and cheap — but they are not optional. They're scheduled into Week 24–25 below.
> 3. **The Track 2 hours don't add up for SA Pro.** Five weeks × 7 hrs = 35 hours for SAP-C02. Realistic prep for an experienced architect is 60–80 hours. Weekend surge blocks are built in below to close the gap. If you skip weekends in August, move the exam to Week 7.

---

## 🎯 Legend

| Marker | Meaning |
|:---:|---|
| 🟣 | **Track 1** — DSA + System Design (job interviews) |
| 🔵 | **Track 2** — Cloud / CNCF (certifications) |
| 🔴 | **Exam window** — booked, paid, non-negotiable date |
| 🟢 | **Milestone** — credential unlocked |
| ⚪ | Buffer / review week |

---

## Section 1 — Gate Conditions (do these in Week 1, not later)

- [ ] **Book the SAP-C02 exam today for a date in Week 5 (30 Aug – 05 Sep).** An unbooked exam has no deadline, and a plan without a deadline is a wish. This single action is the highest-leverage item on this page.
- [ ] Buy the Tutorials Dojo SAP-C02 practice set (needed from Week 4).
- [ ] Confirm your Adrian Cantrill SAP-C02 course access.
- [ ] Pick **one** LeetCode language and never switch. Recommendation: **Python** for speed of expression in a 45-min interview, unless your target companies screen in Java.
- [ ] Set up the **single-monitor split layout** (terminal left, k8s docs right). Practice this way from day one — this is the specific thing that cost you the CKA last time.
- [ ] Create a `killer.sh` account note: your two simulator sessions come free with exam registration and stay active 36 hours each. Don't burn them early.

---

## Section 2 — The 26-Week Schedule

### 🗓 MONTH 1 · AUGUST 2026 — AWS SA Pro Sprint
**Objective:** Multi-account frameworks, hybrid networking, HA/DR architecture
**Resources:** Adrian Cantrill (theory) · Tutorials Dojo (practice) · ByteByteGo Ch. 1–3

| Wk | Dates | 🟣 Track 1 — Coding & Design | 🔵 Track 2 — Cloud / CNCF | Done |
|:--:|---|---|---|:--:|
| W1 | Aug 02 – Aug 08 | Arrays & Hashing (Easy→Med) + TinyURL design | AWS Organizations, SCPs, Control Tower | ☐ |
| W2 | Aug 09 – Aug 15 | Two Pointers + Rate Limiter (token/leaky bucket) | VPC Peering, Transit Gateway, Direct Connect | ☐ |
| W3 | Aug 16 – Aug 22 | Sliding Window + Distributed Chat System | Route 53 policies, CloudFront, Multi-Region DR | ☐ |
| W4 | Aug 23 – Aug 29 | Stack problems + Distributed ID Generator | **Tutorials Dojo full mocks + wrong-answer autopsy** | ☐ |
| W5 | Aug 30 – Sep 05 | Review weak patterns + Video Streaming design | 🔴 **EXAM: AWS SAP-C02** | ☐ |

> [!TIP]
> **Weekend surge (Aug):** Sat/Sun 3.5 hrs each go entirely to Track 2 in August. You need the volume for SA Pro. Track 1 rides on weekday hours only this month.

**Exit gate:** 🟢 AWS Certified Solutions Architect – Professional

---

### 🗓 MONTH 2 · SEPTEMBER 2026 — AWS DevOps Pro Bridge
**Objective:** CI/CD at scale, governance, config management, auto-remediation
**Why now:** ~60% conceptual overlap with SAP-C02. This window closes fast — every week you wait, the overlap decays.
**Exam code:** `DOP-C02`

| Wk | Dates | 🟣 Track 1 — Coding & Design | 🔵 Track 2 — Cloud / CNCF | Done |
|:--:|---|---|---|:--:|
| W6 | Sep 06 – Sep 12 | Linked Lists + Consistent Hashing | Multi-account CodePipeline, Blue/Green + Canary | ☐ |
| W7 | Sep 13 – Sep 19 | Binary Search + Distributed Cache (Redis) | AWS Config rules, Inspector, SSM, Patch Manager | ☐ |
| W8 | Sep 20 – Sep 26 | Trees, DFS/BFS + Metrics Monitoring system | CloudFormation StackSets, drift detection, Service Catalog | ☐ |
| W9 | Sep 27 – Oct 03 | LeetCode Medium review + Distributed Message Queue | 🔴 **EXAM: AWS DOP-C02** | ☐ |

**Exit gate:** 🟢 AWS Certified DevOps Engineer – Professional

---

### 🗓 MONTH 3 · OCTOBER 2026 — CKA Redemption
**Objective:** Terminal speed and imperative muscle memory. Not knowledge — *speed*.
**Resources:** KodeKloud (Mumshad) · killer.sh · Kubernetes docs only

> [!WARNING]
> You didn't fail the CKA on architecture. You failed it on clock management. Everything in this month is timed. If a drill isn't timed, it doesn't count.

| Wk | Dates | 🟣 Track 1 — Coding & Design | 🔵 Track 2 — Cloud / CNCF | Done |
|:--:|---|---|---|:--:|
| W10 | Oct 04 – Oct 10 | Heap / Priority Queue + Web Crawler | K8s architecture, etcd backup/restore, kubeadm upgrades | ☐ |
| W11 | Oct 11 – Oct 17 | Tries + Typeahead / autocomplete design | Networking, CNI (Calico/Flannel), Ingress, Gateway API | ☐ |
| W12 | Oct 18 – Oct 24 | Graphs (shortest path, topo sort) + Distributed KV store | Broken nodes, kubelet systemd failures, static pods | ☐ |
| W13 | Oct 25 – Oct 31 | Backtracking + API Gateway design | **killer.sh drills — target >80% inside 90 min** | ☐ |
| W14 | Nov 01 – Nov 07 | System design trade-off review | 🔴 **EXAM: CKA** | ☐ |

**Exit gate:** 🟢 Certified Kubernetes Administrator *(also the prerequisite gate for CKS)*

---

### 🗓 MONTH 4 · NOVEMBER 2026 — CKAD Velocity Sprint
**Objective:** Application-layer topologies at speed. Easiest exam in the loop if CKA is fresh.

| Wk | Dates | 🟣 Track 1 — Coding & Design | 🔵 Track 2 — Cloud / CNCF | Done |
|:--:|---|---|---|:--:|
| W15 | Nov 08 – Nov 14 | DP intro + Notification System | Multi-container pods (sidecar/adapter/ambassador), quotas & limits | ☐ |
| W16 | Nov 15 – Nov 21 | Greedy + Real-time Leaderboard | StorageClasses, PV/PVC, ConfigMaps, Secrets, probes | ☐ |
| W17 | Nov 22 – Nov 28 | Advanced DP + Geospatial ride-hailing design | Imperative speed runs — Helm & Kustomize basics | ☐ |
| W18 | Nov 29 – Dec 05 | Mock interviews + Distributed File Storage | 🔴 **EXAM: CKAD** | ☐ |

**Exit gate:** 🟢 Certified Kubernetes Application Developer

---

### 🗓 MONTH 5 · DECEMBER 2026 — CKS Security Hardening
**Objective:** The credential that actually differentiates you for banking/fintech consulting.
**Prerequisite:** Active CKA ✅ (from W14)

| Wk | Dates | 🟣 Track 1 — Coding & Design | 🔵 Track 2 — Cloud / CNCF | Done |
|:--:|---|---|---|:--:|
| W19 | Dec 06 – Dec 12 | Advanced Graphs + Payment Gateway LLD | Cluster hardening, CIS benchmarks, securing kube-apiserver | ☐ |
| W20 | Dec 13 – Dec 19 | Bit Manipulation + Checkout concurrency models | Runtime security, Falco, AppArmor / seccomp | ☐ |
| W21 | Dec 20 – Dec 26 | SQL mastery + index optimisation | NetworkPolicies, admission controllers, OPA/Kyverno | ☐ |
| W22 | Dec 27 – Jan 02 | System design mock drills + Ticket Booking (seat locking) | killer.sh security scenarios, Trivy image scanning | ☐ |
| W23 | Jan 03 – Jan 09 | Resume + LinkedIn rebuild | 🔴 **EXAM: CKS** | ☐ |

**Exit gate:** 🟢 Certified Kubernetes Security Specialist

---

### 🗓 MONTH 6 · JANUARY 2027 — Kubestronaut Close-out & Market Entry
**Objective:** Finish the badge, then convert credentials into interviews.

| Wk | Dates | Focus | Done |
|:--:|---|---|:--:|
| W24 | Jan 10 – Jan 16 | 🔵 **KCNA exam** (multiple choice, ~1 week prep) · 🟣 Recruiter outreach: VP/Senior Director/Principal Architect specialists | ☐ |
| W25 | Jan 17 – Jan 23 | 🔵 **KCSA exam** → 🟢 **KUBESTRONAUT** · 🟣 Selective applications: AWS ProServe, Google Cloud, fintech scale-ups | ☐ |
| W26 | Jan 24 – Jan 30 | Consulting entity formalised · profile relaunch with Kubestronaut badge · first outbound campaign | ☐ |

> [!NOTE]
> **KCNA and KCSA are the cheapest wins on this entire page.** Both are multiple-choice, both are well within reach after CKS, and without them the "Kubestronaut" line on your LinkedIn is simply not true. Do not skip them.

---

## Section 3 — Certification Ledger

| # | Certification | Code | Format | Target Week | Status |
|:--:|---|---|---|---|---|
| 1 | AWS Solutions Architect – Professional | SAP-C02 | Scenario MCQ, 180 min | W5 (Sep 05) | ☐ Not booked |
| 2 | AWS DevOps Engineer – Professional | DOP-C02 | Scenario MCQ, 180 min | W9 (Oct 03) | ☐ Not booked |
| 3 | Certified Kubernetes Administrator | CKA | Hands-on, 120 min | W14 (Nov 07) | ☐ Not booked |
| 4 | Certified Kubernetes App Developer | CKAD | Hands-on, 120 min | W18 (Dec 05) | ☐ Not booked |
| 5 | Certified Kubernetes Security Specialist | CKS | Hands-on, 120 min | W23 (Jan 09) | ☐ Not booked |
| 6 | Kubernetes & Cloud Native Associate | KCNA | MCQ, 90 min | W24 (Jan 16) | ☐ Not booked |
| 7 | Kubernetes & Cloud Native Security Assoc. | KCSA | MCQ, 90 min | W25 (Jan 23) | ☐ Not booked |

> [!TIP]
> Buy the **Kubestronaut bundle** from the Linux Foundation rather than five separate exams — it is materially cheaper than individual registrations and each includes killer.sh access. Also check the LF sale calendar; CNCF exams discount heavily around Black Friday and KubeCon.

---

## Section 4 — Weekly Operating Rhythm

| Slot | Duration | Purpose |
|---|---|---|
| Weekday early morning | 1.5 hrs | Track 2 — video, docs, architecture deep dives (fresh brain, hard material) |
| Weekday late night | 0.5 hrs | Track 1 — active recall, flashcards, one LeetCode problem |
| Saturday | 3.5 hrs | Timed labs / full practice exams |
| Sunday | 3.5 hrs | Wrong-answer autopsy + system design writeup (one design per week, written out) |

**Sunday review — five questions, ten minutes, every week:**
1. Did I hit the timed drills, or did I only watch videos?
2. What was my practice-exam score and which domain was weakest?
3. How many LeetCode problems did I solve *without* looking at the solution?
4. Is next week's exam still on schedule, or do I need to move the booking now?
5. What did I skip, and why?

---

## Section 5 — Three Rules That Decide the Outcome

**1. Single monitor, split screen.** Terminal left, `kubernetes.io/docs` right. Never practise with two screens. The exam gives you one, and re-learning the layout under time pressure is where the minutes go.

**2. Never hand-write YAML.** Generate every manifest:
```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
kubectl create deploy web --image=nginx --replicas=3 --dry-run=client -o yaml > deploy.yaml
kubectl create job test --image=busybox --dry-run=client -o yaml -- /bin/sh -c 'echo hi'
```
Set these up front in every practice session:
```bash
alias k=kubectl
export do="--dry-run=client -o yaml"
export now="--force --grace-period=0"
```

**3. Patterns, not problems.** For DSA, the win condition is recognising *which* pattern a problem is in under 60 seconds — sliding window vs two pointers vs heap. Grinding 500 random questions without that recognition step produces nothing at interview.

---

## Section 6 — Consulting Track (parallel, low-intensity)

Keep this to background activity until CKS is cleared. Splitting focus before January will cost you exams.

- [ ] Decide entity structure (LLP vs Private Limited). Pvt Ltd if you intend to hire; LLP if you'll stay solo.
- [ ] Open a business current account once the entity exists.
- [ ] Draft a one-page positioning statement around three offers: **air-gapped / sovereign AI infrastructure**, **FinOps and cloud cost architecture**, **enterprise GitOps and Kubernetes migration**.
- [ ] Register with expert networks (GLG, AlphaSights, Guidepoint) — hourly advisory calls, no delivery commitment, and the cleanest first revenue line.
- [ ] Evaluate Toptal / Braintrust applications for Q1 2027, not before.

> [!CAUTION]
> **On running this alongside your current role — the transcript's advice was wrong in a way that matters.**
>
> That chat framed an offshore B2B entity as a way to keep outside consulting invisible to your employer. Structuring for non-detection is not the same thing as being compliant, and financial-services employers are the worst possible place to test that distinction. Deutsche Bank almost certainly requires **written pre-approval for any outside business activity** — that obligation attaches to *you*, not to whichever entity signs the contract. Routing income through an LLP doesn't dissolve it; it just adds a paper trail that looks worse if it's ever examined.
>
> The correct sequence: read your employment contract and the outside-business-activity / conflicts policy in the staff handbook, then either get written approval through the proper channel or wait. Advisory-network calls and approved side work are often permitted with disclosure. Undisclosed competing work in your own domain generally is not. Talk to an employment lawyer in India before you sign anything, not after.
>
> This is not a reason to abandon the plan. It's a reason to sequence it as: **credentials → higher-paying role or clean exit → consulting entity.** Which is roughly what the schedule above already does.

---

## Section 7 — Resources

| Area | Primary | Secondary |
|---|---|---|
| AWS SA Pro | Adrian Cantrill SAP-C02 | Tutorials Dojo practice exams |
| AWS DevOps Pro | Stephane Maarek / Cantrill DOP-C02 | Tutorials Dojo, AWS whitepapers |
| CKA / CKAD | KodeKloud (Mumshad Mannambeth) | killer.sh simulator (2 free sessions with exam) |
| CKS | KodeKloud CKS (Kim Wüstkamp) | killer.sh security scenarios |
| KCNA / KCSA | KodeKloud learning path | CNCF glossary + free LF intro courses |
| System design (HLD) | ByteByteGo Vol. 1 & 2 (owned) | Company engineering blogs |
| System design (LLD) | DesignGurus — Grokking series | Head First Design Patterns |
| DSA | NeetCode 150 | LeetCode company-tagged sets |

**On the DesignGurus subscription:** worth it, but for a narrower reason than the transcript gave. ByteByteGo covers high-level design well and you already own it. What you're actually buying is the **coding-pattern taxonomy** (which maps directly onto the Track 1 column above) and the **LLD / class-design modules**, which ByteByteGo genuinely doesn't cover. If you were only doing HLD prep, you could skip it. Since Principal/Director loops at Google and AWS test LLD explicitly, buy it.

---

*Generated 02 Aug 2026 — Week 1, Day 1. Book the SAP-C02 exam before you close this file.*
