# Connect Claude to your Flowhaven org

Connect your Flowhaven Salesforce sandbox or production org to Claude, and Claude can read your licensing data and answer questions about it: agreements, minimum guarantees, granted rights, submissions and royalty reports. The flowhaven-insights skill is read-only. It does not create or change records.

Setup takes about 15 minutes of clicking, plus **up to 30 minutes of waiting** while Salesforce activates your new app. Do Steps 1 to 3, take a break, then finish with Steps 4 and 5.

## Before you start

| You need | Why |
|---|---|
| A Salesforce admin for your Flowhaven org | to turn on the MCP server and create the External Client App |
| A Claude plan with custom connectors (Pro, Max, Team or Enterprise) | to add the connector. Team or Enterprise is recommended so admins control access and retention |
| A Claude **Owner**, on Team and Enterprise plans | Owners add the connector once for everyone; members then just click Connect |
| Your Flowhaven org's **My Domain**, and whether it's a **sandbox or production** | both go into the connector URL in Step 3 |
| A read-only Salesforce user, or a read-only permission set, for each person, with **API Enabled** | Claude only sees what that user can see |

**Connect each person as themselves with a read-only permission set.** Do not share one login between people, and do not connect as an administrator. See [admin guardrails](admin-guardrails.md) for the recommended setup.

Start with your **Flowhaven sandbox**. Move to production once your team trusts the answers.

## Step 1: Create the External Client App in your Flowhaven org

This app is what lets Claude sign in to your Flowhaven org. Create it **in the org you want to connect**. An app in your sandbox only works for that sandbox, and an app in production only works for production.

1. In your Flowhaven org, go to **Setup**, type "External Client App Manager" in Quick Find and click **New External Client App**.
2. Basic information: App Name `Claude MCP Client`, plus your contact email.
3. Expand **API (Enable OAuth Settings)** and tick **Enable OAuth**.
4. Callback URL: `https://claude.ai/api/mcp/auth_callback`
5. OAuth scopes, select exactly these two:
   - **Access Salesforce hosted MCP servers** (`mcp_api`)
   - **Perform requests at any time** (`refresh_token, offline_access`)
6. Security: untick **Require secret for Web Server Flow** and **Require secret for Refresh Token Flow**.
7. Security: tick **Require Proof Key for Code Exchange (PKCE)** and **Issue JSON Web Token (JWT)-based access tokens for named users**.
8. Click **Create**. Then open **Settings, OAuth Settings, Consumer Key and Secret**, verify your identity, and copy the **Consumer Key**.

Two settings cause most failed connections:

- **Scopes.** Don't add **Full access** (`full`) or anything else. Claude asks for `mcp_api`, and a missing or extra scope gives an `invalid_scope` error. The app's **Policies** tab has its own list of allowed scopes, so check it there too.
- **JWT tokens (item 7).** Without them, sign in looks fine but every request Claude makes fails.

The Consumer Key identifies the app, so treat it like a password. Paste it only into Claude's connector settings (Step 4), never into a chat, email or ticket. Claude will never ask you for it in a chat.

## Step 2: Turn on the MCP server in your Flowhaven org

1. In your Flowhaven org, go to **Setup**, type "MCP Servers" in Quick Find and open the **Salesforce Servers** tab.
2. Open **`sobject-all`** and click **Activate**. Claude uses it to read records.
3. Leave **`headless-360`** switched off. This version of the skill does not use it.

Skip the **Server URL** Salesforce shows on that page. It's the same for every org, so Claude can only hold one connector with it. Step 3 builds a URL that belongs to your Flowhaven org alone.

## Step 3: Build the server URL for your Flowhaven org

Each connector needs a URL built from your Flowhaven org's **My Domain**. You'll find it under **Setup, My Domain**, or in your browser's address bar: it's the part before `.my.salesforce.com`.

| Your Flowhaven org | My Domain looks like | Server URL |
|---|---|---|
| Production | `acme` | `https://api.salesforce.com/platform/mcp/v1/d/acme/platform/sobject-all` |
| Sandbox | `acme--uat` | `https://api.salesforce.com/platform/mcp/v1/d/acme--uat/sandbox/platform/sobject-all` |
| Developer Edition | `acme-dev-ed` on `.develop.my.salesforce.com` | `https://api.salesforce.com/platform/mcp/v1/d/acme-dev-ed/develop/platform/sobject-all` |

Replace `acme` with your own My Domain.

Because every org gets its own URL, you can connect your Flowhaven sandbox and production side by side, each as its own connector, and the sign in goes straight to the right org.
## Now wait, up to 30 minutes

