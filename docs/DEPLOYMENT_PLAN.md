# Client Operating System Deployment Plan

Status: Proposed architecture
Last updated: 2026-11-06

## Decision

The proposed model is viable:

`client site -> Cloudflare Access -> Cloudflare Tunnel -> Orgo VM ->`
`StudBot API + Hermes -> Buzz group -> people and specialist agents`

Each client also receives a private Google Drive folder and a private GitHub
repository. These systems have different jobs and must not be treated as one
shared storage bucket.

Paperclip is the proposed cross-client factory control plane. CUA and Orgo are
execution computers; Rome is an optional workspace/application layer under
evaluation. Their responsibilities and the integrations with Notion, Linear,
ClickUp, Asana, Slack, and email are defined in
`docs/SOFTWARE_FACTORY_PLAN.md`.

## What each component does

| Component | Responsibility | Must not contain |
| --- | --- | --- |
| Client site | Dashboard, tasks, approvals, agent activity, document links, health | Raw secrets, unrestricted admin controls |
| Cloudflare | DNS, TLS, WAF, Access login, Tunnel to the VM | Client business records as the system of record |
| Orgo workspace + VM | Persistent Hermes runtime, StudBot API, workers, local cache, connectors | Credentials baked into images or repos |
| Buzz group | Human/agent conversation, task intake, progress, questions, approvals | Long-term document archive or unrestricted commands |
| Google Drive | Approved client documents, deliverables, meeting packs | Runtime database, queues, access tokens |
| GitHub repository | Code, configuration templates, schemas, runbooks, approved Markdown | `.env`, tokens, private customer uploads, raw personal data |
| Runtime database | Tasks, memberships, permissions, events, approvals, connector state | Unencrypted secrets or unrelated clients' records |

## Client isolation model

Start with one Orgo workspace and one persistent Linux computer per client.
This costs more than putting all clients into one host, but provides a clean
security, backup, billing, support, and handover boundary.

For each client create:

- Orgo workspace: `studex-<client-slug>`;
- Orgo computer: `<client-slug>-agent-01`;
- Cloudflare application: `<client-slug>.clients.studex...`;
- Buzz group: one private group with approved people and agents;
- Google Drive folder: `Clients/<Client Name>`;
- GitHub repository: private `studex-client-<client-slug>`;
- database tenant and encryption boundary;
- model budget, connector permissions, and audit retention policy.

Do not reuse the Eswatini Buzz group, Drive folder, repository, or memory
namespace for another client.

## Agent communication

Hermes can communicate with people and other agents in Buzz only after the
following are verified:

1. Buzz provides an authenticated API or WebSocket protocol for the agent.
2. The agent has a stable identity and membership in the exact client group.
3. The message schema supports task IDs, sender identity, replies, attachments,
   delivery acknowledgement, and error reporting.
4. Permissions prevent the agent from reading or posting to other client groups.
5. Every consequential action is tied to an auditable task and approval state.

The Buzz URL and group UUID are routing information, not authentication.
Until an end-to-end test passes, describe the integration as proposed, not live.

### Conversation contract

Buzz messages should map to these event types:

- `task.created`
- `task.accepted`
- `task.progress`
- `question.raised`
- `approval.requested`
- `approval.granted` or `approval.denied`
- `task.completed`
- `task.failed`
- `agent.unhealthy`

Every event carries `tenant_id`, `group_id`, `task_id`, `agent_id`, timestamp,
classification, and a link to the saved result or evidence.

## Human authority

Agents may autonomously research approved sources, draft internal work, update
task status, save approved files, run tests, and report progress.

Agents must wait for an authorised human before:

- sending external messages or publishing content;
- signing or accepting agreements;
- spending money or initiating financial activity;
- changing permissions, credentials, or production infrastructure;
- deleting material records;
- making government, partner, legal, safety, lending, mining, surveillance, or
  regulatory representations.

An approval is valid only when it names the task, proposed action, approver,
scope, time, and outcome. A casual Buzz reply is not blanket authority.

## Site architecture

### Client-facing surface

- overview and current outcomes;
- tasks, owners, due dates, and status;
- conversation panel linked to the client's Buzz group;
- approval queue;
- agent roster, current work, heartbeat, and cost usage;
- documents and deliverables linked from Drive;
- audit activity and support status.

### Access

- Cloudflare Access authenticates client users through an approved identity
  provider or one-time PIN policy;
- role mapping happens in the application, not only at the Cloudflare layer;
- administrators use Tailscale or a separately protected administration route;
- the Orgo VM has no open public application port;
- `cloudflared` makes outbound Tunnel connections from the VM;
- WebSockets are permitted only on the required application route.

### Runtime

- web/API service;
- Hermes agent service;
- Buzz adapter;
- Drive adapter;
- GitHub adapter;
- task queue and scheduler;
- relational database;
- encrypted secret provider;
- audit/event writer;
- health and cost monitor.

