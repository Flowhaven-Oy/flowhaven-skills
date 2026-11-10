# Pilot playbook

A good pilot is small, read-only, two to four weeks long, and has three named questions it must answer.

## Set up safely

- Start in a full or partial copy sandbox. Move to production only after the team trusts the answers.
- Connect with a dedicated read-only Salesforce user or permission set: read access to the Flowhaven objects in scope that matches each person's own user license level, no Create, Edit or Delete, API Enabled. This is the real lock; the skill is the second one. See [admin guardrails](admin-guardrails.md).
- Activate only `sobject-all`. Leave `headless-360` off for this version.
- Where Claude lets you set tool permissions on the connector, block the create, update and delete tools.
- Use Claude Team or Enterprise so admins control the connector, retention and who has access.
- Check your own AI and data policy, and any agreement clauses with your licensees, before connecting production data.

## Run it well

1. **Pick 3 to 5 pilot users across roles:** a licensing manager, someone from finance, someone from approvals, and one leader.
2. **Agree three success questions up front.** For example:
   - "Which licensees are behind on their MG?"
   - "What is still open in our Nordic territories?"
   - "Which submissions have been waiting more than 10 days?"
3. **Check the first answers against Flowhaven reports.** Ask Claude to show the query behind each number.
4. **Turn the best answers into reusable artifacts:** an MG status table, a weekly royalty report tracker, call prep for a licensee.
5. **Keep a shared log** of questions that failed or needed a write. That log is the brief for the next version.
6. **Hold a weekly 20 minute check-in** with your Flowhaven Customer Success manager during the pilot.

## Measure

| Measure | Target |
|---|---|
| Success questions answered correctly | 3 of 3 |
| Time to answer an ad hoc question | under 5 minutes |
| Weekly active pilot users | at least 3 |
| Write requests logged | tracked, to shape the next version |

## When you want more

If your team wants Claude to create or change records, for example creating agreements from PDFs, data entry, invoicing or bulk updates, contact your Flowhaven Customer Success manager to discuss your use case. Any use that writes data needs sandbox testing and written approval before it reaches production.
