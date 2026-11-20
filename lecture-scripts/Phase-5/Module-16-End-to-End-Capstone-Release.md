# Lecture Script: End-to-End Capstone: Release and Presentation

> **Instructor reference.** Phase 5: Deploy, operate and support (Focus area 4), Module 16 | Duration: 9 hours (1.5 h concepts and demos, 7.5 h hands-on), run as Day 23 (1 h, Segments 1–2), Day 24 (4 h, Segments 3–9) and Day 25 (4 h, Segments 10–14) | Previous script: [Module 15 — Production Support and Operations](Module-15-Production-Support-and-Operations.md)

## Session overview

**Goal.** Teams turn 25 days of work into ProcureFlow v1.0: they freeze scope and run a gap analysis against the requirements and NFRs, harden the product, run the final NFT suite, clear security findings, deploy v1.0 to production through the pipeline with a hypercare dashboard and on-call rota, rehearse, and present the whole journey to a panel with evidence for every claim, finishing with the program and AI usage retrospectives and certification (the v1.0 production release, final demo and presentation).

**Learner profile at this point.** Every team has run four sprints and has a release for each (v0.1 to v0.4), and every module's capstone increment from CI-1 to CI-15: a domain model and glossary, a story map and NFRs, a guarded repo, ADRs and C4 diagrams, a platform and template, Next.js screens and BFF, contract-first APIs behind a gateway, a data layer with audit, background and real-time work, three Azure Functions, logs, traces and alerts, a secured pipeline, NFT reports, infrastructure as code with promotion and rollback, and a readiness review, runbook and handover pack. Today introduces the last genuinely new idea of the program: **release: everything built over 25 days becomes one product a real user can use in production, and the team can show the evidence for every claim they make about it.**

**Key outcome.** Every team ships v1.0 to production through its pipeline and presents it: the business problem and users, requirements, guardrails and technology choices with reasons, a live demo across the roles including an escalation and a notification, lifecycle evidence (pipeline, logs, traces, scans, NFT results), support readiness, and an honest AI retrospective. Every learner can explain the decisions behind their part of ProcureFlow and the evidence that supports them.

**Case for this session.** ProcureFlow at Meridian Components. Release v1.0 is Sprint 5's goal: "Finance receives scheduled spend reports; the product runs in production with support in place." **The honest scope question:** the Module 2 story map put purchase orders, goods receipt and invoice matching (PF-005 to PF-008, PF-015 to PF-019) in R1 and R2; the course built requisitions, catalogue, approvals and escalation, notifications, audit and the spend report, and the program's generic feature list also includes a "send back" approval action that ProcureFlow hasn't built. Day 23's gap analysis makes this visible, and teams agree the v1.0 scope with Priya: what ships, what moves to v1.1, and why. That conversation is part of the assessment. "Production" means each team's production environment in the training subscription, India region, with seeded data; no real Meridian data. Roles demonstrated: the program's four roles mapped to Meridian's: **Requester**, **Approver** (line manager, department head, CFO), **Finance** (`FinanceController`), **Administrator** (`Admin`). The trainer plays Priya on Day 23 and leads the panel on Day 25 with co-assessors.

**Note on assessment.** The program's rules (from the syllabus) apply: the capstone project is 35% of the overall grade (team, panel rubric out of 100) and capstone individual contribution 15% (Git history, PR ownership, incident drill role, demo Q&A, peer feedback). Certification needs **70% overall and at least 60% on the capstone**: Distinction 85% and above; Merit 75–84%; Pass 70–74%. Below 70%: a personalised remediation plan and one capstone resubmission within two weeks. The post-assessment (same format as Day 1's pre-assessment, weighted 0%) measures learning gain. Check the program's current assessment guide on the day.

**Note on AI output.** AI is used throughout the final build, under the program's rules of engagement: you own every line you merge; no secrets or customer data in public tools; verify before trusting; an AI disclosure note on every PR; AI tools read the guardrail files; good prompts go in `docs/prompts/`. The AI retrospective draws on those disclosure notes.

## Timing breakdown

| Segment | Duration | Cumulative | Kind |
| --- | --- | --- | --- |
| SEGMENT 1: Opening — v1.0: Scope Freeze and Gap Analysis | 15 min | 0:15 | Concept + demo |
| SEGMENT 2: Lab 1 — Gap Analysis and Scope Freeze With the Sponsor | 45 min | 1:00 | Hands-on |
| **END OF DAY 23** | — | — |  |
| SEGMENT 3: Hardening and the Production Release Plan | 15 min | 1:15 | Concept + demo |
| SEGMENT 4: Lab 2A — Hardening: Close the Gaps | 55 min | 2:10 | Hands-on |
| **BREAK** | 10 min | — |  |
| SEGMENT 5: Lab 2B — Final NFT Run and Security Clean-Up | 55 min | 3:05 | Hands-on |
| SEGMENT 6: Production Deployment and Hypercare | 10 min | 3:15 | Concept + demo |
| SEGMENT 7: Lab 3 — Release v1.0 to Production; Hypercare Setup | 60 min | 4:15 | Hands-on |
| **BREAK** | 10 min | — |  |
| SEGMENT 8: The Final Presentation | 15 min | 4:30 | Concept + demo |
| SEGMENT 9: Lab 4 — Demo Rehearsal | 30 min | 5:00 | Hands-on |
| **END OF DAY 24** | — | — |  |
| SEGMENT 10: Lab 5 — Team Demos and Panel Review | 180 min | 8:00 | Hands-on |
| SEGMENT 11: The AI Usage Retrospective | 15 min | 8:15 | Concept + demo |
| SEGMENT 12: Lab 6 — Program and AI Retrospective Workshop | 25 min | 8:40 | Hands-on |
| SEGMENT 13: Results, Certification and What Comes Next | 10 min | 8:50 | Concept |
| SEGMENT 14: Wrap-Up | 10 min | 9:00 | Close |