Google Drive and GitHub connectors run with least-privilege identities. The
agent never receives a user's broad personal Drive or GitHub access when a
client-scoped service identity is sufficient.

## Repository model

Use two repository layers:

1. `studex-client-os-template` contains the reusable application, deployment
   modules, tests, and schemas.
2. Each private client repository contains tenant configuration, approved
   workflow definitions, non-sensitive knowledge indexes, runbooks, and release
   manifests.

Client repos consume a tagged template release. Do not fork application code
independently for every client unless a contractual requirement demands it.

## Google Drive model

Each client folder should contain:

- `00 Intake`
- `01 Approved Knowledge`
- `02 Working Drafts`
- `03 Deliverables`
- `04 Meetings and Decisions`
- `05 Legal and Data Processing`
- `99 Archive`

The Drive adapter uses an allowlist of folder IDs. Files enter the Business
Ghost only after malware/type checks, classification, provenance capture, and
approval. Deletion or sharing changes require a human.

## Secrets

Store Orgo, Buzz, Drive, GitHub, model-provider, Cloudflare, and database
credentials in a secret manager or protected Orgo runtime configuration.

- never paste secrets into Buzz;
- never save them in Drive or Git;
- never expose them in screenshots or agent logs;
- issue one credential per client and connector;
- use the minimum scopes and rotate on staff or client changes;
- redact connector responses before placing them in model context.

## Deployment sequence

### Phase 0: prove the interfaces

1. Obtain Buzz protocol documentation and a non-production agent credential.
2. Confirm the Eswatini group membership and permission model.
3. Create a test Drive folder and private test repository.
4. Prove read, create, update, and permission-denied behavior for each connector.
5. Record model choice, token limits, and daily cost ceiling.

### Phase 1: build the golden client template

1. Build the dashboard and API locally.
2. Implement tenant, role, event, task, approval, document, and agent schemas.
3. Package Hermes and the adapters as supervised services.
4. Add tests, health checks, structured logs, and backup/restore.
5. Use synthetic data for the first complete workflow.

### Phase 2: deploy one Eswatini staging tenant

1. Create one Orgo workspace and Linux computer.
2. Deploy the tagged golden template.
3. Install and configure `cloudflared` with a staging Tunnel.
4. Add Cloudflare Access for named test users.
5. Create the private Eswatini Drive folder and GitHub repository access.
6. Connect Hermes to the exact Eswatini Buzz group.
7. Run the acceptance suite and security review.

### Phase 3: client pilot

1. Load only approved Eswatini documents.
2. Invite a small authorised client group.
3. Run one real, low-risk workflow with human approval gates.
4. Monitor accuracy, corrections, latency, model cost, connector errors, and
   agent availability for at least five working days.
5. Obtain written acceptance or document the remediation plan.

### Phase 4: repeat for ten clients

Use infrastructure as code and an onboarding manifest to create each client
boundary. Provision clients serially until the template has completed three
stable pilots; then increase concurrency.

## Acceptance test

The deployment passes only when all steps are evidenced:

1. Named client user authenticates through Cloudflare Access.
2. The user creates a task from the site or Buzz group.
3. The correct Hermes agent acknowledges it in the correct group.
4. A specialist agent collaborates without crossing tenant boundaries.
5. The agent reads only an allowlisted Drive document.
6. The result is saved to the correct Drive folder and linked to the task.
7. A permitted repository change is made through a branch and review workflow.
8. A consequential test action stops for approval.
9. Completion and evidence appear in Buzz and on the dashboard.
10. Restart, expired-token, network-loss, and backup-restore tests pass.

## Failure boundaries

- If Buzz is unavailable, tasks remain queued and the site shows degraded state.
- If Drive is unavailable, agents do not invent or silently substitute documents.
- If GitHub is unavailable, code changes remain on a branch and are not reported
  as shipped.
- If the model budget is reached, new inference stops and a human is notified.
- If an agent loops or fails three times, the task is blocked and escalated.
- If tenant identity cannot be proven, the request is rejected.

## Production gates

Do not invite a client until these are complete:

- data-processing and privacy review;
- identity and role matrix;
- signed scope and support terms;
- cost and usage caps;
- backup and recovery test;
- connector permission test;
- Buzz end-to-end test;
- security review and incident contact;
- client admin training;
- export and offboarding procedure.

## External references

- [Cloudflare Tunnel fundamentals](https://developers.cloudflare.com/tunnel/concepts/)
- [Cloudflare secure application delivery](https://developers.cloudflare.com/reference-architecture/design-guides/secure-application-delivery/)
- [Cloudflare Tunnel protocol support](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/routing-to-tunnel/protocols/)
- [Orgo introduction](https://docs.orgo.ai/introduction)
- [Orgo API reference](https://docs.orgo.ai/api-reference/introduction)

