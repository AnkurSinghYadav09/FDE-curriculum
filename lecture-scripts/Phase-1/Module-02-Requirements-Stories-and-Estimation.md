# Lecture Script: Requirements Gathering, User Stories and Estimation

> **Instructor reference.** Phase 1 (Focus area 1), Module 2 | Duration: 5 hours (2 h concepts and demos, 3 h hands-on) | Previous script: [Module 1 — The FDE Role and Business Domain Understanding](Module-01-FDE-Role-and-Business-Domain.md)

## Session overview

**Goal.** Learners turn the ProcureFlow domain model from Module 1 into a buildable plan: a story map sliced into five releases, 20 user stories with testable acceptance criteria, measurable non-functional requirements, and an estimated, prioritised backlog with an agreed Sprint 1 goal (Capstone increment CI-2).

**Learner profile at this point.** Learners have completed Module 1. They have a domain glossary, an event-storming map, five bounded contexts (Requisitioning, Purchasing, Receiving, Accounts Payable, Supplier Management), a user volume profile, and a list of open questions, all committed under `docs/domain/`. Everything so far has been about **understanding**. Today introduces the first genuinely new idea of the build phase: **turning understanding into small, testable, traceable commitments.**

**Key outcome.** Every learner can choose a delivery model for a client situation, write a user story that passes INVEST with Given/When/Then criteria, turn "it should be fast" into a measurable NFR, trace a story to code, test and release, and estimate and prioritise a backlog with planning poker and MoSCoW.

**Case for this session.** ProcureFlow at Meridian Components, continued. The trainer again plays **Priya Nair, Head of Finance and sponsor**. In this module she also acts as **Product Owner** (in a real engagement she would nominate one). Numbers stay consistent with the Module 1 fact card: 1,377 users, 3,000 requisitions and 4,500 invoices a month, 40% of invoices in the last five working days, +30% growth next year.

**Note on AI use.** Labs 2 and 3 use an AI assistant to draft stories and generate NFR questions. Its output varies run to run; examples below are marked **illustrative**. The skill being taught is refining the draft against INVEST and the sponsor's words, not accepting it.

## Timing breakdown

| Segment | Duration | Cumulative | Kind |
| --- | --- | --- | --- |
| SEGMENT 1: Opening — From Understanding to Commitments; SDLC Models | 18 min | 0:18 | Concept |
| SEGMENT 2: Agile, Scrum and Kanban; Definition of Ready and Done | 14 min | 0:32 | Concept |
| SEGMENT 3: Elicitation, Conflicts and Story Mapping | 25 min | 0:57 | Concept + demo |
| **BREAK** | 10 min | — |  |
| SEGMENT 4: Lab 1 — Story Map and Five Releases | 40 min | 1:37 | Hands-on |
| SEGMENT 5: Writing User Stories That Pass INVEST | 25 min | 2:02 | Concept + demo |
| SEGMENT 6: Lab 2 — Twenty User Stories (AI-assisted) | 50 min | 2:52 | Hands-on |
| **BREAK** | 10 min | — |  |
| SEGMENT 7: Measurable NFRs and Traceability | 20 min | 3:12 | Concept + demo |
| SEGMENT 8: Lab 3 — NFRs With the Sponsor (AI-assisted) | 35 min | 3:47 | Hands-on |
| SEGMENT 9: Estimation and MoSCoW | 13 min | 4:00 | Concept + demo |
| SEGMENT 10: Lab 4 — Planning Poker, MoSCoW, Sprint 1 Goal | 40 min | 4:40 | Hands-on |
| SEGMENT 11: CI-2 Review, Summary and Wrap-Up | 20 min | 5:00 | Hands-on + close |

Concepts and demos: 120 min (40%, including the 5-minute close). Labs and capstone: 180 min (60%). Breaks are not counted.

## SEGMENT 1: Opening — From Understanding to Commitments; SDLC Models (18 min)

### The Hook (6 min)

**Show one slide: Priya's email reply to your CI-1 submission.**

> *"Thank you, this is the first time anyone has written down how we actually work. Two questions. When can my AP team use something? And how will I know it does what we agreed?"*

**Ask:** *"You have a glossary, an event-storming wall and a user profile. What do you reply to Priya?"*

**Do:** Take 4–5 answers. Write them on the board.

**Respond to likely answers:**

- *"About six months."* → *"Based on what? Which six months of work, and what does she get in month one?"*
- *"We'll build the whole thing and demo it."* → *"How will she know it does what was agreed? Agreed where?"*
- *"We need to write requirements first."* → *"Good. What would one requirement look like? Say one aloud."*

Someone will say something like *"The system should be fast and easy to use."*

**Say:** *"Great. Priya, how will you test 'fast'? You can't. Neither can we. Here's the line for today:"*

**Write on board, large, and leave it up all day:**

> **A requirement you can't test is just a wish.**

**Say:** *"Today we turn Module 1's understanding into commitments that are small, testable and traceable: a story map, user stories, measurable NFRs, and an estimated backlog. By the end, you'll agree a real Sprint 1 goal with Priya."*

### SDLC Models: Where Each Still Fits (8 min)

**Say:** *"Before we write a single story, we need to decide how work will flow. You'll hear people say 'waterfall is dead'. On client engagements, it isn't. Each model still fits somewhere."*

**Draw on whiteboard:**

| Model | How it works | Still fits when… | ProcureFlow example |
| --- | --- | --- | --- |
| Waterfall | Phases in order: requirements → design → build → test → release | Scope is fixed, change is expensive, sign-offs are contractual | SAP interface change: IT only allows changes in a fixed window around the March upgrade |
| Iterative | Build a rough version, learn, rebuild it better | The solution shape is unclear and needs prototypes | Trying three approval screens with approvers before choosing one |
| Agile (Scrum, Kanban) | Small increments, frequent feedback, re-prioritise often | Requirements will change and the sponsor is available | The ProcureFlow app itself: Priya reviews every two weeks |

**Say:** *"On real engagements you'll almost always run a **hybrid**: Agile for the app, plus fixed, plan-driven milestones for the things the client controls, like the SAP change window and the March year-end. Your job is to know which piece runs which way, and say so out loud."*

### Comprehension Check (4 min)

1. *"Meridian's IT team accepts SAP interface changes only in a two-week window after the March upgrade. Which model fits that piece of work, and why?"* (A plan-driven, waterfall-style piece: the date and scope are fixed by IT, so design and test it ahead of the window, inside an otherwise Agile project.)
2. *"Why is 'waterfall is always wrong' a risky belief for an FDE?"* (Clients have fixed constraints: contracts, audits, change windows. Ignoring them means missing a date you never controlled.)

**Common misconception:** Agile means no planning or no documentation.

**Re-teach trigger:** if most say "Agile for everything", ask: *"Can you iterate on a change window that opens once a year?"*

**Transition — Say:** *"We'll run the app the Agile way. So let's get the Agile vocabulary straight."*

## SEGMENT 2: Agile, Scrum and Kanban; Definition of Ready and Done (14 min)

### Scrum in Five Minutes (6 min)

**Say:** *"Scrum is the most common Agile framework you'll meet at clients. It has three roles, five events, and three artefacts. Here's each one, mapped to ProcureFlow."*

**Draw on whiteboard:**

