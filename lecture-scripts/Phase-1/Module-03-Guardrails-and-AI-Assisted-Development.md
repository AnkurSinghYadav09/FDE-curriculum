# Lecture Script: Project Guardrails, .md Files and AI-Assisted Development

> **Instructor reference.** Phase 1 (Focus area 1), Module 3 | Duration: 6 hours (3 h concepts and demos, 3 h hands-on) | Previous script: [Module 2 — Requirements Gathering, User Stories and Estimation](Module-02-Requirements-Stories-and-Estimation.md)

## Session overview

**Goal.** Learners put guardrails around the ProcureFlow team, its AI tools and its client: a protected repo, the project `.md` guardrail set, AI instruction files and an AI usage policy. They then use AI assistants inside those guardrails to generate, debug and test code, and ship a walking skeleton (Next.js page, .NET API, SQL Server) to the dev environment (Capstone increment CI-3).

**Learner profile at this point.** Learners have Module 1's domain artefacts (`docs/domain/`) and Module 2's requirements (`docs/requirements/`: story map, 20 stories, `nfr.md`, backlog, traceability matrix, Sprint 1 goal). They have not yet written production code for ProcureFlow. Today introduces the first genuinely new idea of the build phase: **rules written once in the repo, followed every time by people and by AI.**

**Key outcome.** Every learner can explain what each guardrail file governs, set up branch protection, CODEOWNERS, a PR template and pre-commit hooks, write AI instruction files that measurably change AI output, use an AI assistant to fix a bug from a stack trace and prove the fix with a test, and merge a walking skeleton that deploys to dev.

**Case and stack for this session.** ProcureFlow at Meridian Components, continued. The walking skeleton implements Sprint 1's first slice: **PF-001 Requester submits a requisition** and **PF-003 Requester sees requisition status**. Stack: Next.js (web), .NET (API), SQL Server (database), built from the program starter kit and deployed by its starter pipeline. The trainer plays Priya only briefly, in the opening and the change-control demo.

**Starter kit assumption.** This script assumes the starter kit has `web/` (Next.js), `api/` (.NET solution), `db/` (migrations) and `.github/workflows/` (CI and deploy-to-dev). Adjust paths and commands to your kit before class.

**Note on AI output.** AI-generated code, explanations and review comments vary run to run. Examples marked **illustrative** show the expected shape, not an exact transcript. Command output marked **actual** is deterministic for the stated tool behaviour.

## Timing breakdown

| Segment | Duration | Cumulative | Kind |
| --- | --- | --- | --- |
| SEGMENT 1: Opening — Why Guardrails | 15 min | 0:15 | Concept |
| SEGMENT 2: Documentation as Code and the .md Set | 25 min | 0:40 | Concept + demo |
| SEGMENT 3: Git and GitHub for Teams | 25 min | 1:05 | Concept + demo |
| SEGMENT 4: Process Guardrails | 20 min | 1:25 | Concept + demo |
| **BREAK** | 10 min | — |  |
| SEGMENT 5: Lab 1 — Protected Team Repo | 35 min | 2:00 | Hands-on |
| SEGMENT 6: Technical Guardrails | 20 min | 2:20 | Concept + demo |
| SEGMENT 7: AI Guardrails and Instruction Files | 20 min | 2:40 | Concept + demo |
| **BREAK** | 30 min (lunch) | — |  |
| SEGMENT 8: Lab 2 — First Guardrail Set | 45 min | 3:25 | Hands-on |
| SEGMENT 9: How LLM Coding Assistants Work | 25 min | 3:50 | Concept + demo |
| SEGMENT 10: Lab 3 — Prompt Clinic | 35 min | 4:25 | Hands-on |
| **BREAK** | 10 min | — |  |
| SEGMENT 11: AI Across the Development Workflow | 20 min | 4:45 | Concept + demo |
| SEGMENT 12: Lab 4 — Debugging Race | 35 min | 5:20 | Hands-on |
| SEGMENT 13: CI-3 — Walking Skeleton to Dev | 30 min | 5:50 | Hands-on |
| SEGMENT 14: Summary and Wrap-Up | 10 min | 6:00 | Concept |

Concepts and demos: 180 min (50%). Labs and capstone: 180 min (50%), as the module specifies. Breaks are not counted.

## SEGMENT 1: Opening — Why Guardrails (15 min)

### The Hook (7 min)

**Show one slide: "Week 3 at Meridian".** Read it aloud.

**Say:** *"Three good developers, each with an AI assistant, work on ProcureFlow for two weeks. Then the problems arrive. One pull request calls it `PurchaseRequest`, another calls it `Requisition`. An API key for the dev database is sitting in the commit history. AI-generated code writes vendor bank account numbers into the logs. A package the AI suggested turns out to have a licence Meridian's legal team won't accept. And on Friday, Priya adds 'just one small thing' to the sprint in a hallway conversation, and nobody knows whether it's in or out. The CISO calls."*

**Ask:** *"Every person here was competent. Every AI tool did what it was asked. So what went wrong, and what would have prevented each problem?"*

**Do:** Think-pair-share for 1 minute, then take answers. List each problem on the board with the learners' prevention idea next to it.

**Respond to likely answers:**

- *"Code review would catch it."* → *"Review against what? Where are the rules the reviewer checks?"*
- *"They should have used the glossary."* → *"The people might. Did the AI know the glossary existed?"*
- *"Don't let AI write code."* → *"The client is paying for speed, and AI gives it. How do we keep the speed and lose the risk?"*
- *"Priya shouldn't change scope."* → *"She's the client; she's allowed to. What's missing is a way to change it on purpose."*

**Point at the board.** Every prevention idea is a rule that wasn't written down where people, tools and AI could find it.

**Say:** *"Here's today's line:"*

**Write on board, large, and leave it up all day:**

> **If it isn't written in the repo, neither your team nor your AI will follow it.**

### Why Guardrails (5 min)

**Say:** *"A guardrail on a mountain road doesn't slow you down. It's what lets you drive at a sensible speed without fear. Project guardrails do three jobs: keep the **team** consistent, keep the **AI tools** inside the same rules as the team, and keep the **client** aligned on scope, quality and risk."*

**Draw three columns on the board, filled from the opening problems:**

| Process guardrails | Technical guardrails | AI guardrails |
| --- | --- | --- |
| Change control for Priya's "small thing" | Secret scanning blocks the API key | Instruction files tell the AI to use `Requisition` |
| Review checklist and CODEOWNERS | Logging rules forbid bank details in logs | Data-handling rules: no client data in prompts |
| Definition of Done | Licence check on new packages | Human review of every AI change |

### Learning Contract — write on board (2 min)

- Explain what each file in the project `.md` set governs
- Set up a protected repo: branch protection, CODEOWNERS, PR template, pre-commit hooks
- Write AI instruction files and an AI usage policy
- Prompt an AI assistant well, and show the difference instruction files make
- Use AI to debug, then prove each fix with a test
- Ship a walking skeleton to dev through review and the pipeline (CI-3)

### Comprehension Check (1 min)

**Ask:** *"Which of the three guardrail types stops an AI assistant from logging bank details: process, technical or AI?"* (All three can: an AI instruction file tells it not to, `LOGGING.md` and review catch it, and a test or log scanner can block it. Good guardrails overlap.)

**Transition — Say:** *"If the rules have to live in the repo, let's look at the files that hold them."*

## SEGMENT 2: Documentation as Code and the .md Set (25 min)

### Documentation as Code (5 min)

**Say:** *"You've already been doing this for two modules. Your glossary and your stories live in the repo as Markdown, and changes go through pull requests. That's documentation as code. Three properties make it work."*

- **In the repo:** *"Docs sit next to the code they describe. When you change the code, the doc is one folder away, in the same pull request."*
- **Reviewed through PRs:** *"A rule change gets the same review as a code change. Nobody quietly edits the Definition of Done."*
- **Versioned with the code:** *"Check out last March's release and you get last March's rules too. Auditors love that."*

**Say:** *"And there's a fourth reason that didn't exist five years ago: AI assistants read the repo. A rule in a Confluence page the AI can't see is a rule the AI won't follow."*

### The Project .md Guardrail Set (12 min)

**Say:** *"Here's the full set for ProcureFlow. You already have two rows. By the end of the day you'll have most of the rest."*

