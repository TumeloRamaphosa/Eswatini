# Africa operating plan and Google deployment sequence

Updated: 18 September 2026
Owner: Codex for this planning revision. Delivery owners require assignment.
Status: planning baseline, not a deployment manifest or production-readiness claim.

## 1. Outcome and repository role

Use the Eswatini programme as a reference implementation for country and client operations. Reuse the operating pattern in separate spaces when approved. Global coordination receives approved summaries, while each country and client retains its own records and access boundary.

This public repository holds reusable plans, application code that has been reviewed, synthetic examples and infrastructure templates. It is not the client data room. Related product repositories remain separately owned and versioned.

The planning target is Google Cloud for compute and durable services, with Cloudflare as the proposed access/routing layer. Orgo remains an optional execution environment. This clarifies the earlier bootstrap document's Orgo-ready requirement; it does not claim that hosting or integrations have been completed.

## 2. What belongs on GitHub

| Safe to push after review | Keep in a private system |
|---|---|
| Architecture, role definitions and public-safe slide plans | Internal asset audit, cloud account inventory and private network details |
| Generic VM and container templates | Service-account JSON files, API keys, OAuth refresh tokens, relay signing keys |
| Sanitised environment examples with empty values | Real `.env` files, Terraform state and private variable files |
| Schemas, migrations, tests and synthetic fixtures | Client contacts, messages, documents, orders and financial records |
| Image build recipes and dependency locks | Private container images or dependencies without redistribution rights |
| Setup runbooks and acceptance criteria | Pricing negotiations, contracts, investor terms and unverified partner claims |
| Sanitised decision and build-status summaries | Raw logs, customer screenshots, backups and credential exports |

Treat public Git history as permanent disclosure. Ignore rules do not remove previously committed material. Review the staged diff and scan new files before pushing. Never copy the entire local home directory or the private planning inventory into this repository.

## 3. One operating model

| Component | Responsibility | Required connection |
|---|---|---|
| Nexus leadership view | Work status, approvals, evidence and service health | Read canonical task and agent records |
| Agent OS | Durable assignments and execution state | Dispatch work and record accepted results |
| Country/client space | Local ownership and access boundaries | Scoped identity, storage and task queue |
| Business Ghost | Decisions and institutional context with sources | Read/write only the assigned space |
| Worker VM | Run supervised agent processes and tool adapters | Authenticated task transport and model access |
| Engineering workers | Build and test isolated changes | Repository branch, test output and reviewed artifact |
| Buzz | Communication and activity notification | Map existing agent IDs to space/channel IDs |

Choose a canonical task store and registry before connecting additional dashboards. Existing UI, runtime and marketplace code require integration checks. Neither a registered agent nor a running VM proves a completed workflow.

## 4. Google Cloud settings checklist

Do not change the CLI's default project as a side effect. Use explicit `--project` and `--zone` arguments during implementation.

| Console area | Proposed setting | Evidence before deployment |
|---|---|---|
| Resource Manager | Separate development and production. Prefer a project per production client where stronger isolation is needed. | Project owner, billing owner, client ID and environment recorded privately |
| Billing | Attach authorised billing account and set a monthly pilot budget with recipients | Current estimate including compute, storage, NAT, logging, model calls and backups |
| Budgets & alerts | Use alerts and operational quotas. Evaluate spend-cap eligibility separately if needed. | Notification delivery tested; distinguish alerts-only budgets from enforceable controls |
| Region and zone | Johannesburg is the starting candidate for southern African workloads | Confirm client requirements, service availability, quota and latency before fixing location |
| APIs & Services | Compute Engine, Artifact Registry, Secret Manager, Logging, Monitoring, IAM Credentials and IAP for the VM baseline | Verify enabled APIs in the exact target project |
| Optional service APIs | Cloud Run, Pub/Sub, Firestore, Cloud Storage and Vertex AI only as selected by the architecture | API enabled does not establish data-store creation or model access |
| IAM | Separate deployer identity from runtime identity; dedicated runtime service account per client | Resource-level permissions and negative cross-client access test |
| Networking | Custom VPC/subnet, deliberate egress, no external VM IP by default | NAT/proxy path for public model endpoints and relay; Private Google Access as appropriate |
| Administration | OS Login and IAP-based administration | Authorised operator can connect without opening SSH to the internet |
| VM configuration | Supported Linux image, Shielded VM settings, persistent disk, deletion protection | Image compatibility, disk sizing, patch process and restore procedure |
| Secrets | Grant the runtime access only to its required secrets | Rotation owner and access audit; no secret value in metadata or startup script |
| Backups | Snapshot/data export policies with retention and restore test | Recovery objective and successful restore into an isolated environment |
| Monitoring | Heartbeat expiry, failed jobs, disk space and cost alerts | Alerts identify owner, task, latest success and recovery instructions |
| GitHub deployment identity | Workload Identity Federation restricted to this repository and approved branch/environment | Untrusted pull requests cannot obtain production credentials |

