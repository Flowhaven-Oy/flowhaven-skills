# Admin guardrails

The flowhaven-insights skill is read-only by design, but the skill is the second lock. The first and real lock is what the connected Salesforce user is allowed to do. Set that up carefully.

## 1. Use a least privilege, read-only permission set

Create a permission set for Claude users with:

- **Read** on the Flowhaven objects in scope (agreements, minimum guarantees and installments, granted rights and their children, submissions and SKUs, royalty reports and lines, accounts).
- **Access that matches the person's user license level.** Each person sees through Claude what their own license level and role already let them see in Flowhaven. Add View All only where that level already covers every record; otherwise leave it off and let your sharing rules decide.
- **No Create, Edit or Delete** on any object.
- **API Enabled.**
- No Modify All Data, no View Setup and Configuration, no Customize Application, no Author Apex.

Assign it alongside a minimal profile. Test it in a sandbox first by signing in as that user and confirming that edit buttons are missing.

## 2. No admin or shared logins

- Do not connect Claude as a System Administrator or any user with Modify All Data. The skill will warn if it detects one.
- Do not share one Salesforce login between people. Each person connects as themselves, so Salesforce sharing, field security and audit logs apply to the right person.

## 3. Hide sensitive fields with field-level security

Remove read access, in the Claude permission set, to fields that the people using Claude do not need, such as:

- bank and payment details on accounts or invoices
- tax identifiers
- personal contact details and home addresses
- any free text fields that may hold personal data

Claude cannot read a field the user cannot read.

## 4. Per-user sign in

Every person signs in to Salesforce through the connector with their own user. Sign in uses OAuth with PKCE, so no password or secret is stored in Claude. Treat the External Client App's Consumer Key like a password: only an admin should handle it, and only in Claude's connector settings.

## 5. Lock down the connector in Claude

- Where Claude lets you set tool permissions on the connector, block the create, update and delete tools.
- Activate only the `sobject-all` MCP server in Salesforce. Leave `headless-360` off for this version.
- On Team and Enterprise plans, only Owners add connectors. Decide who in your Claude organization may use the Flowhaven connector.

## 6. Know how to switch it off

- **Revoke the External Client App:** in Setup, revoke OAuth access for a user, or disable or delete the External Client App to cut off everyone at once.
- **Remove the connector:** a Claude Owner removes it under **Organization settings, Connectors**. Individuals can disconnect under **Customize, Connectors**.
- **Remove the permission set** from anyone who should no longer have access.

## 7. Check what happened

- **Setup Audit Trail:** confirm that no configuration changes were made through the Claude user.
- **Login History:** review sign ins by Claude users and the app used.
- Field history and record history: with a read-only user these should show no changes by that user. Any change is a sign the permission set is wrong.

## 8. Sandbox first

Start in a full or partial copy sandbox. Move to production only after your team has checked the answers against Flowhaven reports and trusts them.

## 9. Check your own policies before connecting production data

- **Your AI and data policy:** confirm that using Claude with business and licensee data is allowed, and for which data.
- **Claude plan settings:** use Claude Team or Enterprise so admins control connectors, data retention and who has access. Review your retention settings.
- **Licensee agreements:** some licensing agreements limit how licensee sales data may be shared or processed. Check the relevant clauses before connecting production data.