**Put this table on screen and walk it top to bottom, saying the 'Who reads it first' column aloud for each:**

| File | What it governs | Who reads it first | Status |
| --- | --- | --- | --- |
| `README.md` | Purpose, how to run locally, how to deploy, who to contact | A new joiner on day one | Lab 2 |
| `docs/domain/glossary.md`, `users.md` | Shared business language and user profile | Everyone naming anything | Module 1 |
| `docs/requirements/*.md`, `nfr.md` | User stories, acceptance criteria, measurable NFRs | Developer starting a story | Module 2 |
| `ARCHITECTURE.md`, `docs/adr/NNNN-*.md` | Architecture overview and every significant decision with its reasons | Anyone about to change structure | Starter version today; grows later |
| `CONTRIBUTING.md`, `CODING_STANDARDS.md` | Branching, commit format, review rules, naming and style | Every committer and reviewer | Lab 2 |
| `DEFINITION_OF_DONE.md` | What every story and release must satisfy before it ships | Reviewer and Product Owner | Lab 2 |
| `SECURITY.md` | Security rules, secret handling, how to report a vulnerability | Anyone handling data or credentials | Starter version today |
| `DEPENDENCIES.md` | Allowed licences, how packages are approved, update cadence | Anyone adding a package (or AI suggesting one) | Starter version today |
| `LOGGING.md` | What to log, levels, format, correlation IDs, what must never be logged | Anyone writing a log line | Starter version today |
| `CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md` | Rules AI assistants follow when generating code in this repo | The AI assistant, every session | Lab 2 |
| `docs/prompts/*.md` | Team prompt library | Developer about to prompt | Lab 3 |
| `CHANGELOG.md`, `docs/runbook.md`, `docs/support/handover.md` | Release history, operating procedures, support handover | Support team and the client | Later modules |

**Say:** *"Each file answers one question someone will otherwise ask you in a chat message at 11 p.m. 'How do I run it?' README. 'Can I add this package?' DEPENDENCIES. 'Can I log the vendor ID?' LOGGING."*

### Live Demo: One Rule, Three Places (5 min)

**Say:** *"Watch one rule travel through the set. Module 2 gave us NFR-SEC-03: vendor bank details visible only to AP. Here's how it becomes guardrails."*

**Show these three snippets side by side:**

```markdown
# LOGGING.md — Never log
- Vendor bank account numbers, IFSC/sort codes, tax IDs
- Passwords, tokens, connection strings
- Full request bodies from Accounts Payable endpoints
```

```markdown
# CODING_STANDARDS.md — Sensitive fields
- Mark bank-detail properties with [SensitiveData]; the logging filter redacts them.
```

```markdown
# CLAUDE.md — Data rules
- Never write code that logs vendor bank details or tax IDs. See LOGGING.md.
```

**Say:** *"Same rule, three readers: the developer, the reviewer, the AI. If any one of them misses it, another catches it."*

### Comprehension Check (3 min)

1. *"An AI assistant suggests adding a PDF library. Which file decides whether you may?"* (`DEPENDENCIES.md`: allowed licences and the approval process.)
2. *"Why keep the Definition of Done in the repo rather than on a wiki?"* (Changes are reviewed and versioned with the code, and AI tools and reviewers can read it.)

**Common misconception:** more documents means more bureaucracy. (Each file replaces a repeated question; a file nobody reads should be deleted.)

**Re-teach trigger:** if learners can't match three random files to their purpose, run a 2-minute "who asks this question?" round with five questions.

**Transition — Say:** *"Every one of these files changes through Git. So let's make sure the team uses Git the same way."*

## SEGMENT 3: Git and GitHub for Teams (25 min)

### Trunk-Based vs GitFlow (6 min)

**Say:** *"Two branching strategies you'll meet at clients. Know both; pick on purpose."*

**Draw on whiteboard:**

|  | Trunk-based | GitFlow |
| --- | --- | --- |
| Branches | `main` + short-lived feature branches (hours to 1–2 days) | `main`, `develop`, `feature/*`, `release/*`, `hotfix/*` |
| Integration | Merge to `main` daily; unfinished work hidden behind feature flags | Features merge to `develop`; releases cut from it |
| Fits when | Continuous delivery, one live version, strong CI | Scheduled releases, several supported versions, slow approval cycles |
| Main risk | Needs good tests and flags | Long-lived branches drift; painful merges |

**Say:** *"For ProcureFlow we use trunk-based: one live version, a pipeline that deploys to dev on every merge, and short branches named after the story, like `feat/PF-001-submit-requisition`. If Meridian later needs a frozen release around the SAP window, we cut a release branch for that one moment. That's a decision worth an ADR."*

### Conventional Commits (4 min)

**Say:** *"Commit messages are read by people, by tools that build changelogs, and by AI assistants looking at history. One format for all of them."*

**Write on board:**

```
<type>(<scope>): <short description>

<optional body: why, not what>

Refs: PF-001
```

**Types to use:** `feat`, `fix`, `docs`, `test`, `refactor`, `chore`, `ci`. Scope = the bounded context or area.

**Examples:**

```
feat(requisitioning): submit a requisition with amount and cost centre
fix(requisitioning): route exactly ₹50,000 to line manager only
docs(dod): add audit-log check to Definition of Done
```

**Say:** *"Notice the story ID in the footer. That's the traceability chain from Module 2: story → commit → PR → release."*

### Live Demo: Rebase and a Merge Conflict (10 min)

**Say:** *"Two of you edit the same line of `DEFINITION_OF_DONE.md` on different branches. One merges first. Watch what happens to the second."*

**Live-code (prepared repo, two branches already pushed):**

```bash
git switch docs/dod-audit
git fetch origin
git rebase origin/main
```

**Example output (illustrative — commit hashes and wording vary slightly by Git version):**

```
Auto-merging DEFINITION_OF_DONE.md
CONFLICT (content): Merge conflict in DEFINITION_OF_DONE.md
error: could not apply 3f2a91c... docs(dod): add audit-log check
```

**Open the file and show the markers:**

```markdown
<<<<<<< HEAD
- Automated tests for every acceptance criterion pass, with coverage ≥ 70%
=======
- Automated tests for every acceptance criterion pass
- Audit log entries verified where relevant
>>>>>>> 3f2a91c (docs(dod): add audit-log check)
```

**Say:** *"Careful: during a rebase, `HEAD` is the branch you're rebasing onto, here `main`, and the bottom half is your commit. It's the reverse of what many people expect. The fix isn't 'pick mine' or 'pick theirs'. It's: what should the rule say now? Both changes are valid, so keep both."*

**Resolve to:**

```markdown
- Automated tests for every acceptance criterion pass, with coverage ≥ 70%
- Audit log entries verified where relevant
```

**Then:**

```bash
git add DEFINITION_OF_DONE.md
git rebase --continue
git push --force-with-lease
```

**Say:** *"Three rules for rebasing. Rebase your own branch onto `main` to keep history linear. Never rebase a branch someone else is working on. And when you must force-push your own branch, use `--force-with-lease`, which refuses if someone else pushed in the meantime."*

### Comprehension Check (5 min)

1. *"Which branching strategy fits a client that supports three versions of a mobile app at once, and why?"* (GitFlow-style release branches: each supported version needs its own line of fixes.)
2. *"Write the commit message for fixing the routing bug where ₹50,000 went to the department head."* (e.g., `fix(requisitioning): route exactly ₹50,000 to line manager only` with `Refs: PF-010`.)
3. *"In a conflict, why is 'accept mine' often wrong for docs?"* (Both edits may be valid; the right answer is the rule the team now wants, which can combine both.)

