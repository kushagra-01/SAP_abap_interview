# 25 Workflow Basics

## Concept (2–3 lines)
- Workflow automates business processes (approvals, notifications) using tasks, agents, and events.
- Interview focus: what workflow solves, how it’s triggered, and how users interact (inbox).

## Interview-ready paragraph (say this)
I describe SAP workflow as a way to orchestrate business steps like approvals and notifications with clear responsibilities and audit trail. Workflows are typically triggered by events from business objects or application logic, and users work from the inbox to approve/reject tasks. The pitfall is incorrect agent determination and missing business context, so I ensure the workflow carries the right keys and I design robust error handling and monitoring so stuck items can be diagnosed and restarted.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is SAP workflow used for?
- Q: What is a work item?
- Q: What is the workflow inbox?

### Intermediate
- Q: How are workflows triggered (events)?
- Q: What is agent determination?
- Q: How do you monitor workflow issues?

### Advanced
- Q: How do you debug a stuck workflow?
- Q: How do you ensure correct authorization/agent coverage?
- Q: How do you handle retries/idempotency in workflow-triggered postings?

## Best Answers (1–3 lines)
- A: I use workflow to automate approvals/notifications with clear ownership and an audit trail.
- A: I trigger workflows from business events and pass the right business keys via binding, and I use agent determination to decide who gets the work item.
- A: I keep workflows resilient by ensuring correct agents, enough context to act, and monitoring so stuck items are visible.

## Common mistakes / traps
- Hard-coded agents (breaks with org changes).
- Missing business key/context in the work item (users can’t act).
- No monitoring strategy → stuck work items remain hidden.

## Optimization tips (DO / DON’T)
- **DO**: pass business keys; make tasks actionable with context.
- **DO**: design agent determination based on org/roles, not specific users.
- **DON’T**: trigger duplicate workflows on retries without an idempotency check.

## MEMORY HACK
- **1-line**: “Workflow = approvals via events → tasks → inbox; success = right agent + right context + monitoring.”
- **Keywords**: Event, Agent, Work item, Inbox, Approval