An alerts-only billing budget does not automatically cap spending. Google's separate spend-cap feature is in Preview and covers eligible services; it does not stop ongoing compute or storage charges and enforcement is not instantaneous. Do not treat it as a hard VM spending limit. Check eligibility and effects for the actual account before relying on it. [Budget alerts](https://docs.cloud.google.com/billing/docs/how-to/budgets), [spend caps](https://docs.cloud.google.com/billing/docs/how-to/budgets-spend-caps).

Use attached service accounts on VMs and federation for GitHub deployments instead of copying a long-lived JSON key into the repository. [Workload Identity Federation](https://docs.cloud.google.com/iam/docs/workload-identity-federation), [deployment pipeline setup](https://docs.cloud.google.com/iam/docs/workload-identity-federation-with-deployment-pipelines).

## 5. Google Workspace is a separate connection

Google Cloud authentication does not automatically grant Gmail, Drive or Calendar access.

1. Identify the exact Workspace domain, sender mailbox, Drive folder and Calendar needed by the pilot.
2. Verify the relevant Workspace subscription and user access with its administrator.
3. Configure the selected OAuth application and least-privilege scopes for those actions.
4. Use user OAuth for user-owned actions, or administrator-approved domain-wide delegation only where a server-side workflow requires it. Do not grant broad domain administration merely to send mail.
5. Share only the required Drive resources with the authorised identity. A local Drive mount is not evidence that a VM can access those resources.
6. Test a permitted read first. Test writes against a designated test folder/calendar/mail recipient before operational use.
7. Record successful adapter evidence without storing tokens in this repository.

Keep a selected mail provider as an adapter. Choosing AgentMail for an outreach campaign does not complete a Workspace integration or authorise every agent to send messages.

## 6. Client VM model

Start with one dedicated VM for one pilot client. It can run several lightweight agent containers. Do not default to five VMs per customer, and do not equate one agent with one VM.

| Workload | Initial sizing hypothesis | Scaling trigger |
|---|---|---|
| API-driven agent worker | 2 vCPU / 4–8 GB RAM class, modest persistent disk | Measured queue delay, CPU/RAM pressure or isolation requirement |
| Browser automation worker | Separate worker or more RAM after measurement | Concurrent browser sessions and memory pressure |
| Local model inference | Separate benchmark and hardware selection | Demonstrated need versus managed-model cost and latency |
| Shared coordination API | Reuse an existing suitable hub after audit | Availability and throughput requirements |

These are planning hypotheses, not quotations or final machine types. A shared-core machine can be suitable for a small coordinator but is not evidence of capacity for heavy browsers or local models.

Production client boundary: own project where feasible, own runtime service account, own secrets, own storage, and its own task authorisation. A namespace label alone is not a security boundary. If projects are shared during a pilot, document exactly how server-side access enforcement and negative isolation tests protect each client. Firestore collection naming alone does not isolate server SDK credentials.

## 7. Provisioning sequence

### Stage A. Inspect and select

Inventory existing VMs, applications, service accounts, networks, disks, databases, model endpoints and costs. Identify which machine is a suitable hub without changing or stopping existing services. Capture the image and deployed revision. Resolve ownership and current workloads before reuse.

### Stage B. Produce the implementation package

Create a reviewed infrastructure plan for the chosen client and environment. Recommended future layout:

```text
deploy/google/
  README.md
  modules/client-vm/        # network, identity, VM, storage and monitoring
  environments/example/   # synthetic variables, no client data
  containers/             # pinned image references and process configuration
  checks/                 # isolation, recovery and workflow verification
```

These files are not implemented by this documentation change. Keep remote state and actual environment variables in private systems. Review an infrastructure plan and cost estimate before applying it.

### Stage C. Establish prerequisites

Choose project, billing owner, zone, machine type, disk, service account and image. Create the selected network and outbound path. Grant IAP/OS Login access to authorised operators. IAP TCP forwarding requires the documented ingress configuration for the IAP source range, restricted to the target VM and administration port. Add runtime permissions only for the resources the worker needs.

A VM without an external IP still needs an outbound path to public APIs and the relay, such as Cloud NAT or an approved proxy. Private Google Access does not provide general internet access. [Compute IP addressing](https://docs.cloud.google.com/compute/docs/ip-addresses), [IAP TCP forwarding](https://docs.cloud.google.com/iap/docs/using-tcp-forwarding).

### Stage D. Create the VM and supervised runtime

After the reviewed resource plan and spend decision, create the VM from a trusted image. Configure OS Login, attached runtime identity, disk protection and tested Shielded VM settings. Install the pinned worker release using a repeatable image/bootstrap process. Use systemd or a container supervisor for restart, logs and shutdown handling. Fetch secrets at runtime through the attached identity. Start in test mode with no live outbound business actions.

For the implementation command and complete supported options, use Google's [instance creation guide](https://docs.cloud.google.com/compute/docs/instances/create-start-instance) and [gcloud reference](https://docs.cloud.google.com/sdk/gcloud/reference/compute/instances/create). Final commands must name the actual approved resources, not guessed values.

### Stage E. Connect the client

1. Provision a space ID and existing-or-new agent IDs after checking for duplicates.
2. Configure a hub endpoint and independently authenticated runtime identity.
3. Route tasks into the client's authorised queue or dispatcher scope. Check scope server-side on every assignment and result.
4. Load only client-authorised context from the chosen memory/document store.
5. Execute a bounded test task with a request ID and idempotency key.
6. Save the result and event to durable storage before acknowledging completion.
7. Update Nexus and publish only the approved summary through the configured notification transport.
8. Verify heartbeat expiry, retry behavior, failed-task handling and an operator stop procedure.

Use Cloud Run for suitable stateless APIs/jobs. Persistent relay listeners need explicit reconnect and supervision behavior. Cloud Run supports WebSockets, but connections remain subject to request timeouts. [Google Cloud documentation](https://docs.cloud.google.com/run/docs/triggering/websockets).

## 8. End-to-end acceptance and rollback

The first demonstration must show: authorised task, correct client worker, scoped context, completed output, durable evidence, dashboard update, notification, and healthy heartbeat. Repeat after restart. Retry the same request and confirm no duplicate business action. Attempt a cross-client read and verify rejection.

For rollback, stop task intake, let safe work finish or record interruption, preserve queue and evidence, revert the worker image to the recorded release, and rerun health checks. Restore data into an isolated environment before any replacement. Keep a named operator responsible for the decision. No automatic payment, trading, or public communication is part of the infrastructure smoke test.

## 9. Delivery plan and ownership

| Order | Output | Owner role | Exit evidence |
|---|---|---|---|
| 1 | Publish this public-safe plan | Planning owner | Git commit and remote branch |
| 2 | Select hub, client, budget and region | Programme lead + infrastructure owner | Private decision record and cost estimate |
| 3 | Review Terraform/container implementation | Infrastructure owner + reviewer | Plan output, pinned release and access design |
| 4 | Provision one client test VM | Infrastructure owner | Resource inventory and administration test |
| 5 | Connect one bounded worker workflow | Runtime engineer | Trace with stored result and notification |
| 6 | Verify isolation and recovery | Reviewer + operator | Restart/retry/negative-access evidence |
| 7 | Approve client pilot | Programme lead | Signed-off acceptance record |
| 8 | Repeat for the next client or country | Delivery lead | Capacity, support and unit-cost evidence |

The immediate configuration decisions are: client/space, accountable owner, target project, data location, monthly ceiling, compute profile, domain, and permitted tools. Named people, values and private identifiers belong in a private deployment record.

## 10. Leadership slide outline

Public-safe adaptation of the internal planning deck. Do not add confidential inventory, private terms or customer records when rendering slides.

| Slide | Topic | Leadership decision or evidence |
|---|---|---|
| 1 | Africa operating plan | Scope and review horizon |
| 2 | Portfolio | Business services, country programmes and shared software |
| 3 | Readiness | Code, plans, observed services and unverified claims |
| 4 | Shared platform | Registry and task-record authority |
| 5 | Country sequence | Country lead and qualified pilot before expansion |
| 6 | First customer workflow | Enquiry through accepted delivery |
| 7 | Agent accountability | Stable identity, runtime, role and human owner |
| 8 | Memory and documents | Space boundaries and authoritative records |
| 9 | Google settings | Identity, networking, billing and location |
| 10 | Client virtual machines | One pilot VM with measured sizing |
| 11 | Integration sequence | Dispatch, result, evidence and notification |
| 12 | Infrastructure investment | Customer demand and feasibility before capital claims |
| 13 | First sprint | Owners, dependencies and first demonstration |
| 14 | 90-day expansion | Repeatable delivery gates |
| 15 | Scorecard | Accepted work, reliability, unit cost and commercial evidence |
| 16 | Leadership decisions | Owners, budget, pilot and next review |

## Status of this revision

This revision publishes planning documentation only. It does not create client VMs, change IAM, enable APIs, send client messages, deploy dashboards, or connect live agent workflows. Current cloud inventory was checked read-only and is retained privately. Local unpublished country-space work remains separate and requires its own content review before inclusion.
