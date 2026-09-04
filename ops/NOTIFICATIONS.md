# Notification Contract

## Required events

| Event | Delivery | Required fields |
| --- | --- | --- |
| `task.started` | activity feed | task, agent, time, expected outcome |
| `task.progress` | activity feed, grouped | task, agent, milestone, evidence |
| `task.blocked` | Buzz + dashboard | blocker, owner, requested decision |
| `approval.requested` | immediate Buzz alert | action, risk, impact, deadline |
| `task.failed` | immediate Buzz alert | error summary, attempts, recovery path |
| `task.completed` | Buzz + dashboard | outcome, artifacts, evidence, next action |
| `agent.unhealthy` | immediate health alert | agent, last heartbeat, current task |
| `decision.recorded` | dashboard + memory | decision, approver, rationale, date |

## Noise control

- Heartbeats update health state but notify humans only on a state change.
- Progress events are grouped into a digest unless a deadline or risk is affected.
- Repeated failures escalate after three attempts instead of looping silently.
- Completion messages link to the result and evidence; they do not merely say done.

## Initial channels

- `#eswatini` - country command and executive summaries
- `#opportunities` - qualified opportunity changes
- `#research` - sourced findings and confidence changes
- `#engineering` - runtime and integration work
- `#agent-health` - failures, recovery, and heartbeat state

The notification transport is configured at runtime. Tokens and webhooks must
never be committed.