|  | Scrum term | What it means | ProcureFlow |
| --- | --- | --- | --- |
| Roles | Product Owner | Owns the backlog order; decides what's next | Priya (in real life, her nominee) |
|  | Scrum Master | Removes blockers; protects the process | Your team lead |
|  | Developers | Build and test the increment | You |
| Events | Sprint | Fixed time box, usually 2 weeks | 2-week sprints |
|  | Sprint Planning | Pick stories and agree a sprint goal | Lab 4 today |
|  | Daily Scrum | 15 min: progress and blockers | Every morning |
|  | Sprint Review | Demo the increment to stakeholders | Priya and an AP clerk attend |
|  | Retrospective | Improve how the team works | Last hour of the sprint |
| Artefacts | Product Backlog | Ordered list of everything wanted | Your 20 stories (Lab 2) |
|  | Sprint Backlog | Stories chosen for this sprint + plan | Your Sprint 1 (Lab 4) |
|  | Increment | Working, usable software | First demoable slice |

**Say:** *"Backlog refinement isn't an official event, but every good team does it: a regular session to split, clarify and estimate upcoming stories. Today's labs are one big refinement session."*

### Kanban Flow (2 min)

**Say:** *"Kanban doesn't use sprints. Work flows across a board: To do → In progress → Review → Done. The key rule is a **WIP limit**: no more than, say, three items in progress at once. You measure **cycle time**: how long one item takes from start to done."*

**Say:** *"Where does Kanban fit on a client engagement? After go-live. Support tickets arrive unpredictably; you can't plan them two weeks ahead. Many FDE teams run Scrum for the build and Kanban for run-and-support."*

### Definition of Ready and Done (4 min)

**Say:** *"Two checklists prevent most sprint arguments. Definition of Ready says when a story can enter a sprint. Definition of Done says when it can leave."*

**Write both lists on the board; they become the team's checklists for Labs 2 and 4:**

**Definition of Ready (ProcureFlow)**

- Written as a user story in glossary terms
- Has Given/When/Then acceptance criteria
- Linked NFRs identified
- Estimated by the team
- Dependencies and open questions resolved or noted
- Small enough to finish in one sprint

**Definition of Done (ProcureFlow)**

- Code reviewed and merged through a pull request
- Automated tests for every acceptance criterion pass
- Linked NFRs checked
- Audit log entries verified where relevant
- Demonstrated to the Product Owner
- Traceability matrix updated

### Comprehension Check (2 min)

