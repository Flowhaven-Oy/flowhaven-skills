# Flowhaven skills for Claude

Claude skills for teams using the Flowhaven licensing suite, a licensing and merchandising platform built on Salesforce.

## flowhaven-insights

**Read-only.** flowhaven-insights helps you understand and query your Flowhaven org through a Salesforce connector in Claude. Ask about agreements, minimum guarantees, granted rights, submissions, design approvals and royalty reports in plain language, and turn the answers into summaries, tables and dashboards. Claude shows the query behind every number.

## Install

### 1. Connect your org

Follow [Connect Claude to your Flowhaven org](docs/connect-claude-to-flowhaven.md). Start with a sandbox and a read-only user.

### 2. Add the skill

This repository is a Claude plugin marketplace. Pick one way in:

**Claude app (web and desktop).** Open **Customize, Plugins**, choose **Add marketplace** and enter:

```
Flowhaven-Oy/flowhaven-skills
```

Then install the **flowhaven-suite** plugin. On Team and Enterprise plans an Owner can add it once for the whole organization.

**Claude Code.** Run:

```
/plugin marketplace add Flowhaven-Oy/flowhaven-skills
/plugin install flowhaven-suite@flowhaven
```

**Upload instead.** Download `dist/flowhaven-insights-v1.0.0.zip`, check it against `dist/SHA256SUMS`, then upload it under **Customize, Skills**.

### 3. Ask a first question

Start a new chat with the Flowhaven connector on and ask: *"Which Flowhaven org are you connected to?"*

### Updates

New versions are published to this repository. If you installed through the marketplace, update the plugin from Claude. If you uploaded the zip, download and upload the new one.

## Example questions

- Which licensees are behind on their minimum guarantee?
- What is Acme Toys allowed to sell in Germany, and through which channels?
- Which royalty reports for last quarter are still missing?
- Which submissions have been waiting for approval more than 10 days?
- Build a table of MG status for all active agreements, with the query behind each number.

More, grouped by role, in [question-patterns.md](plugins/flowhaven-suite/skills/flowhaven-insights/references/question-patterns.md).

## What v1 does not do

- It does not create, edit, delete or import records.
- It does not approve or reject submissions, validate or invoice royalty reports, or bill installments.
- It does not run Apex, Flows or other automation, change configuration or send email.
- It does not export contacts or personal data in bulk.
- It does not work with Salesforce orgs that don't have Flowhaven installed.

If you ask for a change, Claude offers to draft what the record would contain so you can enter it yourself.

## Safety

The skill is read-only by design, but the real control is the Salesforce user you connect with. Read [admin guardrails](docs/admin-guardrails.md) before connecting, and see [SECURITY.md](SECURITY.md) for how to report a problem. Running a pilot? See the [pilot playbook](docs/pilot-playbook.md).

## Repository layout

```
.claude-plugin/marketplace.json              the marketplace listing
plugins/flowhaven-suite/                     the plugin
  skills/flowhaven-insights/                 the skill source
docs/                                        setup, guardrails and pilot guides
dist/                                        the skill as a zip, with its checksum
```