Concepts and demos: 15 + 15 + 10 + 15 + 15 + 10 + 10 (close) = 90 min (17%). Hands-on: 45 + 55 + 55 + 60 + 30 + 180 + 25 = 450 min (83%). Total 540 min = 9 h, matching the module's 1.5 h / 7.5 h split. Day 23: Segments 1–2 = 60 min (after Module 15's 3 hours); Day 24: Segments 3–9 = 240 min; Day 25: Segments 10–14 = 240 min. Breaks are not counted; Segment 10 includes short breaks between demos. This module's "CI review" is the panel review in Segment 10, scored with the program's capstone rubric.

## SEGMENT 1: Opening — v1.0: Scope Freeze and Gap Analysis (15 min)

### The Hook (7 min)

**Do:** Play Priya, reading an email from Meridian's CFO.

> *"Priya, the board meets on Friday. Two questions for ProcureFlow. First: is it ready for the March month-end, yes or no, and what does it actually do? I was told it handles purchase orders and invoices. Second: I read that the team used AI. What did the AI do, and how do we know the work is sound?"*

**Say (as Priya):** *"I need to answer both by Friday. Honestly. What do I tell her?"*

**Ask the room:** *"Does ProcureFlow handle purchase orders and invoices today?"*

**Do:** Take 4–5 answers. Let the silence happen.

**Respond to likely answers:**

- *"Mostly."* → *"Which stories, exactly? Show me the story map."*
- *"We built what the modules asked."* → *"True. Does the CFO care what the modules asked?"*
- *"We can build invoices by Friday."* → *"With tests, scans, NFT and a runbook? Or as a demo that breaks in March?"*
- *"We should tell her the truth and a plan."* → *"Yes. That's today's work."*

**Write on board, large, and leave it up all day (and through Day 25):**

> **Done means a real user can do it in production, and you can prove it.**

### Gap Analysis and Scope Freeze (6 min)

**Say:** *"A **gap analysis** compares what was asked for with what exists and works, with evidence. A **scope freeze** is the decision that nothing new enters v1.0 after today: only fixes. Everything not done moves, explicitly, to v1.1, with Priya's agreement. Saying 'not in v1.0' clearly, with a plan, is one of the most valuable things an FDE ever says to a sponsor."*

| Compare against | Source | Evidence that an item is done |
| --- | --- | --- |
| Stories in R1 and R2 | Module 2 story map and backlog | Acceptance criteria pass in an automated test or a recorded demo; deployed |
| Program core features | Syllabus capstone feature list | Working in the staging environment |
| NFRs | `nfr.md` and the Module 13 test plan | Test report with a passing number |
| Mandatory deliverables | Program deliverables list | Link to the artefact |
| Release Definition of Done | Program v1.0 bar: full NFT suite, clean scans, 80% coverage, support sign-off | Pipeline and report links |

**Do (live, 2 of the 6 minutes):** Show the first rows of a gap table and mark them on screen: PF-001 *done* (link to test), PF-005 "Buyer creates a PO" *not built*, "send back" action *not built*.

### Comprehension Check (2 min)

*"A story works on your laptop but has no test and isn't deployed. Done or not done?"* (Not done: done means working in production with evidence.)

