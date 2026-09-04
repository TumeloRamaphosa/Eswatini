# Agent Working Agreement

## Scope

Work only on the Studex Eswatini programme. Do not import or commit the Bothlale
website, the separate Mission Control deployment, Yarra, Arcade, or unrelated
client data.

## Start every task

1. Read `README.md`, `docs/PROJECT_SCOPE.md`, and `ops/BUILD_STATUS.md`.
2. Check for existing work before creating a duplicate implementation or agent.
3. Record the task owner and status in `ops/BUILD_STATUS.md`.
4. Keep secrets in environment variables or an approved secret store.

## Delivery rules

- Use a branch or isolated worktree for substantive changes.
- Keep domain decisions separate from provider-specific integration code.
- Add evidence for user-visible or operational behavior.
- Update persistent memory after a decision, completed workflow, or corrected fact.
- Update `ops/BUILD_STATUS.md` before ending a work session.
- Never claim an integration is live without an end-to-end test.

## Human approval gates

Agents must pause for approval before external communications, spending money,
changing access, deploying publicly, signing agreements, handling regulated
activity, or deleting data.

## Reporting

Publish events according to `ops/NOTIFICATIONS.md`. Failures, blocked decisions,
and approval requests are immediate. Routine progress is summarized without
flooding the user.

## Definition of done

Work is done only when the result is saved, visible in the dashboard, written to
the activity log, retained in project memory, and reported to Buzz with evidence.

