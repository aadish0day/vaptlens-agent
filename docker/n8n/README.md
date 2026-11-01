# n8n Workflow Automation Integration

VAPTLens Agent connects to a containerized **n8n** instance to manage multi-step approvals, ticket synchronization, escalations, and retest lifecycles.

## Workflows

### 1. Critical Finding Approval (`workflows/critical-approval.json`)
- Triggered by `POST /api/approvals/webhook` from the VAPTLens Agent API.
- Generates a signed, expiring URL token (`/api/approvals/decide?token=...`).
- Dispatches an interactive email to the designated reporting manager with AI triage rationale.
- Handles manager decision (`APPROVED` or `REJECTED`) and writes back to VAPTLens DB.

### 2. SLA Escalation (`workflows/escalation.json`)
- Polls or triggers after 24 hours of inactivity on pending critical approvals.
- Notifies secondary approver or team channel.

### 3. Retest Lifecycle (`workflows/retest-scheduler.json`)
- Fires reminder on remediation due date.
- Correlates re-scanned findings to automatically verify patch resolution.