**Common misconception:** "scope freeze means we failed to deliver." (It's how professional teams protect quality and keep promises; an explicit, agreed scope beats an implied one that breaks in production.)

**Re-teach trigger:** if teams want to squeeze in features, ask what they'd remove from the NFT run or security clean-up to make room, and whether Priya would agree.

**Transition — Say:** *"Build your gap table with evidence, then agree v1.0's scope with Priya."*

## SEGMENT 2: Lab 1 — Gap Analysis and Scope Freeze With the Sponsor (45 min)

**Say:** *"Compare ProcureFlow with everything it was asked to be, using evidence links. Decide with MoSCoW what v1.0 must have, then present the proposed scope and the v1.1 plan to Priya in five minutes. Her decision freezes scope."*

### Instructions (read aloud, step by step)

1. **Gap table (20 min).** Create `docs/release/v1.0-gap-analysis.md` with one row per R1/R2 story, program core feature, NFR and mandatory deliverable. Status: *done* (evidence link), *partly done* (what's missing), *not built*. No status without a link or a reason.
2. **MoSCoW for v1.0 (10 min).** Must-haves for v1.0 must be done or finishable on Day 24 *with* tests, scans and runbook coverage. Everything else is Should/Could for v1.1. Use Module 2's MoSCoW rules.
3. **Sponsor conversation (10 min).** Five minutes per team with "Priya" (the trainer): what v1.0 does, what it doesn't, why, and the v1.1 plan with rough dates. Write down her decisions.
4. **Freeze (5 min).** Record the frozen scope in `docs/release/v1.0-scope.md`; tag the backlog; create the v1.1 milestone with the deferred stories; update the RAID log for any risk the deferral creates (for example, invoices still processed by email until v1.1).

### Starter: Gap Table

```markdown
# docs/release/v1.0-gap-analysis.md
| Item | Source | Status | Evidence / what's missing | v1.0 decision |
|---|---|---|---|---|
| PF-001 Requester submits a requisition | Story map R1 | Done | Playwright e2e, contract test, prod-like staging | Must |
| PF-005 Buyer creates a PO from an approved requisition | Story map R1 | Not built | — | v1.1 |
| Send back an approval with a reason | Program core features | Not built | Exit-ticket design in Module 14 | Could (v1.1) |
| NFR-PERF-01 | nfr.md | Done | docs/testing/nft/load-140.md (pass) | Must |
| NFR-REC-02 (proposed RTO 1 h) | Module 13 | Partly | Drill took 1 h 20 min; runbook fix pending | Must: re-run drill Day 24 |
```

### How to Play Priya (instructor only)

- Push back once on any deferral ("My CFO was promised invoices"), then accept a clear plan with dates and the interim process (email for invoices until v1.1).
- Reject a scope that includes unfinished features "to show the board"; ask what happens in March if they break.
- Ask one question about risk: "What could stop v1.0 working on the 31st?"
- Close by stating the frozen scope back to them in one sentence each.

### Reference Solution (instructor): A Typical v1.0 Scope

**Example (illustrative — teams' decisions will differ):** v1.0 = catalogue search; drafts, submit and withdraw; multi-level approval with routing at ₹50,000 and ₹5 lakh and self-approval blocked; escalation after 48 hours; notifications; role dashboards with real-time status; audit trail with 8-year retention; nightly spend report and export; SSO with MFA. v1.1 = send back with reason; purchase orders and the SAP push; goods receipt; invoice matching and the on-hold queue. Interim: POs, receipts and invoices continue in the current process, recorded as a RAID risk with Priya as owner.

### Instructor Circulates

- **Status without links:** no link, no "done".
- **Everything marked Must:** ask what happens to quality on Day 24 if it's all Must.
- **Deferred items without dates or interim process:** Priya can't answer her CFO with "later".

**Success check:** gap table with evidence for every row; MoSCoW decisions; Priya's decisions recorded; scope frozen with a v1.1 milestone and RAID entries for deferral risks.

### END OF DAY 23

*Overnight prompt:* *"List the three things most likely to stop your v1.0 working on the 31st. Tomorrow morning, you fix or prove those first."*

## SEGMENT 3: Hardening and the Production Release Plan (15 min)

### What Hardening Is (5 min)

**Say:** *"Hardening is the last sprint's work on quality, not features: closing the gaps the analysis found in frozen scope, re-running the full NFT suite on the release candidate, clearing security findings, and proving the release Definition of Done. No new features. Every change still goes through a PR and the pipeline."*

| Program v1.0 bar | ProcureFlow evidence |
| --- | --- |
| Full NFT suite | Module 13 plan re-run on the release candidate: load at 140 and 300, spike to 420, soak, resilience, restore drill |
| Clean scans | Zero open critical or high in CodeQL, dependencies, images, secrets and ZAP, or dated exceptions (Module 12) |
| 80% coverage on domain and service layers | The quality gate raised from 70% to 80% (Module 13) |
| Support sign-off | Readiness review go, runbook accepted, handover pack (Module 15) |
| Release DoD | Every v1.0 story traces to AC, code, tests and release notes; demonstrated by someone who didn't build it; deployed from main; tagged; `CHANGELOG.md` updated; guardrail `.md` files current; no open critical defects; earlier features still work |

### The Release Plan (6 min)

**Do (live):** Show a release plan for Day 24 on screen.

```markdown
# docs/release/v1.0-release-plan.md
- 09:00 Hardening fixes merged (frozen scope only)
- 10:30 Release candidate v1.0.0-rc.4 promoted to staging (same digests → production)
- 10:30–12:00 Full NFT run on staging (load, spike, soak started); scans; restore drill re-run
- 12:00 Go / no-go (readiness review update; Priya, tech lead, Meridian IT, CISO)
- 12:15 Production: migrations, canary 10% for 15 min with written criteria, then 100%
- 12:45 Post-release verification as each role; hypercare dashboard live; on-call rota active
- Rollback: previous revision to 100% (no schema rollback: expand-only migrations)
```

**Say:** *"Notice three things: the release candidate that passes NFT is the exact digest that goes to production (Module 14); go / no-go happens before production, not after; and the rollback line is written before anyone needs it."*

### Comprehension Check (2 min)

*"A teammate finds a small UI improvement during hardening. Merge it?"* (Not into v1.0 unless it fixes a defect in frozen scope; otherwise it goes to v1.1. Every change after the freeze is risk.)

### Live Demo Note (2 min)

**Do:** Open one team's pipeline and show the path a release-candidate tag takes to production, highlighting the approval gate.

**Common misconception:** "hardening means fixing whatever we have time for." (It's a prioritised list from the gap analysis and the release DoD; each fix has evidence.)

**Re-teach trigger:** if teams start new features, re-read the frozen scope document aloud.

**Transition — Say:** *"Close the gaps that block v1.0."*

## SEGMENT 4: Lab 2A — Hardening: Close the Gaps (55 min)

**Say:** *"Work only from your gap table's Must rows that aren't done, and from the release DoD. Each fix: a PR with a test, through the pipeline. Rotate roles: whoever didn't build a feature verifies it."*

### Instructions (read aloud, step by step)

1. **Plan (5 min).** Order the open Must rows by risk to the 31st (your overnight list first). Assign owners; pair where useful.
2. **Fix (40 min).** PRs with tests and AI disclosure notes; the pipeline promotes each merge to staging. Typical items: a failing NFR from Module 13, coverage below 80% in a domain project, a runbook section not yet accepted, Module 15's postmortem actions marked Must, a restore drill over the RTO.
3. **Cross-verify (10 min).** For each Must story, someone who didn't build it runs its acceptance criteria on staging and records the result in the gap table.

### Instructor Circulates

- **New features appearing:** back to the frozen scope.
- **Fixes without tests:** the release DoD requires them.
- **Builders verifying their own stories:** swap.
- **AI disclosure notes missing on PRs:** they're evidence for Day 25's AI retrospective.

**Success check:** every Must row either done with evidence or explicitly escalated to Priya as a scope change; each Must story verified by a non-builder on staging.

### BREAK (10 min)

*Suggested break prompt:* *"Which NFR result are you least sure will pass on the release candidate? Run that one first."*

## SEGMENT 5: Lab 2B — Final NFT Run and Security Clean-Up (55 min)

**Say:** *"Tag the release candidate, promote it to staging, and run the full NFT suite and the security scans against exactly that build. Clear what you can; document what you can't, with dates."*

### Instructions (read aloud, step by step)

1. **Release candidate (5 min).** Tag `v1.0.0-rc.N` on main; the pipeline promotes its digests to staging. Record the digests in `docs/release/v1.0-release-plan.md`.
2. **NFT run (30 min, mostly running).** Using the Module 13 plan and the shared runner schedule: 140-VU load (NFR-PERF-01), 300-VU headroom, spike to 420; start a soak (read it in Lab 3); resilience checks for the database outage; re-run the restore drill if it failed the RTO. Record each result against its pass criterion in `docs/release/v1.0-nft-results.md`.
3. **Security clean-up (15 min, in parallel).** Pipeline scans on the release candidate: CodeQL, dependencies, Gitleaks, Trivy, ZAP. Fix what you can (dependency and base-image updates first). Everything else: a dated exception in `SECURITY.md` with a compensating control, owner and expiry, or it blocks the release.
4. **Readiness update (5 min).** Update the Module 15 readiness review with today's evidence links.

### Reference Results (instructor debrief)

**Example (illustrative — your environment decides):** most teams pass NFR-PERF-01 at 140 and the read target at 300 after Module 13's fixes; the spike shows a short p95 rise during scale-out that recovers within the criterion; one team finds a dependency vulnerability published that week and fixes it by updating; one team's restore drill passes the RTO after Module 15's runbook improvements.

**Say:** *"These results are your slides tomorrow. A panel trusts 'p95 1.3 s at 140 users, report linked' far more than 'it's fast'."*

### Instructor Circulates

- **NFT run against a different build from the release candidate:** results don't count; run against the candidate's digests.
- **Scans "cleared" by suppressing rules:** exceptions need reasons, owners and dates.
- **Teams overlapping on shared resources:** keep to the runner schedule.

**Success check:** release candidate on staging with recorded digests; every planned NFT result recorded against its criterion; zero open critical or high findings, or dated exceptions; readiness review updated.

## SEGMENT 6: Production Deployment and Hypercare (10 min)

### Go / No-Go, Then Release (5 min)

**Say:** *"At go / no-go, four people answer one question each: the tech lead (is the evidence complete?), Priya (does the business accept the scope and the exceptions?), Meridian IT (is support ready?), the CISO (are the security exceptions acceptable?). Any no means no. Then the release follows the plan: migrations, canary with written criteria, full rollout, verification as each role."*

### Hypercare Setup (3 min)

| Item | What | Source |
| --- | --- | --- |
| Hypercare dashboard | SLO burn, RED per service, notifications backlog, dead letters, SQL pool, sign-in failures, security events | Modules 11 and 12 |
| On-call rota | Name, backup and contact per day; Sev 1 acknowledged in 15 minutes | Module 15 |
| Daily check-in | 15 minutes with Priya's team: incidents, feedback, actions | Module 15 |
| Exit criteria | For example: one month-end passed, no Sev 1 or 2 for two weeks, L2 resolving without the FDE team | Module 15 |

### Comprehension Check (2 min)

*"The canary's error rate is slightly higher than the old revision's, but within your written criterion. Promote?"* (Yes, by the criteria agreed in advance, while watching closely; changing criteria mid-release is how bad releases get through, or good ones get blocked.)

**Common misconception:** "the release is done when the pipeline is green." (It's done when users can work, verified as each role, with hypercare watching.)

**Re-teach trigger:** if a team can't name its rollback step, stop and write it before releasing.

**Transition — Say:** *"Release v1.0."*

## SEGMENT 7: Lab 3 — Release v1.0 to Production; Hypercare Setup (60 min)

**Say:** *"Hold your go / no-go with the trainer as the panel of four. If it's a go, deploy the release candidate's digests to production through the pipeline, canary first, then verify as every role, and switch on hypercare. If it's a no-go, write exactly why and what would turn it into a go; that's a legitimate outcome and it will be assessed as one."*

### Instructions (read aloud, step by step)

1. **Read the soak (10 min).** Check the soak started in Lab 2B (signals flat or explained, Module 13's method). Add it to the NFT results.
2. **Go / no-go (10 min).** Present the updated readiness review in three minutes; the trainer answers as the tech lead's reviewer, Priya, Meridian IT and the CISO. Record the decision and conditions.
3. **Release (20 min).** Approve the production deployment in the pipeline; migrations run; canary at 10% with your written criteria for 15 minutes; promote to 100% (or roll back by the criteria). Tag `v1.0.0` on the release commit; publish release notes and `CHANGELOG.md`.
4. **Verify as every role (10 min).** In production: requester submits; approver approves (and a self-approval attempt is refused); finance opens the spend report; admin updates a catalogue item. Notification arrives. Record each with a screenshot or trace ID in `docs/release/v1.0-verification.md`.
5. **Hypercare on (10 min).** Pin the hypercare dashboard; publish the on-call rota; confirm one alert reaches the on-call person by test-firing it; schedule the first daily check-in.

**Note:** production uses seeded data in the training subscription. The escalation (48 hours) can't be shown live in production today; tomorrow's demo shows it in staging with a short timeout, and says so.

### Instructor Circulates

- **Deploying a new build instead of the candidate's digests:** stop; promote the tested digests.
- **Canary skipped "to save time":** it's the release plan; follow it.
- **Verification by the person who built each feature:** rotate.
- **On-call rota without contacts or backups:** complete it.

**Success check:** go / no-go recorded; v1.0 in production from the release candidate's digests via canary; tagged with release notes; verified as every role with evidence; hypercare dashboard, rota and alert routing live. (Or: a documented no-go with conditions and a plan.)

### BREAK (10 min)

*Suggested break prompt:* *"If the CFO watched only five minutes of your demo, which five minutes would answer her two questions?"*

## SEGMENT 8: The Final Presentation (15 min)

### The Structure (6 min)

**Say:** *"Twenty minutes per team, then ten minutes of panel questions. The structure is fixed by the program, so the panel can compare teams fairly."*

| Part | Minutes | Content | Evidence to show |
| --- | --- | --- | --- |
| 1. Business domain, users and the problem solved | 3 | Meridian's procurement pain (nine-day approvals, month-end chaos), the users and volumes | Glossary, user profile, story map (Modules 1–2) |
| 2. Requirements, guardrails and technology choices with reasons | 4 | NFRs; guardrail set; the stack chosen from the user profile, and the decisions you'd defend | `nfr.md`, `.md` guardrails, decision matrix, key ADRs (Modules 2–5, 7, 10, 12, 14) |
| 3. Live demo across all four roles, including an escalation and a notification | 5 | Requester, approver, finance, admin; escalation (staging, short timeout, said aloud); a notification | The product, live |
| 4. Lifecycle evidence | 4 | Pipeline run, a trace, a dashboard, scan results, NFT results | Links and screenshots (Modules 11–14) |
| 5. Production support readiness | 2 | Runbook, alerts, the game-day incident and postmortem | Module 15 records |
| 6. AI retrospective and lessons learned | 2 | What AI did well, badly, and how you verified it | Disclosure notes, checkpoint results |

### What Makes It Work (6 min)

**Say:** *"Five habits. One: tell Meridian's story, not the module list. Start with Priya's problem and come back to it. Two: every claim has its evidence on the slide: a number, a link, a screenshot. Three: rehearse the demo path and have a recorded backup, because live demos fail; say so if you switch. Four: everyone speaks, and everyone can answer questions about any part. Five: be honest about scope and limits; the panel rewards 'here's what we deferred and why' over hand-waving."*

**Do (live, 2 of the 6 minutes):** Show one strong slide (a claim with its number and link: "Month-end: p95 1.3 s at 140 users (target 2 s), report →") next to a weak one ("High performance and scalability").

### Comprehension Check (3 min)

*"The panel asks 'Why Container Apps and not AKS?'. What's a strong answer?"* (The reasoning and evidence: Module 4's matrix and user profile, team size and operational load, ADR-0012 superseding ADR-0004 with what changed, cost from the calculator, and the "revisit if" conditions.)

**Common misconception:** "the demo is the presentation." (The demo is five of twenty minutes; evidence and reasoning carry most of the score.)

**Re-teach trigger:** if a team's outline is a module-by-module tour, rebuild it around the six parts with them.

**Transition — Say:** *"Rehearse it, timed, with feedback."*

## SEGMENT 9: Lab 4 — Demo Rehearsal (30 min)

**Say:** *"Pair with another team. Each team runs its presentation once, timed, to the other, which plays the panel with the rubric. Then fix the three most important things."*

### Instructions (read aloud, step by step)

1. **Run (20 min, 10 per team).** Run the first three parts and the AI retrospective in full; summarise parts 4 and 5 in one minute each. The other team times each part and notes rubric gaps.
2. **Feedback and fixes (10 min).** Swap three pieces of feedback each way. Fix the top three: usually timing, a claim without evidence, and the demo path.

### Rehearsal Checklist (for the listening team)

- [ ] Starts with Meridian's problem, returns to it at the end
- [ ] Every claim shows a number, link or screenshot
- [ ] Demo covers requester, approver, finance and admin, an escalation and a notification; backup recording ready
- [ ] Scope and deferrals stated honestly
- [ ] Everyone speaks; any member can answer about any part
- [ ] Within 20 minutes

**Success check:** both teams rehearsed, timed, with three fixes each recorded.

### END OF DAY 24

*Overnight prompt:* *"Write down the one question you hope the panel doesn't ask, then prepare the answer."*

## SEGMENT 10: Lab 5 — Team Demos and Panel Review (180 min)

**Say:** *"Each team: 20 minutes, then 10 minutes of panel questions. The panel scores with the capstone rubric. Everyone else watches and writes one specific piece of praise and one question per team for the peer feedback."*

### Running Order (instructor)

- Up to six teams in 180 minutes (6 × 30). With fewer teams, use the time for a five-minute break after every second team and a longer Q&A. With more than six, run two panels in parallel with a calibration session beforehand.
- Before the first team: the panel reads each team's gap analysis, readiness review and v1.0 verification record.
- The trainer keeps time strictly: a warning at 18 minutes, a stop at 20.

### Capstone Rubric (team score out of 100; panel)

| Criterion | Focus area | Points | What "excellent" looks like |
| --- | --- | --- | --- |
| Business and requirements | 1 | 15 | Domain language used consistently; stories trace to code, tests and releases; NFRs measurable |
| Guardrails | 1 | 5 | `.md` set complete, current and enforced by the pipeline |
| Technology selection and framework | 2 | 15 | Choices justified by the user profile and cost; platform reused across services; dependencies clean and tracked |
| Implementation, server-side and serverless | 2, 3 | 15 | All v1.0 features work across the four roles; functions reliable under retry and failure |
| Logs and observability | 5 | 10 | Root cause of an injected fault found from logs and traces within 15 minutes |
| Security | 5 | 10 | MFA, RBAC and policy checks correct; zero critical or high findings |
| Non-functional testing | 5 | 10 | Every NFR tested with evidence; bottlenecks found and fixed |
| Deployment and support | 4 | 10 | Production deployment via pipeline with rollback; runbook works for another team |
| Presentation and AI retrospective | All | 10 | Clear story, confident Q&A, honest analysis of AI use |

**Panel notes:**

- **Logs and observability** (10 points) is assessed live: during the team's Q&A, or in a 15-minute slot scheduled the day before, the panel injects a fault in the team's staging environment and the team finds the root cause from logs and traces.
- **Scope honesty** counts under "Business and requirements": a clear, agreed, evidenced v1.0 scope with a v1.1 plan scores higher than an over-claimed one.
- Individual contribution (15% of the overall grade) is scored separately from Git history, PR ownership, incident-drill role, demo Q&A and peer feedback.

### Panel Question Bank (instructor)

- *"Show me the evidence for NFR-PERF-01. What would make it fail next March?"*
- *"Walk me through what happens when Service Bus is unavailable for ten minutes at month-end."*
- *"Why is the audit retention 8 years when the program says 7?"*
- *"Which AI-generated code did you reject, and how did you know?"*
- *"A requester changes the number in the URL. What happens, and where's the test?"*
- *"Your team leaves tomorrow. Who fixes the next Sev 2, and with what?"*
- *"What did you defer to v1.1, and what is Meridian doing in the meantime?"*

**Success check:** every team presented within time and answered panel questions; rubric scores and written feedback recorded for each team; peer feedback collected.

## SEGMENT 11: The AI Usage Retrospective (15 min)

### What the Evidence Says (10 min)

**Say:** *"Twenty-five days of AI-assisted engineering, with a verification checkpoint in every module. Let's read the evidence, not our impressions."*

| Module | AI task | Verification checkpoint | What we learned |
| --- | --- | --- | --- |
| 1–2 | Glossary, stories, NFR questions | Sponsor's language; INVEST | AI replaced Priya's words with textbook words |
| 3 | Guardrail drafts; prompt clinic | With and without guardrail files | Instruction files measurably changed output |
| 4 | Stack recommendation | Weighted matrix | AI defaulted to fashionable stacks |
| 5–6 | Platform code; components and stories | Architecture tests; axe and keyboard tests | AI suggested packages with licence problems; skipped labels |
| 7 | OpenAPI and controllers | Contract tests; zero analyser warnings | AI used verbs in URLs, forgot 409s |
| 8 | Schema, seed, tuning | Execution plan before and after | AI suggested `NOLOCK` and wrong index column order |
| 9 | Thread-safety review | Load run | AI missed the per-user cache leak; flagged safe code |
| 10 | Functions and orchestrations | Local retry and poison behaviour | AI broke orchestrator determinism rules |
| 11 | Incident hypothesis | Log and trace evidence | AI chased the Redis decoy |
| 12 | Threat model and code review | Scans and tests | AI gave generic threats; one false positive |
| 13 | Unit tests and k6 scripts | Mutation score and real load | AI tests ran code but checked little |
| 14–15 | Pipelines, IaC, runbooks | What-if, scans; another team runs the runbook | AI invented resource names and flags |

**Say:** *"The pattern across every row: AI was fast and fluent, and wrong in ways only verification caught. The value came from the checkpoints. That's the answer to the CFO's second question: 'AI drafted, people verified with tests, scans, plans and runs, and here's the evidence.'"*

### Live Demo: A Disclosure Note Search (3 min)

**Do:** Search one team's merged PRs for AI disclosure notes and count: how many PRs used AI, how many notes say something was changed after review, which checks caught issues.

### Comprehension Check (2 min)

*"What's the single most useful rule of engagement from the whole program?"* (Expect: verify before trusting, or you own every line you merge; accept any rule backed by an example.)

**Common misconception:** "AI made the work faster, so it made the work better." (It made drafts faster; quality came from the reviews and checkpoints.)

**Re-teach trigger:** if a team can't name a verification that caught an AI error, open their PR history together and find one.

**Transition — Say:** *"Now your team's own retrospective, on the program and on AI."*

## SEGMENT 12: Lab 6 — Program and AI Retrospective Workshop (25 min)

**Say:** *"In teams, run a retrospective on the whole program using the 4Ls (liked, learned, lacked, longed for), then on AI use: keep, change, stop. Then one person per team shares the single most useful insight with the room."*

### Instructions (read aloud, step by step)

1. **4Ls on the program (10 min).** Silent writing for three minutes, then group and discuss.
2. **AI: keep, change, stop (8 min).** Grounded in your disclosure notes and checkpoint results; update `docs/prompts/` with the prompts you'd keep, and `docs/ai-usage-policy.md` with one change you'd make.
3. **Share (7 min).** One insight per team, one minute each.

**Success check:** a team retrospective record in `docs/retrospectives/program.md`; prompt library and AI usage policy updated; one insight shared.

## SEGMENT 13: Results, Certification and What Comes Next (10 min)

### Results and Certification (5 min)

**Say:** *"Assessment combined practical evidence (85%) and knowledge checks (15%). To be certified you need 70% overall and at least 60% on the capstone."*

| Level | Overall score |
| --- | --- |
| Distinction | 85% and above |
| Merit | 75–84% |
| Pass | 70–74% |
| Below 70% | A personalised remediation plan and one capstone resubmission within two weeks |

**Do:** Announce results as the program's process specifies (individually or as a list, as agreed), and the post-assessment arrangements (same format as Day 1; used to measure learning gain).

### What Comes Next (3 min)

**Say:** *"For ProcureFlow: hypercare continues through the first month-end, with the rota you published yesterday; v1.1's backlog is agreed with Priya. For you: the habits matter more than the tools. Start every engagement with the business and the users. Write the rules down before the code. Turn every requirement into a test with a number. Leave every system so someone else can run it."*

### Comprehension Check (2 min)

*"Which ProcureFlow artefact would you show a future client first, and why?"* (Any artefact with a reason tied to their concerns: the gap analysis and scope conversation, the NFR test plan, the incident record, the handover pack.)

**Common misconception:** "certification is the end of the work." (ProcureFlow is in hypercare and v1.1 is planned; certification marks the method learned, not the product finished.)

**Re-teach trigger:** if learners can't link an artefact to a client concern, pair them for two minutes to rehearse a 30-second explanation of one artefact.

## SEGMENT 14: Wrap-Up (10 min)

### v1.0 Release Checklist (final confirmation, 3 min)

**Say:** *"One last look at what each team shipped."*

- [ ] Scope frozen and agreed with Priya; gap analysis with evidence; v1.1 plan and interim process recorded
- [ ] Release candidate passed the full NFT suite; zero open critical or high findings, or dated exceptions
- [ ] Coverage at 80% on domain and service layers; quality gate passing on main
- [ ] Readiness review go (or documented no-go with conditions); production deployment from the candidate's digests via canary; rollback ready
- [ ] v1.0 tagged with release notes and `CHANGELOG.md`; verified as every role
- [ ] Hypercare dashboard, on-call rota and alert routing live; runbook accepted; handover pack complete
- [ ] Final presentation delivered; AI retrospective and program retrospective recorded

### What We Covered (2 min)

- Gap analysis with evidence; scope freeze and an honest sponsor conversation
- Hardening against the release Definition of Done
- The final NFT run and security clean-up on the exact release candidate
- Go / no-go; production release via canary; verification as every role; hypercare
- Presenting a product with evidence; the AI usage retrospective; the program retrospective

### Back to the Opening (2 min)

**Read the CFO's two questions again.**

(Expected: 1. Ready for March, and what does it do? → Yes for the agreed v1.0 scope, with NFT evidence at month-end load and a hypercare plan through the first month-end; purchase orders and invoices are in v1.1 with dates and an interim process. 2. What did AI do, and how do we know the work is sound? → AI drafted code, tests, scripts and documents; every module verified them with contract tests, execution plans, load runs, scans, mutation scores and other teams running runbooks; the disclosure notes and results are the evidence.)

**Point at the board:** *"Done means a real user can do it in production, and you can prove it. Every team can now say both halves."*

### Exit Ticket (2 min, written, one per learner)

> *"Name one habit from this program you'll use in your next client engagement, and the first situation where you'll use it."*

**Expected answer:** any specific habit tied to a specific situation, for example: "Turn the first NFR the client mentions into a number and a test in the first week", or "Write the runbook section the day I add an alert". Vague answers ("communicate better") prompt a follow-up conversation.

**Re-teach trigger:** for any learner who can't name one, the trainer follows up individually as part of their remediation or development plan.

### Bridge to What Comes Next (1 min)

**Say:** *"Twenty-five days ago, ProcureFlow was a sponsor's frustration and nine-day approvals. Today it's in production, with evidence, a support model and a plan. The next client won't be Meridian, and the stack may be different. The method travels: business first, rules written down, every requirement a test, every release reversible, every system runnable by someone else. Thank you."*

### Homework / Self-Practice

1. **Hypercare handover note.** For whoever continues hypercare (Meridian's L2 or the next FDE team): open incidents, alerts seen in the first days, actions in flight, and the date of the first month-end check-in.
2. **v1.1 backlog with Priya.** Turn the deferred scope into prioritised, estimated stories with acceptance criteria (Module 2's practices), linked to the interim-process risk in the RAID log.
3. **Personal case study.** One to two pages for your portfolio: a decision you made on ProcureFlow, the evidence behind it, what happened, and what you'd do differently. No client-confidential data.

## Q&A & Doubt Solving

**Likely questions and suggested answers:**

**Q: Will deferring purchase orders and invoices cost us marks?** → An honest, agreed, evidenced scope with a v1.1 plan scores better than over-claiming. The rubric rewards working features across the roles, traceability and honesty; unfinished features presented as done lose marks under several criteria.

**Q: What if our go / no-go is a no-go?** → Record why, what would turn it into a go, and when. Present it as a professional decision. A no-go with a clear plan is assessed on its quality; a go on missing evidence is a much bigger problem.

**Q: Can we show the escalation in production?** → Not live: it takes 48 hours. Show it in staging with a short timeout, and say clearly that production uses 48 hours, pointing to the configuration and the test.

**Q: Our live demo failed during the presentation. What now?** → Switch to the recorded backup, say so plainly, and keep going. The panel assesses the product and the evidence, not luck with Wi-Fi.

**Q: How is individual contribution assessed in a team project?** → From Git history and PR ownership, your role in the incident drill, how you answer panel questions, and peer feedback. Make sure your contributions are visible in PRs and records.

**Q: Does ProcureFlow keep running after the program?** → In the training subscription, only as long as the program says; production environments will be scaled down or deleted afterwards to control cost. Export anything you need for your portfolio, without client-confidential data.

**Q: What should we do with the AI prompts we collected?** → Keep the ones that worked, with notes on why, in `docs/prompts/`. They're a reusable asset for the next engagement, as long as they contain no client data.

## Instructor Notes

- **Before Day 23:** confirm every team's production environment exists (training subscription, India region), the pipeline's production approval is configured with you as an approver, and the shared k6 runner schedule for Day 24.
- **Play Priya consistently** across Day 23's scope conversations: push back once, accept clear plans, reject unfinished features "for the board".
- **Assemble the panel:** at least two assessors plus the trainer; calibrate on the rubric with a sample presentation or the previous cohort's recording; prepare the fault-injection slot for the logs-and-observability criterion.
- **Day 24 is tight:** keep the release plan's timings visible; if a team is behind at 12:00, hold go / no-go anyway and accept a documented no-go rather than a rushed release.
- **Cost:** after the program, scale down or delete team environments as the program's policy requires; tell teams beforehand so they can capture portfolio evidence.
- **Certification:** follow the program's process for announcing results and remediation plans; check the current assessment guide.
- **Common mistake:** new features during hardening.
- **Common mistake:** NFT or scans run against a build other than the release candidate.
- **Common mistake:** presentations organised as a module tour instead of Meridian's story.
- **Engagement tip:** the strongest moments are the scope conversation with Priya, the production canary going to 100%, and each team's live root-cause find. Pause for each.
- **Materials to prepare:** the CFO's email; gap analysis, scope, release plan, NFT results and verification templates; the rubric sheets; the panel question bank; the presentation timer; the retrospective boards.

## Common Mistakes — Quick Reference

| Mistake | Symptom | Fix |
| --- | --- | --- |
| Status without evidence in the gap analysis | Surprises on Day 24 | Link or "not done" |
| Over-claiming scope to the sponsor | Broken promises in March | Agreed v1.0 scope, v1.1 plan, interim process |
| New features after the freeze | Untested code in v1.0 | Frozen scope; changes only for defects |
| Testing a different build from the one released | Evidence doesn't apply | NFT and scans on the release candidate's digests |
| Exceptions without dates | Permanent "temporary" risks | Owner, compensating control, expiry |
| Releasing without go / no-go | No shared decision | Four answers, recorded |
| Skipping the canary under time pressure | Full blast radius for a bad release | Follow the release plan |
| Verifying your own features | Blind spots | Non-builder verification |
| Module-tour presentations | Panel can't see the product or the reasoning | Six-part structure around Meridian's story |
| Claims without numbers | Low credibility | Evidence on every slide |
| AI retrospective as opinion | No learning | Disclosure notes and checkpoint results |

## Timing Contingencies

| Situation | Action |
| --- | --- |
| Gap analysis takes too long | Limit to R1/R2 stories and NFRs; deliverables checked on Day 24 morning |
| A team can't reach go by 12:00 on Day 24 | Hold go / no-go anyway; a documented no-go is acceptable and assessed |
| Production deployment fails | Roll back per plan; diagnose with Module 14's table; retry once; otherwise document |
| NFT runner overloaded | Prioritise NFR-PERF-01 and the spike; soak and resilience use Module 13's results with justification |
| More than six teams | Two panels in parallel after calibration |
| Demo overruns | Strict 20-minute stop; Q&A continues to 10 minutes |
| Retrospective time squeezed | Run the AI keep/change/stop only; program 4Ls as a shared online board afterwards |

## End-of-Session Quiz (5 Questions)

1. What makes an item "done" in the gap analysis, and why does a feature that works only on a laptop not count?
2. List four parts of the v1.0 release Definition of Done and the evidence for each.
3. Explain why the NFT run and the scans must be performed on the release candidate's digests, and what would be wrong with re-running them on a fresh build.
4. Describe the six parts of the final presentation and the evidence that best supports each.
5. Summarise, with two concrete examples from the program, how ProcureFlow's teams used AI and how they verified its output.

**Answer key (instructor):**

1. Done means it works in production (or staging for the release candidate) for real users, with acceptance criteria passing in a test or recorded demo, and evidence linked. A laptop-only feature hasn't passed the pipeline, tests, scans or deployment, so nothing proves it works for users.
2. Any four: every story traces to acceptance criteria, code, tests and release notes (traceability matrix and links); demonstrated by a non-builder (verification record); deployed from main through the pipeline, tagged, `CHANGELOG.md` updated (pipeline run, tag); guardrail `.md` files current (repo); no open critical defects (backlog query); earlier features still work (regression suites); full NFT suite, clean scans, 80% coverage, support sign-off (reports, gate, readiness review).
3. The evidence must describe exactly what users will run. A fresh build can differ (base images, packages, build environment), so its results wouldn't prove anything about the released digests; build once, promote by digest (Module 14).
4. Domain, users and problem (glossary, user profile, story map); requirements, guardrails and technology choices (NFRs, guardrail files, decision matrix, ADRs); live demo across four roles with escalation and notification (the product); lifecycle evidence (pipeline run, trace, dashboard, scans, NFT reports); support readiness (runbook, alerts, incident and postmortem records); AI retrospective (disclosure notes, checkpoint results).
5. AI drafted artefacts in every module; teams verified them with a defined checkpoint. Examples: Module 7's AI-generated OpenAPI specs and controllers checked by contract tests; Module 8's tuning advice checked against execution plans (rejecting `NOLOCK`); Module 9's thread-safety claims proved or disproved by load runs; Module 13's AI unit tests measured by mutation score; Module 15's AI-drafted runbooks accepted only after another team executed them.

## Homework Rubric

| Criterion | Excellent (4) | Good (3) | Needs Work (2) | Incomplete (1) |
| --- | --- | --- | --- | --- |
| Hypercare handover note | Open incidents, alerts seen, actions in flight with owners, and the first month-end check-in date | Complete; owners or dates missing | List without context | Not attempted |
| v1.1 backlog | Deferred scope as INVEST stories with acceptance criteria, estimates and priorities agreed with Priya; linked to the RAID risk | Stories present; estimates or priorities thin | Titles only | Not attempted |
| Personal case study | A real decision, its evidence, the outcome and a reflective change; no confidential data | Clear but thin on evidence or reflection | Describes work without a decision or evidence | Not attempted |

**Total:** /12. Pass threshold: 8/12.

## Materials Checklist

- [ ] The CFO's email (Day 23 opening and Day 25 close)
- [ ] Templates: gap analysis, v1.0 scope, release plan, NFT results, verification record, retrospective board
- [ ] Production environments per team, with the pipeline's production approval configured
- [ ] Shared k6 runner schedule for Day 24
- [ ] Panel: assessors confirmed, rubric sheets, calibration session, fault-injection slots, question bank
- [ ] Presentation timer and running order
- [ ] Results and certification process confirmed with the program office; post-assessment arranged
- [ ] Environment clean-up plan announced to teams
- [ ] AI assistant access tested