**Common misconception:** rebasing is dangerous and should never be used. (It's safe on your own unshared branch; dangerous only on shared ones.)

**Re-teach trigger:** if more than a quarter can't read the conflict markers, redo the demo with the markers labelled on the board.

**Transition — Say:** *"Git gives us branches. GitHub lets us put rules around them. That's process guardrails."*

## SEGMENT 4: Process Guardrails (20 min)

### Definition of Ready and Done, Now in the Repo (2 min)

**Say:** *"In Module 2 we wrote Ready and Done on the board. Today they become files: `DEFINITION_OF_DONE.md` in the root, with the Ready checklist inside it or in `CONTRIBUTING.md`. From now on, a reviewer can point at a line and say 'not done yet'."*

### Change Control for Scope (5 min)

**Live demo (instructor switches into Priya for 30 seconds):**

**Say, as Priya:** *"While you're in there, can you add a 'duplicate requisition' check? It's tiny. Just for Sprint 1."*

**Say, back as instructor:** *"Hallway requests are how scope creeps. We don't say no. We say 'yes, through the door'. Here's the door."*

**Write on board:**

1. **Capture:** a new story file `PF-021-duplicate-requisition-check.md` in a PR, with acceptance criteria.
2. **Size:** the team estimates it.
3. **Decide:** the Product Owner chooses: swap it for something of equal size, put it in a later sprint, or drop it. The sprint goal doesn't silently grow.
4. **Record:** the PR is the record: who asked, what was decided, when.

**Say:** *"Priya gets what she wants, a decision, quickly. And at the sprint review, nobody argues about what was promised."*

### Branch Protection, CODEOWNERS and PR Templates (9 min)

**Say:** *"Three GitHub features turn our rules from good intentions into things the platform enforces."*

**Branch protection on `main` (show the GitHub settings page):**

- Require a pull request before merging; at least 1 approval (2 for `SECURITY.md` and pipeline changes via CODEOWNERS)
- Require status checks to pass: build, tests, lint, secret scan
- Require branches to be up to date before merging
- Block force pushes and deletion of `main`

**CODEOWNERS:** *"A file that says who must review which paths. GitHub automatically requests them, and with branch protection, their approval is required."*

```
# .github/CODEOWNERS  (later lines win when paths overlap)
*                         @meridian-procureflow/developers
/api/                     @meridian-procureflow/backend-leads
/web/                     @meridian-procureflow/frontend-leads
/docs/requirements/       @meridian-procureflow/product-owner
/SECURITY.md              @meridian-procureflow/security
/.github/workflows/       @meridian-procureflow/tech-lead
/CLAUDE.md                @meridian-procureflow/tech-lead
/AGENTS.md                @meridian-procureflow/tech-lead
```

**Say:** *"Look at the last two lines. Changing what the AI is told to do is as sensitive as changing the pipeline. A tech lead must approve it."*

**PR template:** *"GitHub pre-fills every new PR from `.github/pull_request_template.md`. That's where the review checklist lives."*

```markdown
## What and why
Refs: PF-___

## Checklist
- [ ] Acceptance criteria covered by tests (list test names)
- [ ] Glossary terms used in code and UI
- [ ] No secrets, no sensitive data in logs (LOGGING.md)
- [ ] New packages approved (DEPENDENCIES.md)
- [ ] Docs updated (README, ADR, CHANGELOG) if behaviour changed
- [ ] Traceability matrix updated

## AI assistance
(completed in Lab 2)
```

### Comprehension Check (4 min)

1. *"A developer pushes straight to `main` 'just to fix a typo'. Which guardrail stops it, and why stop a typo fix?"* (Branch protection. Because the rule has to hold for everyone, every time; a typo fix through a PR costs two minutes.)
2. *"Priya asks for a new field mid-sprint. List the four steps."* (Capture as a story PR, size it, PO decides what moves, the PR records it.)
3. *"Why put `CLAUDE.md` in CODEOWNERS?"* (It changes what code the AI generates for everyone; it deserves the same review as the pipeline.)

**Common misconception:** process guardrails are about distrust. (They protect people from 11 p.m. mistakes and from blame.)

**Re-teach trigger:** if most can't name the four change-control steps, replay the Priya request with a different item.

**Transition — Say:** *"After the break, you'll set all of this up on your own team repo, and then try to break it."*

### BREAK (10 min)

*Suggested break prompt:* *"Think of one way a teammate could get code into `main` without anyone reviewing it. After the break we'll close that gap."*

## SEGMENT 5: Lab 1 — Protected Team Repo (35 min)

**Say:** *"Set up your team repo so that the platform enforces your rules. Then spend the last ten minutes trying to break them. If you can get unreviewed code or a secret into `main`, you've found a gap. Close it."*

### Instructions (read aloud, step by step)

1. **Create the repo (5 min).** One person per team creates `procureflow-<team>` from the starter kit template and adds teammates. Copy in `docs/domain/` and `docs/requirements/` from Modules 1 and 2.
2. **Branch protection on `main` (5 min).** Require a PR, 1 approval, review from code owners, passing status checks, and up-to-date branches. Block force pushes and deletion.
3. **CODEOWNERS (5 min).** Create `.github/CODEOWNERS` using your team's names (template below).
4. **PR template (3 min).** Create `.github/pull_request_template.md` from the Segment 4 example.
5. **Linters, formatters and pre-commit hooks (7 min).** Wire up the hook below. Run it once on a clean commit to prove it passes.
6. **Break it (10 min).** Run the four attack tests below. Record each result in `docs/guardrail-tests.md`.

### Starter Files

```
# .github/CODEOWNERS
*                       @<team>/developers
/api/                   @<backend-lead>
/web/                   @<frontend-lead>
/docs/requirements/     @<product-owner-proxy>
/SECURITY.md            @<tech-lead>
/.github/workflows/     @<tech-lead>
/CLAUDE.md              @<tech-lead>
/AGENTS.md              @<tech-lead>
/.github/copilot-instructions.md  @<tech-lead>
```

```sh
# .husky/pre-commit  (adapt paths to your starter kit)
npx lint-staged
dotnet format api/ProcureFlow.sln --verify-no-changes
gitleaks protect --staged --redact
```

```json
// package.json (root) — lint-staged section
"lint-staged": {
  "web/**/*.{ts,tsx}": ["eslint --fix", "prettier --write"],
  "**/*.md": ["prettier --write"]
}
```

**Note:** newer gitleaks releases have renamed some commands. If `protect` isn't recognised, check `gitleaks --help` for the staged-changes command in your version.

### The Four Attack Tests

| # | Attack | Expected result |
| --- | --- | --- |
| 1 | `git push origin main` directly | Rejected by branch protection |
| 2 | Commit a file containing a fake token (instructor supplies one) | Blocked by the pre-commit secret scan |
| 3 | Open a PR and try to merge without approval | Merge button disabled until review and checks pass |
| 4 | Change a file in `docs/requirements/` in a PR | Product Owner proxy automatically requested as reviewer |

**Example output, attack 1 (illustrative — exact wording depends on whether you use branch protection rules or rulesets):**

```
remote: error: GH006: Protected branch update failed for refs/heads/main.
remote: error: Changes must be made through a pull request.
 ! [remote rejected] main -> main (protected branch hook declined)
```

**Example output, attack 2 (illustrative — gitleaks output format varies by version):**

```
Finding:     token = REDACTED
RuleID:      github-pat
File:        api/appsettings.Development.json
leaks found: 1
husky - pre-commit script failed (code 1)
```

**Say (when the first team's secret gets blocked):** *"That's the API key from this morning's story, stopped before it ever left the laptop."*

### Instructor Circulates

- **Admin bypass left on:** *"Can the repo owner still push to `main`? Turn on 'do not allow bypassing' or include administrators."*
- **Hooks only on one laptop:** hooks installed via `npm install` (Husky's `prepare` script) apply to everyone; check a teammate's clone too.
- **Secret scan only local:** remind them the CI pipeline must run it as well; hooks can be skipped with `--no-verify`.
- **CODEOWNERS with wrong handles:** GitHub shows an error on the file in the web UI; check it.

**Success check:** all four attacks fail for the attacker; `docs/guardrail-tests.md` lists each with its result; one passing PR merged through review.

**Extension (fast teams):** try `git commit --no-verify` with the fake token. It gets through locally. Add secret scanning to the CI workflow so the PR fails anyway, and enable GitHub's push protection if your plan supports it.

**Transition — Say:** *"`--no-verify` just showed us that one guardrail isn't enough. Next: the technical guardrails that back each other up."*

## SEGMENT 6: Technical Guardrails (20 min)

### Defence in Depth (5 min)

**Say:** *"You just watched `--no-verify` walk past a pre-commit hook. That's why technical guardrails come in layers. Each layer catches what the one before missed, and each is cheaper the earlier it runs."*

**Draw on whiteboard, left to right:**

| Layer | When it runs | Catches | Can be skipped? |
| --- | --- | --- | --- |
| Coding standards (`CODING_STANDARDS.md`) | While writing | Naming, structure, glossary terms | Yes: it's a document |
| Editor: linter + formatter | On save | Style, simple bugs, unused code | Yes |
| Pre-commit hooks | On `git commit` | Lint, format, secrets, before they leave the laptop | Yes (`--no-verify`) |
| CI quality gate | On every PR | Build, tests, coverage, lint, secret scan, architecture tests | No, with branch protection |
| Human review | On every PR | Intent, design, domain fit, what tools can't judge | No, with CODEOWNERS |

**Say:** *"Only the last two can't be skipped. So anything that really matters, like secrets and tests, must run in CI, not just on the laptop."*

### Linters, Formatters and Coding Standards (4 min)

**Say:** *"Formatters end arguments about style: Prettier for the Next.js side, `dotnet format` for .NET. Linters find likely bugs: ESLint for TypeScript, Roslyn analysers for C#. And `CODING_STANDARDS.md` covers what tools can't: names from the glossary, folder layout by bounded context, how errors are returned."*

**Show an excerpt:**

```markdown
# CODING_STANDARDS.md (excerpt)
- Names come from docs/domain/glossary.md: Requisition, not PurchaseRequest; Vendor in AP, Supplier in Purchasing.
- API folders follow bounded contexts: api/src/Requisitioning, api/src/AccountsPayable, …
- Errors: return RFC 7807 problem details; never return stack traces to the client.
- Money: decimal, never double. Currency stored with every amount.
```

### Quality Gates and Architecture Tests (8 min)

**Say:** *"A quality gate is a set of checks a PR must pass before it can merge. Here's ours."*

**Show the CI job (illustrative — adapt to your starter pipeline):**

```yaml
# .github/workflows/ci.yml (excerpt)
- name: API build and tests
  run: dotnet test api/ProcureFlow.sln --collect:"XPlat Code Coverage"
- name: Web lint and tests
  run: |
    npm ci --prefix web
    npm --prefix web run lint
    npm --prefix web test
- name: Secret scan
  uses: gitleaks/gitleaks-action@v2   # or your organisation's approved scanner
```

**Say:** *"Now a guardrail most teams forget: architecture tests. Our ARCHITECTURE.md says the Domain layer must never depend on Infrastructure. A sentence in a document drifts. A test doesn't."*

**Live-code (xUnit + NetArchTest):**

```csharp
[Fact]
public void Domain_does_not_depend_on_Infrastructure()
{
    var result = Types.InAssembly(typeof(Requisition).Assembly)
        .That().ResideInNamespace("ProcureFlow.Domain")
        .ShouldNot().HaveDependencyOn("ProcureFlow.Infrastructure")
        .GetResult();

    Assert.True(result.IsSuccessful);
}
```

**Say:** *"If an AI assistant, or a tired developer, adds a database call inside a domain class, this test fails in CI and the PR can't merge. The architecture rule just became a guardrail."*

### Comprehension Check (3 min)

1. *"Why run the secret scan in CI if there's already a pre-commit hook?"* (Hooks can be skipped with `--no-verify` or not installed; CI with branch protection can't be skipped.)
2. *"Which guardrail catches 'domain code calls the database directly'?"* (An architecture test in the CI quality gate; review may catch it too.)

**Common misconception:** linters and formatters make code review unnecessary. (They free reviewers to focus on intent and design.)

**Re-teach trigger:** if learners can't say which layers are skippable, redraw the table with a red line under the last two.

**Transition — Say:** *"Every guardrail so far assumes a human types the code. Half of today's code will come from an AI. It needs its own guardrails."*

## SEGMENT 7: AI Guardrails and Instruction Files (20 min)

### The AI Usage Policy (6 min)

**Say:** *"Meridian's CISO will ask four questions about AI on this project. Which tools? What data goes into them? Who checks the output? And how do we know which code AI wrote? The AI usage policy answers all four, in `docs/ai-usage-policy.md`."*

**Write on board:**

| Question | Policy answer for ProcureFlow |
| --- | --- |
| Which tools? | Only tools on the approved list, used through accounts covered by the client's or the program's agreement. No personal accounts. |
| What data? | Code, synthetic test data and our own docs: yes. Real invoices, vendor bank details, personal data, secrets, production logs: never. |
| Who checks? | A human author owns every line. AI output goes through the same tests, CI and review as human code. |
| How do we know? | Every PR discloses AI assistance in the PR template. |

**Say:** *"The data rule is the one that ends careers. If you need a realistic invoice to debug, you make a fake one. If a log line might contain a bank number, it doesn't go in a prompt."*

**Show the disclosure section that goes into the PR template in Lab 2:**

```markdown
## AI assistance
- Used: [ ] None  [ ] Some (suggestions, snippets)  [ ] Substantial (most of the change)
- Tool(s): ___
- What I verified: [ ] Read every line  [ ] Tests cover the AI-written code  [ ] Checked new packages against DEPENDENCIES.md
- No client data or secrets were shared with the tool: [ ] Confirmed
```

### AI Instruction Files (10 min)

**Say:** *"Policy tells people what to do. Instruction files tell the AI what to do. Different assistants look for different files in the repo."*

| File | Read by | Purpose |
| --- | --- | --- |
| `CLAUDE.md` | Claude Code | Project rules and commands for Claude |
| `AGENTS.md` | Several coding agents that follow this open convention | Same rules, tool-neutral |
| `.github/copilot-instructions.md` | GitHub Copilot | Same rules for Copilot |

**Say:** *"Three files, one set of rules. Keep the shared rules identical, or they'll drift and each assistant will follow a different project. Some teams add a small CI check that compares them."*

**Live demo: show a starter `CLAUDE.md` and explain each section in one sentence.**

```markdown
# CLAUDE.md — ProcureFlow

## Project
Procure-to-pay app for Meridian Components. Web: Next.js (web/). API: .NET (api/). DB: SQL Server (db/).

## Commands
- API tests: dotnet test api/ProcureFlow.sln
- Web: npm --prefix web run dev | lint | test

## Rules
- Use terms from docs/domain/glossary.md exactly. Requisition, not PurchaseRequest.
- Follow CODING_STANDARDS.md. Folders follow bounded contexts.
- Every new behaviour needs a test named after the story and AC, e.g. PF001_AC2_...
- Log with the structured logger and correlation ID (LOGGING.md). Never log vendor bank details, tax IDs, tokens or connection strings.
- Never add a package without checking DEPENDENCIES.md; say which package and licence you propose.
- Never put secrets in code or config files; use environment variables / secret store.

## When unsure
Ask, and point to the doc that's unclear. Don't invent business rules.
```

**Say:** *"Three tips. Keep it short: it's read on every request, and long files get skimmed by models just like by people. Point to the detailed docs rather than copying them. And put commands in it, because an assistant that can run the tests can check its own work."*

### Comprehension Check (4 min)

1. *"A developer pastes a real failing invoice, with the vendor's bank details, into an AI chat to debug a match error. Which rule was broken, and what should they have done?"* (Data handling: no client data. Create a synthetic invoice that reproduces the error.)
2. *"Why have both `CLAUDE.md` and `.github/copilot-instructions.md`?"* (Different assistants read different files; the team may use more than one tool.)
3. *"What's wrong with a 400-line `CLAUDE.md` that copies every standard?"* (Too long to be followed reliably and drifts from the source docs; keep it short and link out.)

**Common misconception:** if the AI wrote it, the AI is responsible for it. (The human author owns every line.)

**Re-teach trigger:** if anyone answers question 1 with "it's fine if the tool is approved", restate: approved tool doesn't mean approved data.

**Transition — Say:** *"After lunch, you'll write the real guardrail set for your repo, including these files."*

### BREAK (30 min, lunch)

*Suggested break prompt:* *"Write down one rule you wish every AI assistant you've used had known about your code."*

## SEGMENT 8: Lab 2 — First Guardrail Set (45 min)

**Say:** *"Write your team's first guardrail set. Every file goes in through its own pull request, reviewed by the right code owner. You're not just writing rules; you're proving your Lab 1 setup works."*

### Instructions (read aloud, step by step)

1. **Split the work (3 min).** Each team member owns one or two files from the list below.
2. **Write (25 min).** Use the skeletons below. Keep every file short: if a section has no rule yet, write "TBD: owner, date" rather than padding.
3. **Review the starter files (5 min).** The starter kit ships first versions of `ARCHITECTURE.md`, `SECURITY.md`, `DEPENDENCIES.md` and `LOGGING.md`. One person reads each, fixes anything that contradicts Modules 1–2, and opens a PR.
4. **PRs and review (10 min).** One PR per file. Code owners review. Every PR uses the template and fills the AI disclosure honestly.
5. **Merge (2 min).** Merge everything that passes review. Anything still open stays open with a comment saying what's missing.

| File | Must contain |
| --- | --- |
| `README.md` | One-paragraph purpose; run locally (web, API, DB); how deploy to dev works; who to contact |
| `CONTRIBUTING.md` | Trunk-based branching and branch names; conventional commits with `Refs: PF-___`; PR and review rules; Definition of Ready |
| `CODING_STANDARDS.md` | Glossary naming; folders by bounded context; error format; money handling; test naming `PF___AC_` |
| `DEFINITION_OF_DONE.md` | Story-level and release-level checklists (from Module 2, updated) |
| `CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md` | Same shared rules in each; commands; "when unsure" rule |
| `docs/ai-usage-policy.md` | Approved tools; data handling; human ownership; disclosure |
| `.github/pull_request_template.md` | Add the AI assistance section from Segment 7 |

### Starter Skeletons

```markdown
# README.md
# ProcureFlow
Procure-to-pay app for Meridian Components: requisitions, approvals, POs, receiving, invoice matching.

## Run locally
1. Database: ___
2. API: ___
3. Web: ___

## Deploy
Every merge to main deploys to dev via .github/workflows/___. Test and prod: ___.

## Contacts
Tech lead: ___ · Product Owner: ___ · Security: ___
```

```markdown
# DEFINITION_OF_DONE.md
## Story is done when
- [ ] Merged to main through a reviewed PR; all CI checks green
- [ ] Every acceptance criterion has a passing test named PF___AC_
- [ ] Linked NFRs checked (docs/requirements/nfr.md)
- [ ] Audit log entries verified where relevant
- [ ] No new package without DEPENDENCIES.md approval
- [ ] Demonstrated to the Product Owner
- [ ] Traceability matrix updated

## Release is done when
- [ ] All stories in the release are done
- [ ] CHANGELOG.md updated
- [ ] Runbook updated for new operational steps
```

```markdown
# docs/ai-usage-policy.md
## Approved tools
## Data handling (never share)
## Human ownership and review
## Disclosure (see PR template)
## Reporting a problem
```

### Reference Solution (instructor, partial)

**Example `CONTRIBUTING.md` (illustrative — teams' wording will vary):**

```markdown
# CONTRIBUTING.md
## Branches
Trunk-based. Branch from main, name it <type>/PF-<id>-<short-name>, merge within 2 days.

## Commits
Conventional commits: feat|fix|docs|test|refactor|chore|ci(<context>): <description>
Footer: Refs: PF-<id>

## Pull requests
- Use the PR template; fill in the AI assistance section.
- 1 approval minimum; code owners must approve their paths.
- Rebase on main before merging; never force-push someone else's branch.

## Definition of Ready
Glossary terms · Given/When/Then AC · NFRs linked · estimated · fits one sprint
```

### Instructor Circulates

- **Instruction files that differ:** *"Diff your three AI files. Is the rule list identical?"*
- **Instruction files that are long essays:** *"Could the AI follow all of this on every request? Cut to rules and links."*
- **Rules that contradict earlier modules** ("PurchaseOrderRequest" in a naming example): send back to the glossary.
- **AI disclosure left blank on PRs written with AI:** *"Did you use an assistant for this file? Then say so."*
- **PRs merged without the code owner:** check branch protection requires code-owner review.

**Success check:** all seven items merged through reviewed PRs; the three AI instruction files share identical rules; the PR template includes the AI disclosure; no file contradicts the glossary or Module 2 requirements.

**Extension (fast teams):** write `docs/adr/0001-trunk-based-development.md`: context, decision, consequences, and when you'd revisit it (the SAP change window).

## SEGMENT 9: How LLM Coding Assistants Work (25 min)

### Four Ideas That Explain Most AI Mistakes (10 min)

**Say:** *"You don't need to know how a model is trained to use it well. You need four ideas. Each one explains a mistake you've already seen an assistant make."*

- **Context.** *"The model only knows what's in front of it right now: your prompt, the files the tool has loaded, the instruction file, the conversation so far. It does not know your glossary unless something put the glossary into its context. That's why instruction files exist."*
- **Tokens.** *"Models read text in chunks called tokens, roughly three-quarters of an English word each. Every model has a limit on how many tokens it can consider at once, and every token costs time and money. Very long sessions push early details out of view, so start a fresh session for each new task."*
- **Hallucination.** *"A model predicts plausible text. Plausible isn't the same as true. It can invent a method that doesn't exist, a config option, or a whole package name. Invented package names are a real security risk: attackers publish packages with names models tend to invent."*
- **Non-determinism.** *"Ask the same question twice and you can get two different answers. So you can't trust one good run. You trust tests."*

**Write on board:**

> Context → give it what it needs. Tokens → keep it focused. Hallucination → verify. Non-determinism → test.

### Live Demo: Two Quick Failures (6 min)

**Demo 1 — missing context.** In a fresh assistant session with no repo files loaded, ask:

```
Add a status field to our requisition model.
```

**Example output (illustrative — actual AI output will vary run to run):** a new `PurchaseRequest` class with a `string Status` property, in a folder the repo doesn't have.

**Say:** *"Not wrong code. Wrong project. It had no context, so it guessed."*

**Demo 2 — hallucinated package.** Ask:

```
Which NuGet package should I use to generate Indian GST e-invoices in .NET?
```

**Example output (illustrative):** a confident package name with install instructions.

**Say:** *"Now search the package registry for it, live."* (It may exist, may not, or may be unmaintained.) *"Either way, `DEPENDENCIES.md` decides, not the assistant. Never install a package you haven't looked up yourself."*

### Prompt Engineering for Engineers (7 min)

**Say:** *"A good engineering prompt is a mini-spec. Five parts."*

| Part | What to give | Example for PF-003 |
| --- | --- | --- |
| Context | Which files, which story, which stack | "PF-003 in docs/requirements/stories; API in api/src/Requisitioning" |
| Task | One specific job | "Add GET /requisitions/{id}/status" |
| Constraints | Rules to follow; point to the docs | "Follow CODING_STANDARDS.md and LOGGING.md; glossary names" |
| Output | What form you want back | "Code plus xUnit tests named PF003_AC1_…; list files changed" |
| Verify | How it should check itself | "Run dotnet test and tell me the result; list assumptions" |

**Show bad vs good side by side:**

```
BAD:  make an endpoint for requisition status

GOOD: Implement PF-003 (docs/requirements/stories/PF-003-requisition-status.md).
      Add GET /requisitions/{id}/status in api/src/Requisitioning.
      Follow CODING_STANDARDS.md and LOGGING.md. Use glossary terms.
      Return 404 problem details for an unknown id.
      Write xUnit tests for each acceptance criterion, named PF003_AC<n>_<behaviour>.
      Don't add packages. List any assumptions. Then run dotnet test.
```

**Say:** *"Good prompts get reused. Save them in `docs/prompts/` as the team's prompt library, so the next person starts from the best version, not from scratch."*

### Comprehension Check (2 min)

1. *"An assistant generated code that calls `requisition.ApproveAll()`, which doesn't exist. Which of the four ideas explains it, and what catches it?"* (Hallucination; the compiler and tests catch it.)
2. *"Your prompt worked perfectly once. Why isn't that enough to put it in the prompt library?"* (Non-determinism: run it a few times and check the output against standards before trusting it.)

**Common misconception:** a better model removes the need for context and verification.

**Re-teach trigger:** if learners blame the model rather than the missing context in Demo 1, rerun it with the story file and `CLAUDE.md` loaded and compare.

**Transition — Say:** *"That comparison is exactly the next lab. Let's measure what your instruction files are worth."*

## SEGMENT 10: Lab 3 — Prompt Clinic (35 min)

**Say:** *"You've spent a morning writing rules for the AI. Are they worth anything? Run the same task with and without your instruction files, score both outputs against your standards, and find out."*

### Instructions (read aloud, step by step)

1. **Set up two conditions (5 min).** In pairs.
   - **Condition A (no guardrails):** a scratch copy of the repo with `CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `CODING_STANDARDS.md` and `docs/` removed.
   - **Condition B (guardrails):** your real repo on a new branch `lab3/prompt-clinic`.
2. **Run the same prompt (10 min).** Use the prompt below, word for word, in a fresh assistant session for each condition. Run each condition **twice** (fresh session each time), because of non-determinism.
3. **Score (10 min).** Score all four outputs with the rubric. Don't fix anything; you're measuring, not building.
4. **Improve the prompt (7 min).** Rewrite the prompt using the five-part structure from Segment 9. Run it once in Condition B and score it.
5. **Save (3 min).** Record scores in `docs/prompts/clinic-results.md`. Save your best prompt as `docs/prompts/implement-story.md`. Don't merge the generated code; CI-3 builds the real version.

### The Clinic Prompt (deliberately plain; use exactly)

```
Implement user story PF-001: a requester submits a requisition with item,
quantity, amount and cost centre. Add the API endpoint and tests.
```

**Say:** *"The prompt is plain on purpose. If the output follows your standards in Condition B, the instruction files did that work, not the prompt."*

### Scoring Rubric (0 = no, 1 = partly, 2 = yes)

| # | Criterion | Where the rule lives |
| --- | --- | --- |
| 1 | Uses glossary names (`Requisition`, `CostCentre`, `Requester`) | `glossary.md` |
| 2 | Files in the bounded-context folder (`api/src/Requisitioning`) | `CODING_STANDARDS.md` |
| 3 | Money as `decimal` with currency | `CODING_STANDARDS.md` |
| 4 | Validation errors returned as problem details | `CODING_STANDARDS.md` |
| 5 | Structured logging with correlation ID; nothing sensitive logged | `LOGGING.md` |
| 6 | Tests named `PF001_AC<n>_…`, one per acceptance criterion | `CODING_STANDARDS.md`, story file |
| 7 | Audit-log entry on submit | Story AC3, `nfr.md` |
| 8 | No new packages, or a package proposed with its licence | `DEPENDENCIES.md` |

Maximum: 16 per output.

### Expected Results (instructor)

**Example (illustrative — actual scores will vary by tool, model and run):**

| Condition | Run 1 | Run 2 | Typical misses |
| --- | --- | --- | --- |
| A — no guardrails | 5 | 6 | `PurchaseRequest` naming; `double` for money; no audit log; tests named `Test1` |
| B — guardrails | 12 | 11 | Audit log sometimes missed; one test missing for AC2 |
| B + improved prompt | 14 | — | Usually only small deviations remain |

**Say (during the debrief):** *"Look at three things. The gap between A and B is what your instruction files are worth. The gap between run 1 and run 2 is non-determinism, which is why tests, not one good run, are the guardrail. And whatever B still misses tells you which rule is unclear or missing in your files. Fix the file, not just the code."*

### Instructor Circulates

- **Condition A still has the docs:** the assistant may find them anyway; check the files are really gone from the scratch copy.
- **Learners fixing code instead of scoring:** *"Measure first. Fixing comes in CI-3."*
- **Same session reused for both runs:** the second run then sees the first; insist on fresh sessions.
- **Scores with no evidence:** each score needs a file name or line as evidence in `clinic-results.md`.

**Success check:** four scored outputs plus the improved-prompt run, each with evidence; at least one instruction-file fix proposed from the misses; best prompt saved in `docs/prompts/`.

**Extension (fast pairs):** open a PR that fixes the rule most often missed in Condition B (for example, add "every submit and approve action writes an audit entry" to the AI instruction files), then rerun Condition B once and see whether the score moves.

**Transition — Say:** *"Generating code is one use. After the break: debugging, refactoring, tests, docs and review."*

### BREAK (10 min)

*Suggested break prompt:* *"When an AI assistant has helped you debug before, what did you give it, and what did it get wrong?"*

## SEGMENT 11: AI Across the Development Workflow (20 min)

### Six Uses, Six Risks (7 min)

**Say:** *"AI helps at every stage of the workflow. Each use has one typical way of going wrong, and one guardrail that catches it."*

**Put on screen:**

| Use | How to ask | Typical failure | Guardrail |
| --- | --- | --- | --- |
| Code generation | Story file + standards + "write tests too" | Plausible code in the wrong style or with invented APIs | Instruction files, compiler, tests, review |
| Debugging | Stack trace + log lines + code + expected vs actual; ask for hypotheses first | Fixes the symptom, hides the cause | A failing test before the fix |
| Refactoring | "Refactor X without changing behaviour; tests must still pass" | Silently changes behaviour | Tests in place *before* refactoring |
| Test generation | Acceptance criteria in Given/When/Then | Tests that assert the code's current behaviour, even when it's wrong | Tests derived from AC, not from the code; watch them fail first |
| Documentation | "Draft the README run section from these scripts" | Confident instructions that don't work | Follow the steps on a clean machine |
| Code review | PR diff + PR checklist | Noisy comments; misses domain intent | AI is a first reviewer; a human code owner approves |

### Live Demo: Debug the Right Way (8 min)

**Say:** *"Watch the difference between 'fix this' and a proper debugging prompt. The bug: the requisition list page shows a blank status for every row."*

**Weak prompt:**

```
The status column is empty. Fix it.
```

**Example output (illustrative — actual AI output will vary run to run):** adds `status ?? "Unknown"` in the React component. The column now shows "Unknown" everywhere. The bug is hidden, not fixed.

**Strong prompt:**

```
Bug: requisition list shows blank status for every row.
Expected: "Awaiting approval" for new requisitions (PF-003 AC1).
Evidence:
- Browser network tab: GET /requisitions returns [{"Id":"…","Status":"AwaitingApproval"}]
- web/src/requisitions/List.tsx reads row.status
Give me your top 3 hypotheses, ranked, with how to confirm each.
Then write a failing test that reproduces the most likely one, before any fix.
```

**Example output (illustrative):** hypothesis 1, the API returns PascalCase (`Status`) while the frontend reads camelCase (`status`); confirm by checking the JSON serializer settings in the API's startup code.

**Say:** *"Three habits in that prompt: real evidence, hypotheses before fixes, and a failing test first. The failing test proves you found the cause. When it passes, it proves you fixed it. And it stays in the suite so the bug can't come back."*

**Say:** *"And notice what wasn't in the prompt: no real vendor names, no production log dump. Synthetic evidence only."*

### Comprehension Check (5 min)

1. *"An AI generates tests by reading your finished code. All pass. What's the risk?"* (The tests may encode existing bugs as expected behaviour; derive tests from acceptance criteria and see them fail first.)
2. *"Why ask for hypotheses before a fix?"* (It forces reasoning about the cause and gives you something to verify, instead of a patch over the symptom.)
3. *"Can an AI reviewer approve a PR?"* (No. It can comment; a human code owner approves under branch protection.)

**Common misconception:** if the tests pass after the AI's fix, the bug is fixed. (Only if a test reproduced the bug first.)

**Re-teach trigger:** if most accept the `?? "Unknown"` fix as fine, ask: *"What does Priya's requester see now? Is that what PF-003 says?"*

**Transition — Say:** *"Three bugs are hiding in a fuller version of ProcureFlow. Let's race."*

## SEGMENT 12: Lab 4 — Debugging Race (35 min)

**Say:** *"Three bugs are seeded on the branch `race/seeded-bugs`. Use AI as much as you like. But a bug only counts as fixed when a test reproduced it first, the fix makes that test pass, and the PR is merged through review. First team to merge each verified fix scores a point. Speed without proof scores nothing."*

### Rules (read aloud)

1. Fork the instructor's `procureflow-race` repo: a pre-built ProcureFlow with routing, approvals and a list page, with the bugs on the branch `race/seeded-bugs`. Copy your Lab 1 branch protection and CODEOWNERS onto the fork (5 minutes).
2. For each bug: **failing test first → fix → green → PR**. The PR shows the test failing on the first commit and passing on the second.
3. Use the strong debugging prompt pattern from Segment 11. Synthetic data only; no pasting production-style logs with real names.
4. Commit with conventional commits and the story ID. Fill in the AI disclosure.
5. A code owner from another pair reviews. The instructor confirms each merge.

### Bug Reports (give to learners)

**Bug 1 — "₹50,000 requisitions go to the department head" (PF-010)**

> A requester in Chennai submitted a requisition for exactly ₹50,000. It went to her line manager and then her department head. Priya says requisitions up to ₹50,000 need the line manager only.

**Bug 2 — "Approve button gives an error for some requisitions" (PF-002)**

> The line manager clicked Approve on REQ-1042 and saw "Something went wrong. Reference: 7f3c9a2e". Other requisitions approve fine.

Evidence (from the dev logs, found by searching the correlation ID):

```
{"level":"Error","correlationId":"7f3c9a2e","message":"Unhandled exception approving requisition","requisitionId":"REQ-1042"}
System.NullReferenceException: Object reference not set to an instance of an object.
   at ProcureFlow.Api.Requisitioning.ApprovalService.ApproveAsync(Guid requisitionId, String approverId)
      in /src/api/src/Requisitioning/ApprovalService.cs:line 42
```

**Bug 3 — "Some requisitions appear twice in the list" (PF-003)**

> On the requisition list page, a few requisitions show up twice, once as "Awaiting approval" and once as "Approved". It seems to happen only for bigger amounts.

**Say (when the race starts):** *"Notice Bug 2: the user saw only a reference number, the log had the detail, and the correlation ID joined them. That's `LOGGING.md` paying off."*

### Answer Key (instructor only)

**Bug 1 — off-by-one at the boundary.**

```csharp
// Seeded (wrong)
if (amount < 50_000m) return [Approver.LineManager];
// Fixed
if (amount <= 50_000m) return [Approver.LineManager];
```

```csharp
[Fact]
public void PF010_AC4_exactly_50000_goes_to_line_manager_only()
{
    var route = ApprovalRouting.For(50_000m);
    Assert.Equal(new[] { Approver.LineManager }, route);
}
```

This is Module 2's boundary AC4, which is exactly why it was written.

**Bug 2 — missing cost centre on older data.** Line 42 reads `var budgetCode = requisition.CostCentre.Code;`. REQ-1042 was seeded before cost centre became required, so `CostCentre` is null.

**Watch for the AI's likely fix:** `requisition.CostCentre?.Code`. It removes the crash but lets an approval go through with no budget check. That hides the problem.

**Correct fix:** return a validation problem ("This requisition has no cost centre; ask the requester to add one") and add an open question for Priya: *"What happens to older requisitions without a cost centre?"*

```csharp
[Fact]
public async Task PF002_approving_requisition_without_cost_centre_returns_validation_problem()
{
    var req = TestData.RequisitionWithoutCostCentre();
    var result = await _service.ApproveAsync(req.Id, "manager-1");
    Assert.True(result.IsValidationProblem);
}
```

**Bug 3 — join multiplies rows.** The list query joins every approval step:

```sql
-- Seeded (wrong): one row per approval step
SELECT r.Id, r.Item, r.Amount, a.Status
FROM Requisitions r
LEFT JOIN Approvals a ON a.RequisitionId = r.Id;
```

Requisitions above ₹50,000 have two or more approval steps, so they appear more than once. **Fix:** read the current status from `Requisitions.Status` (or take only the latest approval step with `OUTER APPLY (SELECT TOP 1 … ORDER BY a.Step DESC)`). **Test:** an integration test that creates a requisition with two approval steps and asserts the list returns it once. Adapt to LINQ if your starter kit uses EF Core.

### Instructor Circulates

- **Fix with no failing test first:** doesn't count. *"Show me the red run."*
- **`?.` accepted for Bug 2:** *"What does the approver approve now, and what does audit see?"*
- **Real-looking data pasted into prompts:** stop and point at the AI usage policy.
- **AI explanation accepted without checking:** *"How did you confirm hypothesis 1?"*

**Success check:** each team merges at least two verified fixes; every fix has a test that failed before and passes after; Bug 2's open question is added to `docs/domain/open-questions.md`.

**Extension (fast teams):** ask the AI to review your Bug 3 PR against the PR checklist. Compare its comments with your human reviewer's. Which caught more that mattered?

## SEGMENT 13: CI-3 — Walking Skeleton to Dev (30 min)

**Say:** *"Everything today comes together here. Build the thinnest end-to-end slice of PF-001 and PF-003 (a Next.js page, a .NET API, a SQL Server table) inside your guardrails, through review, and deploy it to dev with the starter pipeline. The starter kit has done the wiring; your job is the slice."*

### Instructions (read aloud, step by step)

1. **Branch (1 min).** `feat/PF-001-submit-requisition`.
2. **Build with AI (15 min).** Use your saved prompt from `docs/prompts/implement-story.md`, with the instruction files in place. Split across the team:
   - **Database:** a migration in `db/` creating the `Requisitions` table (Id, Item, Quantity, Amount, Currency, CostCentre, Status, CreatedBy, CreatedAt).
   - **API:** `POST /requisitions` and `GET /requisitions/{id}/status` in `api/src/Requisitioning`, with an audit-log entry on submit.
   - **Web:** a page in `web/` with the submit form and a status view.
   - **Tests:** one test per acceptance criterion, named `PF001_AC<n>_…` and `PF003_AC<n>_…`.
3. **PR and review (7 min).** Conventional commit, `Refs: PF-001, PF-003`, PR template filled in including AI disclosure. Code owners for `api/` and `web/` review.
4. **Deploy and smoke test (5 min).** Merge. Watch the starter pipeline deploy to dev. Submit one requisition on the dev URL and see its status.
5. **Trace (2 min).** Update the PF-001 and PF-003 rows of `docs/requirements/traceability.md` with the PR number, test names and "dev".

**Say (at minute 15):** *"If you're not close, cut scope, not guardrails. A form with two fields that really reaches the database through review beats a full form that skipped the tests."*

### CI-3 Checklist (put on screen)

- [ ] Guardrail `.md` set merged through reviewed PRs (Lab 2)
- [ ] `docs/ai-usage-policy.md` and the AI disclosure section in the PR template
- [ ] Branch protection, CODEOWNERS, PR template and pre-commit hooks active (Lab 1)
- [ ] Walking skeleton (Next.js page, .NET API, SQL Server table) merged and deployed to dev
- [ ] Tests for PF-001 and PF-003 acceptance criteria passing in CI
- [ ] Traceability matrix updated for PF-001 and PF-003

### Instructor Circulates

- **Pipeline red:** read the failing step with the team; don't bypass it. A red pipeline is the guardrail working.
- **Tempted to merge without review "to save time":** branch protection will stop them; point at the opening story.
- **No audit entry on submit:** it's in the acceptance criteria and `nfr.md`.
- **Secrets in `appsettings.json` for the dev database:** use the pipeline's secret store, as the starter kit shows.

**Success check:** a requisition submitted on the dev URL shows "Awaiting approval"; the PR shows review, green CI and AI disclosure; the traceability rows are filled in.

## SEGMENT 14: Summary and Wrap-Up (10 min)

### What We Covered Today (2 min)

- Why guardrails: keep the team, its AI tools and the client aligned on scope, quality and risk
- Documentation as code, and what each file in the project `.md` set governs
- Trunk-based development, rebasing, resolving conflicts and conventional commits
- Process guardrails: Definition of Done in the repo, change control, branch protection, CODEOWNERS, PR templates
- Technical guardrails in layers: standards, linters, hooks, secret scanning, CI quality gates, architecture tests
- AI guardrails: usage policy, data handling, instruction files, disclosure
- How LLM assistants work (context, tokens, hallucination, non-determinism) and how to prompt them like a spec
- AI for generating, debugging and testing code, with a failing test as proof

### Back to the Opening (3 min)

**Point at this morning's "Week 3 at Meridian" board.** For each problem, ask the class which guardrail now prevents it.

(Expected: `PurchaseRequest` naming → glossary + instruction files + review. API key in history → pre-commit and CI secret scanning. Bank details in logs → `LOGGING.md`, instruction files, review. Unapproved licence → `DEPENDENCIES.md` + PR checklist. Priya's hallway request → change control through a story PR.)

### Exit Ticket (3 min, written, one per learner)

> *"A new developer joins tomorrow and uses an AI assistant you've never seen. List three things in your repo that will keep their AI-generated code inside your team's rules, and one thing that would still need a human to catch."*

**Expected answer:** instruction files (or `AGENTS.md`) the tool reads; CI quality gate with tests, lint, secret scan and architecture tests; branch protection with code-owner review. Still needs a human: whether the code matches the business intent (for example, the Bug 2 cost-centre decision).

**Re-teach trigger:** if more than a quarter list only documents (no enforced checks), open the next module with a 5-minute "skippable or not?" recap of the defence-in-depth table.

### Bridge to What Comes Next (1 min)

**Say:** *"This morning, three competent developers and their AI tools made a mess. Now your repo enforces your rules for people and AI alike, and a real slice of ProcureFlow is running in dev. Remember: if it isn't written in the repo, neither your team nor your AI will follow it. Next, we build the rest of Sprint 1 on this skeleton, and every feature will go through the guardrails you set up today."*

### Homework / Self-Practice

1. **Close the gaps.** Fix anything left unchecked on the CI-3 checklist through PRs.
2. **Tighten one instruction rule.** Pick the rule your Lab 3 Condition B missed most. Rewrite it in all three instruction files, rerun the clinic prompt twice, and record whether the score improved.
3. **Write an ADR.** `docs/adr/0002-<topic>.md` for one decision made today (for example, how the walking skeleton handles missing cost centres), with context, decision, consequences and who decided.

## Q&A & Doubt Solving

**Likely questions and suggested answers:**

**Q: Isn't all this process too heavy for a small FDE team?** → Most of it is set up once and then runs by itself: branch protection, hooks and CI cost nothing per change. Keep files short; delete any file nobody reads. The cost of one leaked secret or one audit finding is far higher.

**Q: Do we really need three AI instruction files?** → Only for the tools the team actually uses. If everyone uses one assistant, keep its file plus `AGENTS.md` as the tool-neutral copy. Whatever you keep, keep the rules identical.

**Q: Can I use my personal AI account if it's faster?** → No. Only approved tools through covered accounts. The data-handling promises the client relies on come from those agreements, not from your settings.

**Q: How do I know if code was AI-generated in someone else's PR?** → The PR disclosure says so. Treat it as information, not a judgement: AI-written code gets the same review and tests as any other code, with extra care on new packages and unfamiliar APIs.

**Q: Should AI ever merge or approve PRs?** → No. AI can draft, suggest and comment. A human code owner approves under branch protection, and the human author owns every line.

**Q: Why trunk-based if GitFlow feels safer?** → Short branches mean small, frequent merges and fewer painful conflicts. Safety comes from tests, CI and feature flags, not from long-lived branches. Use a release branch only when a client constraint demands it, and record why in an ADR.

**Q: What if the AI keeps ignoring a rule in CLAUDE.md?** → Check that the rule is short, specific and near the top; that it doesn't conflict with another rule; and that a test or linter backs it up. Instruction files steer; tests enforce.

## Instructor Notes

- **Prepare the starter kit and the race repo before class.** Confirm learners can create repos from the template, the starter pipeline deploys to dev, and `procureflow-race` has the three seeded bugs on `race/seeded-bugs`.
- **Check GitHub permissions.** Learners need admin rights on their team repo to set branch protection and rulesets. Some features (for example push protection) depend on the organisation's plan.
- **Check AI tool access.** Approved assistants available on all laptops, signed in through covered accounts. Have a backup set of pre-captured outputs for Lab 3.
- **Common mistake:** admin bypass left enabled on branch protection, so the repo owner can still push to `main`.
- **Common mistake:** secret scanning only in a local hook, never in CI.
- **Common mistake:** three AI instruction files that slowly disagree.
- **Common mistake:** accepting an AI fix without a failing test first (watch for `?.` on Bug 2).
- **Engagement tip:** the strongest moments are the first blocked secret in Lab 1 and the A-versus-B scores in Lab 3. Pause the room for both.
- **Time check:** if running behind before lunch, demo the rebase conflict from a pre-recorded terminal session rather than live.
- **Materials to prepare:** starter kit template; `procureflow-race` repo; a fake token for the secret test (never a real one); scoring sheets for Lab 3; dev environment URL for each team.

## Common Mistakes — Quick Reference

| Mistake | Symptom | Fix |
| --- | --- | --- |
| Rules only on a wiki | AI and reviewers don't follow them | Move them into the repo as `.md` files, reviewed through PRs |
| Admin bypass on `main` | Owner pushes directly | Include administrators / disallow bypass |
| Secret scan only in pre-commit | `--no-verify` gets secrets through | Run the scan in CI too; enable push protection |
| Long, drifting instruction files | AI follows some rules, ignores others | Short rule lists, identical across files, linking to source docs |
| Client data pasted into prompts | Policy breach | Synthetic data only; check before every paste |
| Fix without a failing test | Bug returns, or symptom hidden | Failing test first, then fix, then green |
| Tests generated from the code | Tests encode the bug | Generate tests from acceptance criteria |
| Installing an AI-suggested package unchecked | Licence or security risk | Look it up; follow `DEPENDENCIES.md` |
| Rebasing a shared branch | Teammates' history rewritten | Rebase only your own branch; `--force-with-lease` |

## Timing Contingencies

| Situation | Action |
| --- | --- |
| Running 10 min behind before the first break | Pre-recorded rebase demo; skip comprehension question 3 in Segment 3 |
| GitHub admin settings unavailable to learners | Instructor configures branch protection on all team repos in advance; learners verify with the attack tests |
| Lab 2 running long | Require README, CONTRIBUTING, DoD and the AI files; others become homework |
| AI tools unavailable | Use pre-captured outputs for Labs 3 and 4; the scoring and the test-first fix still work |
| Starter pipeline fails on deploy | Merge through review anyway; demo the deploy from the instructor's repo; teams redeploy as homework |
| Low energy after lunch | Start Lab 2 with a 3-minute "worst rule you've ever seen" round, then write |
| Advanced team finishes early | Add an architecture test for the web project (e.g., UI code may not call SQL directly) |

## End-of-Session Quiz (5 Questions)

1. Name the file in the project `.md` set that answers each question: "Can I add this package?", "Can I log the vendor ID?", "What must be true before this story ships?"
2. A developer commits a secret with `git commit --no-verify`. Which guardrail still stops it reaching `main`, and why?
3. Give the four ideas that explain most AI coding-assistant mistakes, and the matching habit for each.
4. An AI suggests fixing a `NullReferenceException` with `?.`. What should you check before accepting it?
5. Write a conventional commit message for fixing the bug where ₹50,000 requisitions went to the department head.

**Answer key (instructor):**

1. `DEPENDENCIES.md`; `LOGGING.md`; `DEFINITION_OF_DONE.md`.
2. The CI secret scan as a required status check under branch protection (and push protection, where enabled). Local hooks can be skipped; required CI checks can't.
3. Context → give it what it needs (instruction files, story, files). Tokens → keep sessions focused. Hallucination → verify APIs and packages. Non-determinism → rely on tests, not one good run.
4. Whether a null there is valid business data or a symptom of a missing rule; write a failing test for the real expected behaviour first. `?.` may hide the problem (as in Bug 2).
5. `fix(requisitioning): route exactly ₹50,000 to line manager only` with footer `Refs: PF-010`.

## Homework Rubric

| Criterion | Excellent (4) | Good (3) | Needs Work (2) | Incomplete (1) |
| --- | --- | --- | --- | --- |
| CI-3 gaps closed | Every checklist item done through reviewed PRs, CI green | One item outstanding with a clear plan | Several items outstanding | Not attempted |
| Instruction rule tightened | Rule rewritten identically in all AI files; clinic rerun twice; scores and evidence recorded | Rule rewritten and rerun once | Rule rewritten but not tested | Not attempted |
| ADR | Clear context, decision, consequences and decider; linked from the relevant story or PR | Mostly complete; one part thin | Describes the change but not the decision or reasons | Not attempted |

**Total:** /12. Pass threshold: 8/12.

## Materials Checklist

- [ ] Opening slide: "Week 3 at Meridian"
- [ ] Starter kit template repo verified (web, api, db, pipeline deploys to dev)
- [ ] `procureflow-race` repo with three seeded bugs on `race/seeded-bugs`
- [ ] Prepared two-branch repo for the rebase conflict demo (or a pre-recorded terminal session)
- [ ] Fake token for the secret-scanning test (never a real credential)
- [ ] Learners have admin rights on their team repos
- [ ] Approved AI assistants available and signed in through covered accounts
- [ ] Lab 3 scoring sheets and backup pre-captured AI outputs
- [ ] Dev environment URL per team
- [ ] Timer visible for all labs
