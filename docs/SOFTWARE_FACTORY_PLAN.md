# Studex Software Factory Plan

Status: Deployment design
Last updated: 2026-11-06

## Decision

Build the factory around one control plane, separate execution computers, and
explicit systems of record:

- **Paperclip** is the factory control plane for companies, goals, agent roles,
  tasks, heartbeats, budgets, approvals, and audit history.
- **Orgo** provides persistent client and agent computers.
- **CUA** provides isolated computer-use sandboxes for browser and desktop work,
  including bring-your-own GCP capacity. It is an execution plane, not the task
  database.
- **Rome** is evaluated as the persistent agent workspace, workflow, and custom
  application layer. It must not become a second control plane during the first
  deployment.
- **Hermes and Codex** are workers attached to Paperclip through approved
  adapters. GitHub Copilot remains a developer tool inside the code workflow;
  it is not an autonomous approver.
- **Buzz** is the Eswatini human/agent conversation room. Slack and email are
  intake and notification channels.
- **GitHub** is the source of truth for code, pull requests, releases, and
  repository evidence.
- **Google Drive** is the source of truth for approved client documents and
  deliverables.
- **Linear** is the engineering delivery view. **ClickUp** may be the client and
  operations view. **Asana** should be read-only or migrated unless a named team
  requires it as its operating view.
- **Notion**, including the Claudio directory, is the executive catalogue,
  strategy, roster, and linked dashboard. It is not the runtime task database.

This prevents circular sync, duplicated tasks, conflicting statuses, and agents
acting on stale instructions.

## Factory flow

`intake -> qualify -> specification -> architecture -> build -> test/evidence`
`-> review/approval -> deploy -> operate -> retrospective/memory`

No build enters the factory without a named client or internal sponsor, outcome,
owner, data classification, budget, acceptance criteria, and approval route.

## Canonical record

Every work item carries:

- `tenant_id`, `project_id`, `spec_id`, and `task_id`;
- accountable human, assigned agent, status, priority, and deadline;
- source channel and requestor identity;
- repository, branch, pull request, and release links where applicable;
- allowlisted Drive folder and output artifact links;
- Buzz group or Slack channel mapping;
- budget, model, token/cost use, and retry count;
- approval state, test evidence, audit events, and completion definition.

Paperclip owns the canonical task ID and state. Other tools store that ID and a
link back to the canonical record.

## Integration rules

| System | Direction | Rule |
| --- | --- | --- |
| Notion / Claudio | Factory -> Notion | Publish portfolio, roster, decisions, and daily summaries |
| Linear | Two-way through adapter | Engineering issues only; Paperclip remains canonical for agent state |
| ClickUp | Factory -> ClickUp initially | Client/operations milestones and human assignments |
| Asana | Asana -> intake or read-only | Import legacy work; do not create a second live queue |
| Email | Email -> triage -> factory | No automatic external reply without policy and approval |
| Slack | Two-way notifications | Map named people, channels, tenant, and task ID before enabling actions |
| Buzz | Two-way task conversation | Exact group membership and authenticated protocol required |
| GitHub | Two-way code workflow | Branch and pull request only; protected branch and human review |
| Google Drive | Allowlisted read/write | Client folder boundary, classification, provenance, and sharing controls |

Lindi's email identity and Viktor's Slack identity must be explicitly mapped to
authorised roles. A display name, email forward, or channel mention is not an
identity or permission grant.

## Spec-driven build packet

Each build requires these linked artifacts:

1. problem statement and product requirement;
2. scope, non-goals, constraints, and threat/data classification;
3. architecture decision and affected systems;
4. acceptance criteria and test plan;
5. sequenced tasks with human and agent owners;
6. branch and pull request with review evidence;
7. automated test, security, and deployment evidence;
8. release record, rollback plan, operating owner, and client acceptance.

Agents may draft and update the packet. Humans approve scope, spending,
production access, external communication, release, and consequential actions.

## Daily agent update contract

Every active worker posts one structured heartbeat to Paperclip and the relevant
human channel:

- completed since the prior heartbeat;
- current task and evidence link;
- next action and expected completion;
- blockers, questions, and requested approval;
- cost/model usage and remaining budget;
- health, retry count, and last successful connector check.

The system produces a daily portfolio digest for Notion/Claudio. It must not
claim work is complete without an artifact or test record.

## Client factory template

Each client receives a private site, Paperclip company or tenant boundary,
repository, Drive folder, Buzz group or approved collaboration channel,
execution workspace, budget, connector identities, and audit retention policy.
The Eswatini deployment is the first governed template, not a shared bucket for
the next ten clients.

## Today's deployable slice

The safe first deployment is a staging proof, not a production client launch:

1. deploy one private Paperclip staging control plane;
2. create the Studex Software Factory company and Eswatini project;
3. import the current agent roster and P0/P1 work queue;
4. connect one Codex or Hermes worker with a strict budget and approval policy;
5. connect this GitHub repository and one test Drive folder with least privilege;
6. map the Eswatini Buzz group but keep sending disabled until authentication and
   group isolation pass an end-to-end test;
7. publish a read-only factory status view to the Claudio page;
8. run one synthetic specification-to-pull-request workflow and capture evidence.

Do not connect every productivity tool on day one. Add Linear after the canonical
task workflow passes, then ClickUp, Slack/email, and any required Asana migration.

## Deployment gates

- approved domain, cloud account, region, and cost ceiling;
- named administrators and client roles;
- secret manager and rotated credentials;
- backup, restore, logging, monitoring, and incident contact;
- tenant isolation and connector permission tests;
- Paperclip-to-worker heartbeat and approval test;
- GitHub protected branch and review test;
- Drive allowlist and sharing-denied test;
- Buzz authentication and exact-group test;
- CUA/Orgo computer isolation, shutdown, and cost-limit test;
- privacy/data-processing review and client acceptance.

## Acceptance scenario

A named user submits an approved synthetic request. Paperclip creates the
canonical task; a worker receives it on an isolated computer, reads only an
allowlisted document, produces a spec, opens a branch and pull request, runs the
tests, pauses for human approval, and publishes the approved result. Paperclip,
Buzz, Notion, GitHub, and Drive all show consistent references to the same task.
No other client data or channel is visible.

## Open decisions

- GCP project, billing account, region, domain, and deployment owner;
- Paperclip production authentication and tenancy configuration;
- whether persistent computers use Orgo, CUA-on-GCP, or both by workload class;
- Rome evaluation criteria and whether it adds value beyond Paperclip plus the
  existing dashboard;
- verified Buzz protocol, credentials, and agent membership;
- named Linear, ClickUp, Asana, Slack, and email owners and required scopes;
- source-of-truth confirmation for every existing active project.

## External references

- [Paperclip](https://github.com/paperclipai/paperclip)
- [Rome](https://github.com/rome-os/rome)
- [CUA](https://cua.ai/)
- [Orgo documentation](https://docs.orgo.ai/introduction)

