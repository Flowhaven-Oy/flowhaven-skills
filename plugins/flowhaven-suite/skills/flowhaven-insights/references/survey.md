# Survey: run this first

Every step here is a read: a query or a describe through the connector's read tools. Run it once at the start of each conversation, before answering anything. Report the results to the user in two or three lines.

## 1. Which org is this?

```sql
SELECT Name, IsSandbox, OrganizationType FROM Organization
```

- A successful answer proves the connection works.
- Tell the user: "You are connected to **<Name>**, a **<sandbox | production org>**."
- If `IsSandbox` is false, say clearly that this is production and that you will only read.
- If more than one Salesforce connector is on, ask which one to use first.

## 2. Is Flowhaven installed?

The package check is done by describe, not by guessing names.

1. List installed packages:

   ```sql
   SELECT NamespacePrefix FROM PackageLicense
   ```

2. List the org's objects with the connector's describe or object listing tool. Look for the core Flowhaven licensing objects by **label**: Agreement, Minimum Guarantee, Granted Right, Royalty Report and Royalty Report Line. Labels may differ slightly between orgs, so match loosely (for example "Installment" or "Instalment").

3. Decide:

   | What you find | Meaning |
   |---|---|
   | The licensing objects exist and their API names start with one of the installed namespace prefixes followed by two underscores | Managed Flowhaven package. Use that prefix on every object and field name. |
   | The licensing objects exist with no prefix | Flowhaven deployed without a namespace. Use the names as they are. |
   | No licensing objects found | Not a Flowhaven org. Say: "This skill only works with a Flowhaven org, and I can't find Flowhaven in **<Name>**." Then stop using this skill. |

4. Remember the API names you found for each concept. Describe each object only when you need its fields, and **one object per describe call**: Flowhaven objects have many fields, and a describe of several at once can be too large to read.

## 3. Who is connected?

Find the connected user with the connector's user info tool, or ask the user for their username if no such tool exists. Then:

```sql
SELECT Name, Profile.Name, Profile.PermissionsModifyAllData, Profile.PermissionsViewAllData
FROM User WHERE Id = '<connected user id>'
```

```sql
SELECT PermissionSet.Label, PermissionSet.PermissionsModifyAllData, PermissionSet.PermissionsViewAllData
FROM PermissionSetAssignment
WHERE AssigneeId = '<connected user id>'
  AND PermissionSet.IsOwnedByProfile = false
  AND (PermissionSet.PermissionsModifyAllData = true OR PermissionSet.PermissionsViewAllData = true)
```

(`IsOwnedByProfile = false` leaves out the hidden permission set behind the profile, which the first query already covers.)

- If the profile name looks like an administrator profile, or any result has Modify All Data, say once: "You're connected as a user with administrator level access. This skill only reads, but we recommend connecting with a dedicated read-only user. See the admin guardrails in the skill's documentation."
- View All Data alone is common for reporting users. Mention it only if the user asks about access.
- Never suggest adding permissions, even if a query fails for lack of access. Say what you could not read instead.
- Do not show the user ID back unless asked.

## 4. Which record types are in use?

```sql
SELECT SobjectType, Name, DeveloperName FROM RecordType
WHERE IsActive = true AND SobjectType IN (<the licensing object API names you found>)
```

Record types often separate agreement kinds (for example a short deal memo versus a full agreement) or submission workflows. Use their names, not their IDs, when you filter.

## 5. One currency or several?

Describe the agreement or minimum guarantee object. If it has a currency field (`CurrencyIsoCode`), the org may use several currencies. If it has none, the org uses one currency and queries that name the field will fail.

If the field exists, find the corporate currency:

```sql
SELECT IsoCode, IsCorporate, ConversionRate FROM CurrencyType WHERE IsActive = true
```

In a multi-currency org, totals from a query (`SUM`, `AVG`, `MIN`, `MAX` on money fields) come back **converted to the corporate currency**, even when the query groups by currency. Individual records keep their own currency. So either:

- present query totals as corporate currency totals, saying they use the org's conversion rates, or
- for totals in each original currency, fetch the records and add them up yourself, per currency.

Never label a converted total with the original currency code. Group money by currency in every answer and never add amounts across currencies.

## What to tell the user

Keep it short, for example:

> Connected to **Acme Toys (Sandbox)**. Flowhaven is installed. You're signed in as a read-only user. Ask me anything about your agreements, guarantees, rights, submissions or royalty reports.