1. *"A story has no acceptance criteria but the team is 'pretty sure' what it means. Can it go into the sprint?"* (No. It fails the Definition of Ready. Refine it first.)
2. *"Code is merged and tests pass, but Priya hasn't seen it. Is it done?"* (Not by our Definition of Done: it hasn't been demonstrated to the Product Owner.)

**Common misconception:** Done means "code merged".

**Re-teach trigger:** if more than a quarter say yes to question 2, ask: *"Would Priya's AP team agree it's done?"*

**Transition — Say:** *"Ready and Done only work if the requirements are clear. So where do requirements come from, and what do we do when stakeholders disagree?"*

## SEGMENT 3: Elicitation, Conflicts and Story Mapping (25 min)

### Four Kinds of Requirement (6 min)

**Say:** *"Everything Priya told us in Module 1 falls into one of four buckets. Sorting them early saves arguments later."*

**Draw on whiteboard:**

| Kind | Plain meaning | ProcureFlow example |
| --- | --- | --- |
| Functional | What the system does | Route a requisition to approvers by amount |
| Non-functional (NFR) | How well it does it | Month-end peak: ~360 invoices a day without slowing down |
| Constraint | A fixed limit we can't negotiate | SAP stays the system of record; Microsoft sign-in only; live before March year-end |
| Assumption | Something we're betting on | Approvers will approve on their phones |

**Do (2 min):** Read four statements; learners shout the bucket.

1. *"Requesters can see where their requisition is."* (Functional)
2. *"Vendor bank details are visible only to AP."* (NFR: security)
3. *"We must use the existing SAP for payments."* (Constraint)
4. *"Suppliers will send invoices as PDFs."* (Assumption)

### Handling Conflicting Stakeholders (6 min)

**Say:** *"Here's a real conflict at Meridian. AP clerks want zero tolerance on price mismatches: any difference, the invoice goes on hold. Buyers want 5%, because small rounding differences create hold work. Both are right from where they sit. What do you do?"*

**Take two or three answers, then write the four steps on the board:**

1. **Make it visible.** Write both positions side by side, in their own words.
2. **Tie it to the outcome.** *"Which option moves Priya's KPIs: invoices on hold (22% → target) and paid on time (71% → 95%)?"*
3. **Give the decision-maker options with data.** *"At 0%, roughly the current hold rate stays. At 5%, holds drop but overpayment risk rises."*
4. **Record the decision.** Who decided, what, why, and when. In the repo, as a decision note linked to the story.

**Say:** *"Your job isn't to pick the winner. It's to make the trade-off clear enough that the right person can decide quickly, and then make sure the decision is written down. You'll hear Priya's decision in Lab 3."*

### Story Mapping and Release Planning (9 min)

**Say:** *"Twenty stories in a flat list tell you nothing about what to build first. A story map does. Across the top, the user's journey. Down each column, the stories, most essential at the top. Then you slice horizontally into releases."*

**Live demo:** Build the backbone on the wall, left to right, from the Module 1 event-storming map:

```
BACKBONE:  Request  →  Approve  →  Order  →  Receive  →  Invoice & match  →  Pay  →  Report
```

**Say:** *"Three slicing ideas to know:"*

- **Walking skeleton:** *"The thinnest slice that runs end to end through every step, even if some steps are manual or stubbed. It proves the architecture works."*
- **MVP (Minimum Viable Product):** *"The smallest release that moves a real business number. For Priya, that's approval cycle time or invoices on hold."*
- **Increments:** *"Each later release adds value on top, in priority order."*

**Live demo:** Place stickies for the walking skeleton row only, one per backbone column:

```
WALKING SKELETON:
Submit a requisition | One approver approves | PO record created | Record GRN | Record invoice | Mark ready to pay | Show audit log
```

**Say:** *"Notice what's missing: no amount-based routing, no SAP, no automatic match. That's on purpose. The skeleton only has to walk. Muscles come in the next releases."*

### Comprehension Check (2 min)

1. *"A team's Release 1 is 'all of Requisitioning, perfectly'. Is that a walking skeleton?"* (No. It goes deep in one column instead of thin across the whole journey; nothing proves the end-to-end flow works.)
2. *"What makes a release an MVP rather than just 'the first release'?"* (It moves a real business KPI for the sponsor.)

**Common misconception:** MVP means "a low-quality version of everything".

**Re-teach trigger:** if teams slice by bounded context, redraw the map and ask: *"Can a requester see their request paid at the end of Release 1?"*

**Transition — Say:** *"After the break, you'll build the full ProcureFlow map and slice it into five releases."*

### BREAK (10 min)

*Suggested break prompt:* *"Which single business number would you promise Priya your MVP moves, and by how much?"*

## SEGMENT 4: Lab 1 — Story Map and Five Releases (40 min)

**Say:** *"Build the full ProcureFlow story map, then slice it into five releases. Release 1 must be a walking skeleton. Release 2 must be an MVP that moves one of Priya's numbers. Every release after that must earn its place."*

### Instructions (read aloud, step by step)

1. **Backbone (5 min).** In teams of 4–5, lay out the user activities left to right: Request → Approve → Order → Receive → Invoice & match → Pay → Report. Use your Module 1 event-storming wall as the source.
2. **Stories under each activity (15 min).** Write short story titles on stickies ("Route by amount", "Delegate when on leave"). Stack them vertically, most essential at the top. Aim for 30–40 stickies across the map.
3. **Slice (12 min).** Draw four horizontal lines to make five releases. Name each release and write the one business outcome it delivers.
4. **Check (5 min).** For R1, trace one requisition from Request to Pay. For R2, name the KPI it moves and its target from Module 1.
5. **Save (3 min).** Photograph the map to `docs/requirements/story-map.png`. Write the releases into `docs/requirements/release-plan.md`.

### Starter Template

```markdown
# docs/requirements/release-plan.md

| Release | Name | Outcome (KPI and target) | Key stories | Fixed date or constraint |
|---|---|---|---|---|
| R1 | Walking skeleton | ___ | ___ | ___ |
| R2 | MVP | ___ | ___ | ___ |
| R3 | ___ | ___ | ___ | ___ |
| R4 | ___ | ___ | ___ | ___ |
| R5 | ___ | ___ | ___ | ___ |
```

### Reference Solution (instructor)

**Example (illustrative — teams will slice differently; check the reasoning, not the exact cut):**

| Release | Name | Outcome | Key stories |
| --- | --- | --- | --- |
| R1 | Walking skeleton | One requisition travels end to end with a full audit trail | Submit requisition; single approver; PO record created; record GRN; record invoice; mark ready to pay; view audit log; Microsoft sign-in |
| R2 | MVP: faster approvals, fewer holds | Approval cycle 9 → 2 days; invoices on hold 22% → lower | Route by amount (manager / department head / CFO); delegate when on leave; PO pushed to SAP; GRN at the dock; automatic three-way match within tolerance; on-hold queue for AP |
| R3 | Sourcing and supplier quality | Fewer after-the-fact POs; fewer duplicate vendors | Three quotes above ₹2 lakh; supplier/vendor duplicate check; requester status notifications |
| R4 | Compliance and visibility | Audit-ready; KPI tracking for Priya | GST e-invoicing; UK VAT handling for Leeds; KPI dashboard (cycle time, on-hold rate, on-time payment) |
| R5 | Scale and growth | Ready for +30% volume after the acquisition | Onboard new sites; mobile approvals; early-payment discount scheduling |

**Constraint to flag:** any release touching SAP must line up with IT's change window around the March upgrade (the plan-driven piece from Segment 1).

### Instructor Circulates

- **R1 goes deep, not wide** ("All of Requisitioning"): *"Can a requester see their request paid at the end of R1?"*
- **R2 has no number:** *"Which of Priya's KPIs moves, from what to what?"*
- **Stickies are tasks, not stories** ("Set up database"): *"Who gets value from that? Fold it into a story."*
- **Glossary drift** ("Purchase request" instead of "requisition"): send them back to `docs/domain/glossary.md`.

**Success check:** backbone has every activity; R1 runs end to end; R2 names a KPI and a target; every release has one stated outcome; the SAP constraint is marked.

**Extension (fast teams):** mark which stories depend on the SAP change window, and move them if the window doesn't line up.

**Transition — Say:** *"Your stickies are titles. Next, we turn them into stories a developer can build and Priya can test."*

## SEGMENT 5: Writing User Stories That Pass INVEST (25 min)

### Epics, Features, Stories (3 min)

**Say:** *"Three sizes of the same idea. An **epic** is a big goal, often a bounded context or a release theme: 'Requisition approvals'. A **feature** is a chunk of it: 'Amount-based routing'. A **story** is one slice a team can finish in a sprint: 'Route a requisition under ₹50,000 to the line manager only'."*

**Write the story template on the board:**

> **As a** [role from the glossary], **I want** [capability], **so that** [benefit tied to an outcome].

### INVEST (5 min)

**Say:** *"A good story passes six checks. INVEST."*

| Letter | Means | Ask yourself | Fails when… |
| --- | --- | --- | --- |
| I | Independent | Can it be built and released on its own? | "Only works after stories 4, 7 and 9" |
| N | Negotiable | Is the *how* still open for discussion? | It names a screen layout or a database table |
| V | Valuable | Does a real user or the sponsor get value? | "Set up the database" |
| E | Estimable | Can the team size it? | Nobody knows how SAP's PO interface works |
| S | Small | Fits comfortably in one sprint? | "Build accounts payable" |
| T | Testable | Can we write a pass/fail check? | "Approvals should be easy" |

### Story Splitting Patterns (5 min)

**Say:** *"When a story is too big, split it. Don't split by technical layer ('front end', 'back end'), because neither half delivers value alone. Split by one of these."*

| Pattern | Big story | Split into |
| --- | --- | --- |
| Workflow steps | Manage a requisition | Submit · Approve · Reject · Withdraw |
| Business rule variations | Route by amount | Up to ₹50,000 · ₹50,000–₹5 lakh · Above ₹5 lakh |
| Happy / unhappy path | Match an invoice | Match succeeds · Match fails, invoice put on hold |
| Data variations | Record an invoice | Indian supplier (GST) · UK supplier (VAT) |
| Interface (simple first) | Send PO to SAP | Record PO in ProcureFlow · Push PO to SAP |
| Spike first | Integrate with SAP | 2-day spike to learn the interface · then the real story |

### Acceptance Criteria in Given / When / Then (6 min)

**Say:** *"Acceptance criteria turn a story into tests. Given sets the starting state, When is the action, Then is the observable result. One criterion, one behaviour."*

**Live demo:** write this on screen, one line at a time, asking the class for each Then before you type it.

```markdown
Story: As a requester, I want my requisition routed to the right approvers by amount,
so that I don't need to know the approval matrix.

AC1  Given a requisition for ₹40,000
     When I submit it
     Then it is sent to my line manager only

AC2  Given a requisition for ₹3,00,000
     When I submit it
     Then it is sent to my line manager, then my department head

AC3  Given a requisition for ₹6,00,000
     When I submit it
     Then it is sent to my line manager, then my department head, then the CFO

AC4  Given a requisition for exactly ₹50,000
     When I submit it
     Then it is sent to my line manager only
```

**Ask:** *"Why did I add AC4?"* (Boundaries are where bugs live. Priya said "up to ₹50,000", so exactly ₹50,000 goes to the manager only. If the policy were ambiguous, AC4 would become an open question for Priya.)

### Requirements as Markdown, Reviewed Through Pull Requests (4 min)

**Say:** *"Where do stories live? Not in a slide deck or a chat thread. In the repo, as Markdown, next to the code. One file per story. A change to a story is a pull request, and the PR review is where Priya, or her delegate, agrees the acceptance criteria."*

**Show the story file template:**

```markdown
---
id: PF-012
title: Route requisition by amount
epic: Requisitioning
release: R2
moscow: Must
points: 5
nfr: [NFR-PERF-01, NFR-AUD-01]
status: Ready
---
## Story
As a **requester**, I want my **requisition** routed to the right **approvers** by amount,
so that I don't need to know the approval matrix.

## Acceptance criteria
- AC1 Given … When … Then …

## Open questions
- …

## Trace
- Code: (PR link once built)
- Tests: tests/requisitioning/test_routing.py
- Release: R2
```

**Say:** *"Three benefits. Version history: you can see who changed an acceptance criterion and why. Review: the PR is a natural approval step, which Meridian's auditors will like. And traceability: the story ID travels into branch names, commits and tests. More on that after the break."*

### Comprehension Check (2 min)

1. *"'As a developer, I want a requisitions table so that I can store data.' Which INVEST letters does it fail?"* (V: no user value; N: it names the implementation. It's a task, not a story.)
2. *"Why not split 'Route by amount' into 'routing UI' and 'routing API'?"* (Neither half is valuable or testable alone; split by rule variation instead.)

**Common misconception:** acceptance criteria are a description of the screen.

**Re-teach trigger:** if more than a quarter miss question 1, rewrite it live: *"As a requester, I want to save a draft requisition so that I can finish it later."*

**Transition — Say:** *"Now write twenty of these. An AI assistant will do the first draft. You'll do the important part."*

## SEGMENT 6: Lab 2 — Twenty User Stories, AI-Assisted (50 min)

**Say:** *"Twenty stories, as Markdown files, reviewed through pull requests. Split them across your team: four or five each. Start from R1 and R2 on your story map, because those are the ones you'll estimate later today."*

### Instructions (read aloud, step by step)

1. **Pick (3 min).** Each learner takes 4–5 stickies from R1 and R2 of the team's story map. Together the team covers 20.
2. **Draft with AI (5 min).** Paste your Module 1 `interview-notes.md` and `glossary.md` with the prompt below.
3. **Refine against INVEST (17 min).** For each draft: fix the role and terms to match the glossary, rewrite the benefit to name an outcome, add a boundary or unhappy-path criterion, and split anything that won't fit in one sprint.
4. **Write files (10 min).** One file per story under `docs/requirements/stories/`, named `PF-001-submit-requisition.md`, using the template from Segment 5.
5. **Pull request (10 min).** Push on a branch named `req/PF-001-PF-005`. Open a PR using the checklist below. A teammate reviews it and leaves at least one comment.
6. **Merge (5 min).** Fix review comments and merge. Open questions stay in the file, not in chat.

### The AI Prompt (give to learners exactly)

```
Using these interview notes and this domain glossary, draft user stories for the
features listed below. Use the format "As a <role>, I want <capability>, so that
<benefit>". Use only roles and terms that appear in the glossary. For each story,
write 3-4 acceptance criteria in Given/When/Then, including at least one boundary
or failure case. List anything you had to assume under "Assumptions".

Features: <paste your 4-5 sticky titles>
```

### What the AI Draft Typically Gets Wrong (show learners)

**Example AI draft (illustrative — actual AI output will vary run to run):**

```markdown
As a user, I want to create a purchase request in an intuitive interface,
so that the process is more efficient.

Given the user is logged in
When they click the "Create Request" button
Then a form is displayed
```

**Say, pointing at each problem:**

- *"'A user'. Which one? Our glossary has requesters, approvers, buyers, AP clerks."*
- *"'Purchase request'. Priya says requisition."*
- *"'Intuitive' and 'more efficient'. Not testable. Which outcome? Approval cycle time."*
- *"The criterion describes a button and a form. That's UI, not behaviour. What must be true after submitting?"*

**Refined version:**

```markdown
As a requester, I want to submit a requisition with item, quantity, amount and cost centre,
so that it reaches the right approver the same day.

AC1  Given all required fields are filled
     When I submit the requisition
     Then it gets a requisition number and its status is "Awaiting approval"
AC2  Given the amount field is empty
     When I submit
     Then the requisition is not submitted and I'm told the amount is required
AC3  Given I submit a requisition
     When it is saved
     Then the audit log records who submitted it and when
```

### PR Checklist (put in the PR template)

```markdown
- [ ] Role and terms match docs/domain/glossary.md
- [ ] Benefit names a business outcome
- [ ] Passes INVEST (note any letter it fails and why)
- [ ] At least one boundary or unhappy-path criterion
- [ ] Linked NFRs listed (placeholder IDs fine until Lab 3)
- [ ] Open questions written in the file
```

### Reference Solution (instructor): 20 Story Titles

**Example (illustrative — teams' sets will differ):**

| ID | Story (short) | Epic | Release |
| --- | --- | --- | --- |
| PF-001 | Requester submits a requisition | Requisitioning | R1 |
| PF-002 | Line manager approves or rejects a requisition | Requisitioning | R1 |
| PF-003 | Requester sees requisition status | Requisitioning | R1 |
| PF-004 | Users sign in with their Microsoft account | Platform | R1 |
| PF-005 | Buyer creates a PO from an approved requisition | Purchasing | R1 |
| PF-006 | Warehouse records a GRN against a PO | Receiving | R1 |
| PF-007 | AP clerk records an invoice against a PO | Accounts Payable | R1 |
| PF-008 | AP clerk marks an invoice ready to pay | Accounts Payable | R1 |
| PF-009 | Auditor views the audit log for a requisition | Platform | R1 |
| PF-010 | Route requisition up to ₹50,000 to line manager | Requisitioning | R2 |
| PF-011 | Route ₹50,000–₹5 lakh to department head as well | Requisitioning | R2 |
| PF-012 | Route above ₹5 lakh to CFO as well | Requisitioning | R2 |
| PF-013 | Approver delegates approvals while on leave | Requisitioning | R2 |
| PF-014 | Requester cannot approve their own requisition | Requisitioning | R2 |
| PF-015 | Approved PO is pushed to SAP | Purchasing | R2 |
| PF-016 | Warehouse records GRN at the dock on a tablet | Receiving | R2 |
| PF-017 | Invoice auto-matched to PO and GRN within tolerance | Accounts Payable | R2 |
| PF-018 | Mismatched invoice put on hold and buyer notified | Accounts Payable | R2 |
| PF-019 | AP clerk works from an on-hold queue | Accounts Payable | R2 |
| PF-020 | Spike: learn SAP PO interface (2 days) | Purchasing | R2 |

### Instructor Circulates

- **"As a user" or "As the system":** *"Which person from the glossary?"*
- **No unhappy path:** *"What happens when it goes wrong?"*
- **Story too big** ("Automatic three-way match with all variations"): *"Which splitting pattern fits? Try happy path first."*
- **AI-invented terms** ("RFQ", "purchase request"): *"Did Priya say that?"*
- **PRs merged without a comment:** send back for review.

**Success check:** 20 story files merged; every story names a glossary role and an outcome; every story has at least one boundary or failure criterion; at least two stories show a split, with the pattern named.

**Extension (fast teams):** write an `INVEST-review.md` listing three AI-drafted stories, which letters they failed, and how you fixed them.

**Transition — Say:** *"Notice what your stories don't say: how fast, how secure, how available. After the break, we pin those down."*

### BREAK (10 min)

*Suggested break prompt:* *"Priya says ProcureFlow must be 'fast'. Write one sentence that would let a tester prove it, with a number in it."*

## SEGMENT 7: Measurable NFRs and Traceability (20 min)

### From "Fast" to a Number (4 min)

**Ask:** *"Read me your break sentence for 'fast'."* Take two or three.

**Say:** *"A measurable NFR has four parts: a **metric**, a **target**, the **conditions** it must hold under, and **how we'll test it**. Miss any one and it's back to being a wish."*

**Write the formula on the board:**

> **[Metric] must be [target] under [conditions], verified by [test].**

**Live demo:** rewrite one vague requirement in front of the class.

- Vague: *"The system should be fast."*
- Measurable: *"95% of requisition submissions complete in under 2 seconds with 140 concurrent users during the month-end peak, verified by a load test before each release."*

**Say:** *"Where did 140 come from? Module 1's `users.md`, and it was marked as an assumption. So this NFR inherits that assumption. Write that down next to it."*

### The Six Categories (6 min)

**Say:** *"Today's six categories, each with a vague version you'll hear from clients and a measurable version you should write."*

| Category | Vague (what you'll hear) | Measurable (what you write) |
| --- | --- | --- |
| Performance | "It must be quick." | 95% of pages load in under 2 s at 140 concurrent users |
| Scalability | "It must handle growth." | Handles 468 invoices/day at peak (today's 360 + 30%) with no change to response times |
| Availability | "It can't go down." | 99.5% available during business hours (Pune 08:00 IST to Leeds 18:00 UK time); no planned downtime in the last 5 working days of a month |
| Security | "It must be secure." | Microsoft sign-in only; nobody can approve their own requisition; vendor bank details visible only to AP roles |
| Logging and audit | "Audit must be happy." | Every submit, approve, reject and payment action logged with user, time and before/after values; log entries cannot be edited |
| Supportability | "Support must be able to fix it." | Every error logged with a reference ID shown to the user; runbook for the top 5 failure scenarios |

**Say:** *"Notice the availability window. Pune starts at 8 a.m., Leeds finishes at 6 p.m. UK time. That's the Module 1 geography turning into a requirement."*

### Traceability: Story → Code → Test → Release (6 min)

**Say:** *"Meridian's internal audit will ask: 'Show me that the rule No one approves their own requisition was built, tested and released.' Traceability is how you answer in one minute, not one week."*

**Draw the chain on the board:**

```
PF-014 (story file)  →  branch feat/PF-014-no-self-approval  →  PR #42 "PF-014: block self-approval"
      →  test: test_PF014_requester_cannot_approve_own  →  Release R2 notes: "PF-014 shipped"
```

**Say:** *"The trick is simple: the story ID goes everywhere. Branch name, PR title, commit message, test name, release notes. Then a traceability matrix just collects them."*

**Show the matrix template (this is `docs/requirements/traceability.md`):**

```markdown
| Story | Acceptance criteria | NFRs | Code (PR) | Tests | Release | Status |
|---|---|---|---|---|---|---|
| PF-014 | AC1–AC3 | NFR-SEC-02, NFR-AUD-01 | #42 | test_PF014_* | R2 | Done |
| PF-017 | AC1–AC4 | NFR-PERF-02 | — | — | R2 | Ready |
```

**Say:** *"Today most columns will be empty. That's fine. The matrix starts on day one and fills as you build."*

### Comprehension Check (4 min)

1. *"'The system must be highly available.' What's missing?"* (A metric and target, the conditions or hours it applies to, and how it will be tested.)
2. *"An auditor asks how you know PF-014 was tested. Where do you look?"* (The traceability matrix: PF-014's row links to the test names and the PR.)

**Common misconception:** NFRs are a technical afterthought, decided by the developers.

**Re-teach trigger:** if more than a quarter can't spot what's missing in question 1, run the formula on *"It must be secure."* together.

**Transition — Say:** *"You know the shape of a good NFR. The numbers have to come from Priya. Go and get them."*

## SEGMENT 8: Lab 3 — NFRs With the Sponsor, AI-Assisted (35 min)

**Say:** *"Priya will speak in feelings: 'fast', 'never down', 'secure'. Your job is to leave with numbers she agrees to. An AI assistant will help you find the questions you forgot to ask."*

### Instructions (read aloud, step by step)

1. **Find the gaps with AI (5 min).** Paste your `users.md`, `open-questions.md` and three of your stories with the prompt below.
2. **Pick your questions (3 min).** Choose the 6 most important questions. At least one per category: performance, scalability, availability, security, logging and audit, supportability.
3. **Interview Priya (15 min).** Press-conference style, as in Module 1. Every answer you get, push for a number: *"How many? How fast? How long? What happens if not?"*
4. **Write `nfr.md` (10 min).** One row per NFR, using the template below. Every target needs a source: Priya, a calculation, or "assumption".
5. **Link (2 min).** Replace the placeholder NFR IDs in your story files with the real ones.

### The AI Prompt (give to learners exactly)

```
Here is a user profile, a list of open questions and three user stories for a
procure-to-pay system. List the non-functional requirements that are missing or
unmeasurable, grouped by: performance, scalability, availability, security,
logging and audit, supportability. For each, write one question I should ask
the business sponsor to get a measurable target. Do not invent targets.
```

**Example AI output (illustrative — actual AI output will vary run to run):**

```
Availability: During which hours must the system be available, and how much
downtime per month is acceptable?
Logging and audit: How long must approval records be kept, and who may view them?
Security: Which roles may see supplier bank details?
```

**Say:** *"Good questions, but notice what you still have to do: decide which matter most, ask them in Priya's language, and push past her first answer."*

### How to Play Priya (instructor only)

**Rules for yourself:** answer the first time in feelings. Give a number only when pushed with "how many", "how long", or "what happens if". Relay IT and security answers as *"The CISO told me…"*.

**Sponsor NFR fact card:**

| If asked about… | First answer (feeling) | When pushed for a number |
| --- | --- | --- |
| Speed | "My approvers give up if it's slow." | "More than two or three seconds and they go back to email." |
| Busy periods | "Month-end is chaos." | "Use the numbers you already have. And plan for the acquisition next year." |
| Hours | "People use it all day." | "Pune starts at 8 in the morning. Leeds finishes at 6 in the evening, their time." |
| Downtime | "It can't go down." | "An hour down on the last working day of the month is a disaster. Any other day, a short outage outside business hours is fine." |
| Data loss (hidden) | "We can't lose approvals." | "If we lost more than 15 minutes of approvals, audit would never let me hear the end of it." |
| Security | "It must be secure." | "The CISO says: Microsoft sign-in only. Nobody approves their own requisition. Vendor bank details only for AP." |
| Data location (hidden) | — | "The CISO wants it hosted in India. Leeds can access it from there." |
| Audit | "Audit must be happy." | "Who approved what and when, kept for eight years. Nobody can change history." |
| Support | "Someone has to fix it." | "The service desk works 8 to 6 India time. At month-end, if approvals stop, I want someone on it within an hour." |
| Price tolerance (the Segment 3 conflict) | "AP and the buyers have been fighting about this for years." | "Decision: 2% or ₹5,000, whichever is lower. Write it down so they stop asking me." |
| Trap: "Should it be fast and secure?" | "Yes, obviously." | (No number. Debrief: a yes/no question gets a useless answer.) |

### Starter Template

```markdown
# docs/requirements/nfr.md

| ID | Category | Requirement | Metric and target | Conditions | How tested | Source | Linked stories |
|---|---|---|---|---|---|---|---|
| NFR-PERF-01 | Performance | ___ | ___ | ___ | ___ | ___ | ___ |
```

### Reference Solution (instructor, partial)

**Example (illustrative — teams' wording and IDs will vary):**

```markdown
| ID | Category | Metric and target | Conditions | How tested | Source |
|---|---|---|---|---|---|
| NFR-PERF-01 | Performance | 95% of submit/approve actions < 2 s | 140 concurrent users, month-end | Load test each release | Priya + assumption (140) |
| NFR-SCAL-01 | Scalability | 468 invoices/day with no change to NFR-PERF-01 | Peak, after acquisition | Load test at 468/day | Calculation (360 × 1.3) |
| NFR-AVL-01 | Availability | 99.5% during 08:00 IST – 18:00 UK time | Business days | Uptime monitoring | Priya |
| NFR-AVL-02 | Availability | No planned downtime in last 5 working days | Month-end | Release calendar check | Priya |
| NFR-REC-01 | Recovery | Lose at most 15 minutes of data | Any failure | Restore drill each quarter | Priya |
| NFR-SEC-01 | Security | Microsoft sign-in only | All users | Sign-in test | CISO via Priya |
| NFR-SEC-02 | Security | Requester cannot approve own requisition | All amounts | Automated test | CISO via Priya |
| NFR-SEC-03 | Security | Vendor bank details visible to AP roles only | All screens and exports | Role-based test | CISO via Priya |
| NFR-SEC-04 | Security | Hosted in an India region | Production | Deployment review | CISO via Priya |
| NFR-AUD-01 | Audit | Every approval logged (who, what, when); kept 8 years; not editable | All actions | Audit log test | Priya |
| NFR-SUP-01 | Supportability | Month-end approval outage: response within 1 hour | Last 5 working days | Support runbook drill | Priya |
```

**Also record the decision:** price tolerance = 2% or ₹5,000, whichever is lower; decided by Priya; linked to PF-017.

### Instructor Circulates

- **Targets with no source:** *"Did Priya say that, did you calculate it, or are you assuming?"*
- **Yes/no questions to Priya:** *"What answer did you get? What would you ask instead?"*
- **Availability with no hours:** *"99.5% of when?"*
- **Decision not written down:** the tolerance decision must appear in `nfr.md` or a decision note, linked to PF-017.

**Success check:** at least 8 NFRs covering all six categories; every one has a metric, target, conditions, test method and source; the tolerance decision is recorded; story files link to real NFR IDs.

**Extension (fast teams):** add one NFR the AI suggested that Priya didn't mention, mark it "proposed", and add it to open questions for the CISO.

**Transition — Say:** *"You know what to build and how well. Last question: how much fits in a sprint, and what goes first?"*

## SEGMENT 9: Estimation and MoSCoW (13 min)

### Story Points and Planning Poker (5 min)

**Say:** *"Story points are not hours. They're a relative size: effort, complexity and uncertainty together. We use a Fibonacci-like scale, 1, 2, 3, 5, 8, 13, because the gaps grow as uncertainty grows. Nobody can tell a 9 from a 10, but everyone can tell a 5 from an 8."*

**Say:** *"Pick a reference story everyone understands and call it a 3. Everything else is sized against it. For us: PF-001, 'Requester submits a requisition', is a 3."*

**Live demo: one round of planning poker with three volunteers.** Story: PF-017, "Invoice auto-matched to PO and GRN within tolerance".

1. Read the story and its acceptance criteria aloud.
2. Everyone picks a card silently, then all reveal at once.
3. Highest and lowest explain their reasoning.
4. Re-vote. Stop when the team converges.

**Example (illustrative — volunteers' votes will vary):** first reveal 3, 8, 13. The 13 says: *"We've never seen SAP's data. What if GRNs arrive in a different format?"* The 3 says: *"It's just comparing three numbers."* Re-vote: 8, 8, 13.

**Say:** *"That 13 is telling us something. The uncertainty is about SAP, not about matching. That's a **spike**: a short, time-boxed piece of research, like PF-020 'Learn the SAP PO interface, 2 days'. Do the spike, then re-estimate."*

### T-Shirt Sizing and Velocity (3 min)

**Say:** *"For big things, like epics or whole releases, points are too precise. Use **T-shirt sizes**: S, M, L, XL. R3 is an M; R4 with GST e-invoicing is an XL."*

**Say:** *"**Velocity** is the points a team actually completes per sprint. You only know it after two or three sprints. Until then, forecast as a range and label it an assumption, exactly like the 140 concurrent users."*

**Write on board:**

```
R1 + R2 backlog       ≈ 90 points (example)
Assumed velocity       = 20–30 points per 2-week sprint (ASSUMPTION: no history yet)
Forecast               = 90 ÷ 30 to 90 ÷ 20  →  3 to 5 sprints  →  6 to 10 weeks
```

**Say:** *"That's how you answer Priya's 'when?': a range, the assumption behind it, and the date you'll update it."*

### MoSCoW (3 min)

**Say:** *"MoSCoW sorts the backlog by what happens if we don't build it."*

| Category | Meaning | ProcureFlow example |
| --- | --- | --- |
| Must | Release fails without it | Route by amount; no self-approval (audit) |
| Should | Important, painful to drop, workaround exists | Delegate when on leave |
| Could | Nice to have if time allows | Requester status notifications |
| Won't (this time) | Agreed out of scope for this release | Mobile approvals (R5) |

**Say:** *"Rule of thumb: keep Musts to about 60% of capacity, so there's room for surprises. If everything is a Must, nothing is."*

### Comprehension Check (2 min)

1. *"Two developers estimate the same story at 3 and 13. What do you do?"* (Have both explain; the gap usually reveals a misunderstanding or hidden risk. Clarify, maybe add a spike, then re-vote.)
2. *"Priya marks all 20 stories as Must. What do you say?"* (Ask what fails without each one; tie it to the R2 outcome; keep Musts near 60% of capacity.)

**Common misconception:** story points are hours in disguise, or a measure of individual productivity.

**Re-teach trigger:** if learners convert points to hours, ask: *"Would a senior and a junior take the same hours? Would they agree it's bigger than PF-001?"*

**Transition — Say:** *"Let's estimate your backlog for real, and agree Sprint 1 with Priya."*

## SEGMENT 10: Lab 4 — Planning Poker, MoSCoW, Sprint 1 Goal (40 min)

**Say:** *"Three outputs: every story estimated, every story prioritised with Priya, and a Sprint 1 goal she agrees to. Priya is your Product Owner. She decides priority; you decide size. Don't let either side do the other's job."*

### Instructions (read aloud, step by step)

1. **Reference story (2 min).** Agree that PF-001 "Requester submits a requisition" is a 3.
2. **Planning poker (18 min).** Estimate all 20 stories on 1, 2, 3, 5, 8, 13. Spend under a minute on stories where everyone agrees; spend the time on big gaps. Any 13 gets a spike or a split.
3. **MoSCoW with Priya (8 min).** Present your list. Priya marks Must, Should, Could or Won't. Push back with data if Musts go above about 60% of the R1 + R2 total.
4. **Sprint 1 (7 min).** Assume velocity of 25 points for a two-week sprint (label it an assumption). Pull stories in priority order until you reach it. Check each against the Definition of Ready.
5. **Sprint goal (3 min).** Write one sentence describing what Priya can see working at the end of Sprint 1. Read it to her; get a yes.
6. **Save (2 min).** Update `points` and `moscow` in each story file. Write `docs/requirements/backlog.md` (ordered list with points, MoSCoW and release) and add the Sprint 1 goal at the top.

### How to Play Priya as Product Owner (instructor only)

| Situation | Priya says | What you're testing |
| --- | --- | --- |
| First look at the list | "Honestly, all of it is a Must." | Do they push back with outcomes and capacity? |
| After pushback | "Fine. Routing by amount and no self-approval are Musts. Delegation is a Should. Notifications can wait." | Do they record it? |
| Sprint 1 proposal is the walking skeleton | "Why can't I have routing by amount in Sprint 1? That's the whole point." | Can they explain the walking skeleton and give a date for routing? |
| Team says routing is in Sprint 2 | "Good. Show me one requisition going all the way through at the Sprint 1 review, and I'll wait." | Do they turn that into the sprint goal? |
| A story with no acceptance criteria is in Sprint 1 | "What exactly will I see for this one?" | Definition of Ready |

### Starter Template

```markdown
# docs/requirements/backlog.md

## Sprint 1 goal
___

Assumed velocity: 25 points per 2-week sprint (assumption; update after Sprint 2)

| Order | ID | Story | Points | MoSCoW | Release | Sprint |
|---|---|---|---|---|---|---|
| 1 | ___ | ___ | ___ | ___ | ___ | ___ |
```

### Reference Solution (instructor)

**Example (illustrative — teams' estimates and choices will vary; check the reasoning):**

```markdown
## Sprint 1 goal
At the Sprint 1 review, a Pune requester signs in with their Microsoft account, submits a
requisition, their line manager approves it, and Priya can follow it through PO, GRN and
invoice to "ready to pay", with every step in the audit log.

| Order | ID | Story | Points | MoSCoW | Release | Sprint |
|---|---|---|---|---|---|---|
| 1 | PF-004 | Microsoft sign-in | 3 | Must | R1 | 1 |
| 2 | PF-001 | Submit a requisition | 3 | Must | R1 | 1 |
| 3 | PF-002 | Line manager approves or rejects | 3 | Must | R1 | 1 |
| 4 | PF-009 | Audit log for a requisition | 3 | Must | R1 | 1 |
| 5 | PF-005 | Create PO from approved requisition | 3 | Must | R1 | 1 |
| 6 | PF-006 | Record GRN against a PO | 2 | Must | R1 | 1 |
| 7 | PF-007 | Record invoice against a PO | 3 | Must | R1 | 1 |
| 8 | PF-008 | Mark invoice ready to pay | 1 | Must | R1 | 1 |
| 9 | PF-003 | Requester sees status | 2 | Should | R1 | 1 |
| 10 | PF-020 | Spike: SAP PO interface | 2 days, time-boxed | Must | R2 | 1 |
| 11 | PF-010 | Route up to ₹50,000 | 3 | Must | R2 | 2 |
| 12 | PF-014 | No self-approval | 2 | Must | R2 | 2 |
| … | … | … | … | … | … | … |
```

Sprint 1 total: 23 points plus a 2-day spike, within the assumed 25.

**Say (if teams ask why the spike is in Sprint 1):** *"Because R2's biggest risk is SAP. Learning early means Sprint 2's estimates can be trusted."*

### Instructor Circulates

- **Long debates on small stories:** *"Do you disagree by more than one card? If not, take the higher one and move on."*
- **13s left unsplit:** *"Which splitting pattern or spike turns this into something you'd commit to?"*
- **Sprint goal as a list of story IDs:** *"What will Priya see working? Say it as one sentence."*
- **Velocity stated as fact:** it must say "assumption".

**Success check:** all 20 stories have points and MoSCoW; no unsplit 13s; Musts near 60% of R1 + R2 points or the exception is explained; Sprint 1 fits the assumed velocity; Priya said yes to a one-sentence sprint goal.

**Extension (fast teams):** T-shirt size R3 to R5 and give Priya a date range for R2 using the 20–30 point velocity range.

## SEGMENT 11: CI-2 Review, Summary and Wrap-Up (20 min)

### Build the Traceability Matrix (7 min)

**Say:** *"Last artefact. Create `docs/requirements/traceability.md` with one row per story: ID, acceptance criteria, linked NFRs, release, status. The Code and Tests columns stay empty until you build. Start it now, so it grows with the code instead of being reconstructed for the auditor."*

### CI-2 Peer Review — Gallery Walk (8 min)

**Say:** *"Swap repos with the team to your left. Check their CI-2 against this list. Leave two PR comments: one strength, one gap. Be specific."*

**CI-2 checklist (put on screen):**

- [ ] `docs/requirements/story-map.png`: backbone, stories, five release lines
- [ ] `docs/requirements/release-plan.md`: five releases, each with one outcome; R1 is a walking skeleton; R2 names a KPI and target
- [ ] `docs/requirements/stories/`: 20 story files with glossary roles, outcomes, Given/When/Then, at least one failure case each
- [ ] `docs/requirements/nfr.md`: at least 8 measurable NFRs across all six categories, each with a source
- [ ] `docs/requirements/backlog.md`: every story estimated and prioritised; Sprint 1 goal agreed
- [ ] `docs/requirements/traceability.md`: one row per story, NFRs linked

### What We Covered Today (1 min)

- Chose delivery models per piece of work: Agile for the app, plan-driven for fixed client windows
- Set up Scrum roles, events and artefacts, plus a Definition of Ready and Done
- Sorted requirements into functional, NFR, constraint and assumption, and handled a stakeholder conflict
- Built a story map and sliced it into a walking skeleton, an MVP and three increments
- Wrote 20 user stories that pass INVEST, with Given/When/Then criteria, reviewed through pull requests
- Turned "fast, secure, always up" into measurable NFRs with Priya
- Estimated with planning poker, prioritised with MoSCoW, and agreed a Sprint 1 goal
- Started a traceability matrix from story to release

### Back to the Opening (1.5 min)

**Point at the opening slide: Priya's two questions.**

**Ask:** *"Can we answer her now?"*

(Expected: *"When can my AP team use something?"* → Sprint 1 shows a requisition end to end; the R2 MVP in an estimated 6–10 weeks, based on an assumed velocity we'll update after Sprint 2. *"How will I know it does what we agreed?"* → every story has acceptance criteria she approved in a PR, every NFR has a test, and the traceability matrix links them to releases.)

### Exit Ticket (2 min, written, one per learner)

> *"Priya's colleague says: 'The new report must be user-friendly and load quickly.' Rewrite it as one user story with one Given/When/Then criterion and one measurable NFR."*

**Expected answer (example):** *"As a finance controller, I want to see all invoices on hold by supplier, so that I can clear the oldest first."* AC: *Given 50 invoices on hold, When I open the report, Then they are grouped by supplier, oldest first.* NFR: *95% of report loads in under 3 seconds with up to 1,000 invoices on hold, verified by a load test.*

**Re-teach trigger:** if more than a quarter keep "user-friendly" or "quickly" without a number, open the next module with a 5-minute rewrite drill.

### Bridge to What Comes Next (30 sec)

**Say:** *"This morning Priya asked when and how she'd know. Now you have a story map, a backlog, measurable NFRs and a Sprint 1 goal she agreed to. Remember: a requirement you can't test is just a wish. Next module, we add the guardrails that keep ProcureFlow safe and compliant, and your NFR catalogue becomes the starting point."*

### Homework / Self-Practice

1. **Fix your CI-2 gaps.** Address the gap comment from peer review through a pull request.
2. **Split three stories.** Pick three stories estimated at 8 or more. Split each using a named pattern, write the new files, and re-estimate.
3. **Transfer task.** For the hospital expense-claims domain from Module 1's homework, write three user stories with Given/When/Then criteria and three measurable NFRs (one performance, one security, one audit), each with a source or "assumption".

## Q&A & Doubt Solving

**Likely questions and suggested answers:**

**Q: If the client insists on a fixed-price, fixed-scope contract, can we still work Agile?** → Often, yes. Fix the outcome and the budget, keep scope negotiable inside it, and agree a change process. Where scope truly is fixed (like the SAP change window), plan that piece up front. Say which parts are which in writing.

**Q: Why Markdown in the repo instead of Jira or Confluence?** → Many clients will use a tracker, and that's fine for status. Keeping the requirement text in the repo gives you version history, pull-request review and easy traceability to code and tests. Some teams link the two: the tracker ticket points to the Markdown file.

**Q: How detailed should acceptance criteria be?** → Enough that a tester could write a pass/fail test without asking you. Usually 3–6 criteria per story, including at least one boundary or failure case. More than about eight usually means the story should be split.

**Q: What if the sponsor refuses to give numbers for NFRs?** → Propose a number from data you already have (the user profile, the peak), label it an assumption, and ask her to confirm or change it. People find it easier to react to a number than to invent one.

**Q: Can story points be compared between teams?** → No. Each team's scale is relative to its own reference story. Comparing teams by points invites inflation. Use points only to forecast that team's own work.

**Q: Who owns the traceability matrix?** → The whole team keeps it current as part of the Definition of Done. Someone (often the tech lead) checks it before each release.

**Q: Isn't 'Won't' in MoSCoW a bit harsh for the sponsor?** → It means "won't this time", not "never". Writing it down protects the sponsor: everyone can see what was consciously deferred, instead of finding out late.

## Instructor Notes

- **Stay in character as Priya.** In Lab 3 she answers in feelings until pushed; in Lab 4 she starts with "everything is a Must". Both moments only work if you hold the line.
- **Keep the numbers consistent with Module 1:** 1,377 users, ~140 concurrent (assumption), 360 invoices/day at peak, +30% growth, Pune / Chennai / Leeds.
- **Common mistake:** writing tasks as stories ("Set up the database"). Ask who gets value from it.
- **Common mistake:** acceptance criteria that describe the screen ("a form is displayed"). Ask what must be true afterwards.
- **Common mistake:** NFRs without a source. Every target is Priya, a calculation, or an assumption.
- **Common mistake:** velocity stated as fact before any sprint has run.
- **Engagement tip:** the strongest moment is the planning poker reveal with a 3 and a 13 on the same story. Let the two explain; the class discovers the spike on its own.
- **Time check:** if running behind before the first break, shorten the Scrum table to roles and events only; artefacts come up naturally in Lab 4.
- **Materials to prepare:** planning poker decks (1, 2, 3, 5, 8, 13, ?); sticky notes and wall space for story maps; repos with `docs/requirements/stories/` and a PR template containing the checklist; AI assistant access verified; a pre-captured AI story draft and NFR question list as backup.

## Common Mistakes — Quick Reference

| Mistake | Symptom | Fix |
| --- | --- | --- |
| Release 1 goes deep in one area | R1 is "all of Requisitioning"; nothing runs end to end | Slice thin across the whole backbone: a walking skeleton |
| Task written as a story | "As a developer, I want a table…" | Ask who gets value; fold the task into a user-facing story |
| "As a user" | Vague role, untestable benefit | Use a glossary role and a business outcome |
| Splitting by technical layer | "Routing UI" and "Routing API" | Split by rule, workflow step, happy/unhappy path or data |
| Untestable acceptance criteria | "Easy to use", "a form is shown" | Given/When/Then with an observable result |
| Vague NFR | "Fast", "secure", "always up" | Metric + target + conditions + test, with a source |
| Points treated as hours | Debates about days, not size | Compare to the reference story only |
| Everything is a Must | No room for surprises | Keep Musts near 60% of capacity; tie each to the outcome |
| Traceability built at the end | Matrix reconstructed for audit | Put the story ID in branch, PR, test and release notes from day one |

## Timing Contingencies

| Situation | Action |
| --- | --- |
| Running 10 min behind before the first break | Shorten the Scrum table to roles and events; skip the bucket-sorting drill in Segment 3 |
| Lab 2 running long | Reduce to 15 stories; the remaining 5 become homework |
| Running long after the second break | In Lab 4, estimate only R1 and R2 Musts with poker; T-shirt the rest |
| Low energy after lunch | Run the Lab 4 poker standing, with cards held up high |
| AI assistant unavailable | Hand out the pre-captured AI drafts; the INVEST refinement works the same |
| Git or PR access broken | Write story files locally; review by swapping laptops; push later |
| Very large class | Run Lab 3 as press-conference rounds; teams share answers on a common board |

## End-of-Session Quiz (5 Questions)

1. Meridian's IT allows SAP interface changes only in a fixed window after the March upgrade. Which delivery model fits that piece of work, and why?
2. What is the difference between a walking skeleton and an MVP? Give a ProcureFlow example of each.
3. "As a user, I want an intuitive approval screen so that things are faster." Name two INVEST letters it fails and rewrite it with one Given/When/Then criterion.
4. Rewrite "The system must always be available" as a measurable NFR for ProcureFlow.
5. In planning poker, one developer votes 3 and another votes 13 on the same story. What should the team do?

**Answer key (instructor):**

1. A plan-driven (waterfall-style) piece inside an otherwise Agile project: the date and scope are fixed by the client, so design and test it ahead of the window.
2. A walking skeleton is the thinnest end-to-end slice that proves the flow works (one requisition from submit to "ready to pay" with an audit log). An MVP is the smallest release that moves a business KPI (routing by amount and auto-match, cutting approval time from 9 days towards 2).
3. Fails N (names the UI), T ("intuitive", "faster" untestable), and the role isn't from the glossary. Example: "As a line manager, I want to approve a requisition from its summary, so that requests don't wait days." *Given a requisition awaiting my approval, When I approve it, Then its status becomes Approved and the audit log records me and the time.*
4. Example: "ProcureFlow is available 99.5% of the time between 08:00 IST and 18:00 UK time on business days, with no planned downtime in the last 5 working days of a month, verified by uptime monitoring."
5. Both explain their reasoning; clarify the story or uncover the hidden risk; add a spike or split if needed; re-vote.

## Homework Rubric

| Criterion | Excellent (4) | Good (3) | Needs Work (2) | Incomplete (1) |
| --- | --- | --- | --- | --- |
| CI-2 gap fixed via PR | Gap fixed in a PR with a clear description and the reviewer's comment resolved | Gap fixed; PR description thin | Partly fixed or committed without a PR | Not attempted |
| Three stories split and re-estimated | Each split uses a named pattern; every new story passes INVEST and is re-estimated | Splits sensible; one pattern unnamed or one story weak | Split by technical layer or not re-estimated | Not attempted |
| Transfer task (expense claims) | 3 INVEST stories with Given/When/Then; 3 NFRs with metric, target, conditions, test and source | Mostly correct; one NFR missing a part | Stories untestable or NFRs vague | Not attempted |

**Total:** /12. Pass threshold: 8/12.

## Materials Checklist

- [ ] Opening slide: Priya's two-question email
- [ ] Planning poker decks (1, 2, 3, 5, 8, 13, ?) for every team
- [ ] Sticky notes and wall space (or a digital board) for story maps
- [ ] Sponsor NFR fact card and Product Owner card (instructor only)
- [ ] Team repos with `docs/requirements/stories/` and a PR template containing the checklist
- [ ] Module 1 artefacts available in each repo (`docs/domain/`)
- [ ] AI assistant access tested on the classroom network
- [ ] Backup: pre-captured AI story draft and NFR question list
- [ ] Timer visible for all labs
