# FDE Full-Stack Engineering Program — Lecture Scripts

Instructor-ready lecture scripts for the 100-hour FDE program (16 modules, 5 phases, 40 h concepts and demos, 60 h hands-on). Every module ships one Capstone Increment (CI-n) of **ProcureFlow**, a procure-to-pay platform for Meridian Components (Pune, Chennai, Leeds).

Source of truth for modules, durations and capstone increments: the program syllabus, *FDE Engineering Curriculum v2* (kept outside this repository). Modules 1–5 are the gold-standard scripts; Modules 6 onwards follow their structure.

## Phase 1: Business domain, requirements and guardrails (Focus area 1)

| Module | Duration (concept / hands-on) | Capstone increment | Script |
| --- | --- | --- | --- |
| 1. FDE Role and Business Domain Understanding | 5 h (2 / 3) | CI-1: domain glossary, event storming map, user personas and volumes | [Module-01](Phase-1/Module-01-FDE-Role-and-Business-Domain.md) |
| 2. Requirements Gathering, User Stories and Estimation | 5 h (2 / 3) | CI-2: story map, prioritised backlog, NFR catalogue, release plan | [Module-02](Phase-1/Module-02-Requirements-Stories-and-Estimation.md) |
| 3. Project Guardrails, .md Files and AI-Assisted Development | 6 h (3 / 3) | CI-3: repo guardrail set, walking skeleton deployed | [Module-03](Phase-1/Module-03-Guardrails-and-AI-Assisted-Development.md) |

## Phase 2: Decide and build (Focus area 2)

| Module | Duration (concept / hands-on) | Capstone increment | Script |
| --- | --- | --- | --- |
| 4. Technology Selection and Solution Architecture | 5 h (3 / 2) | CI-4: decision matrix, C4 diagrams, ADRs, cost estimate | [Module-04](Phase-2/Module-04-Technology-Selection-and-Architecture.md) |
| 5. Framework Building and Dependency Management | 6 h (2.5 / 3.5) | CI-5: platform packages, solution template, dependency policy, SBOM, RAID log | [Module-05](Phase-2/Module-05-Framework-Building-and-Dependency-Management.md) |
| 6. Frontend Implementation: React and Next.js | 8 h (3 / 5) | CI-6: component library, request screens, Next.js app shell with BFF and protected routes | [Module-06](Phase-2/Module-06-Frontend-Implementation-React-Nextjs.md) |
| 7. Backend Implementation: C#, .NET and REST APIs | 9 h (4 / 5) | CI-7: domain model, Catalog and Request APIs with OpenAPI, gateway, health checks, resilience policies | [Module-07](Phase-2/Module-07-Backend-Implementation-CSharp-REST-APIs.md) |
| 8. Data Layer: SQL Server and Data Access | 5 h (2 / 3) | CI-8: production schema, migrations, repositories, audit tables, tuned spend query | [Module-08](Phase-2/Module-08-Data-Layer-SQL-Server.md) |

## Phase 3: Server-side and serverless programming (Focus area 3)

| Module | Duration (concept / hands-on) | Capstone increment | Script |
| --- | --- | --- | --- |
| 9. Server-Side Programming | 5 h (2 / 3) | CI-9: background worker, real-time status updates, distributed and output caching | [Module-09](Phase-3/Module-09-Server-Side-Programming.md) |
| 10. Serverless Programming with Azure Functions | 7 h (3 / 4) | CI-10: three production-ready functions (notifications, escalation, spend report) with local tests and a server-versus-serverless ADR | [Module-10](Phase-3/Module-10-Serverless-Azure-Functions.md) |

## Phase 4: Log files, security and non-functional testing (Focus area 5)

| Module | Duration (concept / hands-on) | Capstone increment | Script |
| --- | --- | --- | --- |
| 11. Logging, Log File Analysis and Observability | 6 h (2.5 / 3.5) | CI-11: logging standard implemented, centralised logs, end-to-end traces, dashboards, alerts and a log analysis report | [Module-11](Phase-4/Module-11-Logging-and-Observability.md) |
| 12. Application Security and DevSecOps | 6 h (2.5 / 3.5) | CI-12: SSO with MFA, RBAC and policy checks, secrets via managed identity, security logging, DevSecOps pipeline with no critical or high findings | [Module-12](Phase-4/Module-12-Application-Security-DevSecOps.md) |
| 13. Testing and Non-Functional Testing | 7 h (3 / 4) | CI-13: functional suites plus load, stress, soak, resilience and accessibility reports | Not yet written |

## Phase 5: Deploy, operate and support (Focus area 4)

| Module | Duration (concept / hands-on) | Capstone increment | Script |
| --- | --- | --- | --- |
| 14. CI/CD, Containers and Cloud Deployment | 7 h (2.5 / 4.5) | CI-14: infrastructure as code, Helm and Functions deployment, environment promotion | Not yet written |
| 15. Production Support and Operations | 4 h (1.5 / 2.5) | CI-15: readiness review, runbook, incident drill, support handover pack | Not yet written |
| 16. End-to-End Capstone: Release and Presentation | 9 h (1.5 / 7.5) | v1.0 production release, final demo and presentation | Not yet written |

Capstone increments above are abbreviated from the syllabus; each script's CI review checklist holds the full increment.

## Case facts kept consistent across scripts

The scripts follow the Meridian case established in Modules 1–5, which differs from the syllabus's generic capstone profile in a few places. Where they differ, the client's figures win:

| Fact | Syllabus (generic) | ProcureFlow at Meridian (used in scripts) |
| --- | --- | --- |
| Users | 5,000 employees, ~300 concurrent | 1,377 users, ~140 concurrent at month-end (assumption) |
| Audit retention | 7 years | 8 years (NFR-AUD-01) |
| Approval rules | Manager, then finance above a threshold | ₹50,000 line manager; to ₹5 lakh + department head; above + CFO |
| Architecture | Services behind a gateway | Modular monolith (ADR-0003) plus a Catalogue edge service and YARP gateway (ADR-0007, Module 7) |

## ADR register (as introduced in the scripts)

| ADR | Decision | Module |
| --- | --- | --- |
| 0003 | Modular monolith with modules per bounded context | 4 |
| 0004 | Azure App Service (India region) for web and API | 4 |
| 0005 | Azure SQL Database; Service Bus to SAP; audit archive to storage | 4 |
| 0006 | Frontend state and rendering (homework) | 6 |
| 0007 | Catalogue as a separate service, behind a YARP gateway (homework) | 7 |
| 0008 | Audit retention and archiving (homework) | 8 |
| 0009 | Shared state for scale-out: caches, SignalR backplane, keys (homework) | 9 |
| 0010 | Server versus serverless, per workload (Lab 4) | 10 |
| 0011 | Identity and access: Entra ID, BFF tokens, roles, managed identity (homework) | 12 |
