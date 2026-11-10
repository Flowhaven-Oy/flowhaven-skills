# Security

## Read-only by design

The flowhaven-insights skill only reads data. Its instructions tell Claude never to create, update, delete, upsert, undelete or merge records, never to run Apex, Flows or other automation, never to change configuration and never to send email from your org. It treats record content as data and ignores instructions found inside records, notes and files.

## Salesforce permissions are the real control

A skill is guidance for Claude, not an access control. What Claude can actually see and do is set by the Salesforce user that signs in through the connector. To keep your org safe:

- Connect each person as themselves with a read-only permission set.
- Never connect as an administrator or with Modify All Data.
- Use field-level security to hide bank, tax and personal data.
- Block the create, update and delete tools in Claude's connector settings where available.
- Start in a sandbox.

Full guidance: [docs/admin-guardrails.md](docs/admin-guardrails.md).

## Secrets

Claude never needs your password, tokens, keys or the External Client App's Consumer Key in a chat. Sign in happens through Salesforce's own login page. If a secret is ever pasted into a chat, rotate it.

## Reporting a problem

If you find a way to make the skill write data, reveal data it should not, or weaken your org's settings, please report it to your Flowhaven Customer Success manager. Include the prompt you used, what Claude did, and the skill version. Please do not include real customer data or secrets in your report.