A new External Client App is **not usable straight away**. Salesforce says it can take **up to 30 minutes** to become available, and often it takes about 10. The same delay applies after you edit the app, such as changing the callback URL or scopes.

If you connect too early, the Salesforce sign in page shows an error like **`invalid_client_id`**. That doesn't mean your settings are wrong. Don't recreate the app, because a new app restarts the clock. Wait, then click Connect again.

A good rhythm: finish Steps 1 to 3, note the time, and come back to Step 4 about 15 minutes later.

## Step 4: Add the Flowhaven connector in Claude

On Team and Enterprise plans an **Owner** does this once for the whole organization. On Pro and Max you do it yourself. Add one connector for each Flowhaven org you want to use.

1. In Claude, open **Customize, Connectors** (Owners: **Organization settings, Connectors**), click **+** and choose **Add custom connector**.
2. Name it after the org, such as `Flowhaven Sandbox` or `Flowhaven Production`. With several orgs connected, the name is how you and Claude tell them apart.
3. Remote MCP server URL: the URL you built in Step 3.
4. **Advanced settings, OAuth Client ID**: paste the Consumer Key from Step 1. Leave the client secret empty, since PKCE takes its place.
5. Click **Add**.

## Step 5: Connect, sign in and test

1. In **Customize, Connectors**, click **Connect** next to your Flowhaven connector. Use a private browser window, so a session from another Salesforce org can't get in the way.
2. On the Salesforce login page, click **Use Custom Domain**, enter your Flowhaven org's My Domain, and sign in with **your own read-only user** in that org. Then click **Allow**.
3. Back in Claude, the connector shows as connected. Click **Configure** and block the tools that create, update or delete records. The skill never uses them, and blocking them adds a second lock.
4. Add the flowhaven-insights skill if you haven't yet. The repository README shows how: from the plugin marketplace, or by uploading the zip.
5. In a new chat, turn the connector on and ask:

> *Which Flowhaven org are you connected to? Tell me its name and whether it's a sandbox or production.*

Claude should answer with your org's name and type. Then try a read-only question, for example:

> *Which agreements expire in the next 90 days?*

> *Which licensees are behind on their minimum guarantee?*

> *Which submissions have been waiting for approval more than 10 days?*

## How to disconnect

- **One person:** in Claude, open **Customize, Connectors**, open the Flowhaven connector and click **Disconnect**.
- **Remove the connector for everyone:** a Claude Owner opens **Organization settings, Connectors** and removes it.
- **Cut access from the Salesforce side:** in Setup, revoke the app's access for the user, or disable or delete the `Claude MCP Client` External Client App. This ends every Claude session to that org.

See [admin guardrails](admin-guardrails.md) for checking access afterwards.

## Troubleshooting

| What you see | Likely cause | Fix |
|---|---|---|
| `invalid_client_id` on the Salesforce login page | the app is still activating (up to 30 min) | wait and click Connect again; don't recreate the app |
| `invalid_client_id` after waiting | you signed in to a different org than the one holding the app | private window, **Use Custom Domain**, your Flowhaven org's My Domain |
| "A connector with this URL already exists" | another connector already uses that URL | build the org-specific URL in Step 3 |
| `invalid_scope`: the requested scope is not allowed | `mcp_api` missing, or extra scopes such as `full` | exactly the two scopes from Step 1, on Settings and Policies, then wait |
| `OAUTH_APPROVAL_ERROR_GENERIC` | URL not in the Step 3 format, or an open session in another org | check the URL and retry in a private window |
| Sign in works but every request fails (`INVALID_SESSION_ID`) | JWT tokens off, or your user lacks API Enabled | fix it, then disconnect and reconnect so Claude gets a fresh session |
| Login page complains about the redirect | callback URL typo | must be exactly `https://claude.ai/api/mcp/auth_callback` |
| You can't add a custom connector | your Claude role isn't Owner | ask a Claude Owner to add it (Step 4) |
| Server not listed in Setup | hosted MCP isn't available for the org yet | contact your Salesforce account team |
| Claude says it can't read some records | your read-only permission set doesn't cover them | ask your Salesforce admin whether you should have access; Claude won't suggest changes |

## Sources

- Create an External Client App, Salesforce Hosted MCP Servers: https://developer.salesforce.com/docs/platform/hosted-mcp-servers/guide/create-external-client-app.html
- Configure Claude, Salesforce Hosted MCP Servers: https://developer.salesforce.com/docs/platform/hosted-mcp-servers/guide/claude.html
