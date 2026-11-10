# Licensing concepts in Flowhaven

This page explains what the main records mean to a user and how they connect. Find the actual object and field names by describe at run time (see `survey.md`).

## The big picture

```
Licensor and licensee (accounts)
  └─ Agreement
       ├─ Minimum guarantee
       │    └─ Installments
       ├─ Granted rights
       │    ├─ Brands
       │    ├─ Product categories
       │    ├─ Territories
       │    ├─ Distribution channels
       │    └─ Royalty rates
       ├─ Submissions (design approvals)
       │    └─ SKUs (products)
       └─ Royalty reports
            └─ Report lines
```

## Accounts: licensor and licensee

The **licensor** owns the brands. The **licensee** pays to make and sell products using them. Both are accounts. Example: Northwind Licensing (licensor) grants Acme Toys (licensee) the right to make plush toys.

## Agreement

The contract between a licensor and a licensee. It sets the term (start and end dates), the reporting rhythm (monthly, quarterly and so on), and often a sell-off period after the end date in which remaining stock may still be sold.

Agreements move through a lifecycle. Typical states a user sees:

- **Draft or deal memo:** terms agreed in principle, not yet fully signed. Money on these agreements is pipeline, not yet billed.
- **Executed or active:** signed and in force.
- **Expired or terminated:** past the term, possibly still in sell-off.

The exact stages and statuses are set per org. Read them from the describe and use the labels the user sees.

## Minimum guarantee (MG)

A promise by the licensee to pay at least a set amount over the agreement, whatever their sales. It is paid in **installments** on a schedule, often starting with an advance.

What the MG numbers mean to a user:

| Term | Meaning |
|---|---|
| Commitment | The total the licensee guaranteed, made up of the scheduled installments. |
| Invoiced | How much of the guarantee has been billed so far. Future installments are not invoiced yet. |
| Recouped | How much of the billed guarantee has been earned back by royalties the licensee reported. |
| Open balance | Billed guarantee not yet earned back by royalties. |
| Unrecouped | The same gap seen from the licensee's side: guarantee paid that sales have not yet covered. |
| Overage | Royalties earned beyond the guarantee. These are billed on top. |

A guarantee usually sits on the agreement, but an org can also set guarantees at the level of a granted right, a rate or a SKU. Check the minimum guarantee record types and say which levels your answer covers.

A licensee is "behind" when a large share of the invoiced guarantee is still open late in the term. A licensee is "fully recouped" when the open balance is zero, and further royalties become overage.

Balances are maintained by Flowhaven as invoicing and royalty processing happen. Report them as the org shows them. Never recalculate a balance in your own way and present it as the official figure. If the org shows more than one balance field that looks similar, ask the user which one their team reports from, or state which one you used.

Some installments, such as flat fees, may sit outside the guarantee. Check the installment's fields and labels before adding installments together. Some orgs also classify guarantees by type, such as a standard recouping guarantee, a flat fee, an audit or marketing fee, or a non-recouping guarantee. Only recouping guarantees can be "behind". If the type field is blank on most records, say so and state how you treated them.

## Granted right

What the licensee is actually allowed to do. An agreement can have several granted rights, each one a bundle of:

- **Brands:** which properties or brands may be used.
- **Product categories:** what kind of products. Categories form a tree, and rights are usually granted on the most specific level.
- **Territories:** the countries where products may be sold.
- **Distribution channels:** where they may be sold, such as mass retail, specialty stores or online.
- **Royalty rates:** the percentage or amount owed on sales, which can differ by channel, territory or category.

A granted right can link to the minimum guarantee it counts toward. Granted rights have their own dates and can also carry a sell-off period.

## Submission and SKU (design approvals)

Before a licensee sells a product, they submit it for approval. A **submission** moves through approval stages and statuses that each org configures, such as concept, pre-production and final. Reviewers approve, reject or ask for changes.

A **SKU** is a product. It belongs to both its agreement and the submission it was approved through, and it carries the brand and category it was approved for. Its approval date matters: sales reported before a SKU was approved can be flagged.

## Royalty report and lines

A **royalty report** is the licensee's statement of sales for one reporting period on one agreement. Each **line** is a sale or group of sales: the SKU, brand, category, territory, channel, quantity, sales amount and royalty.

Report statuses a user sees typically run from **open** (waiting for the licensee) to **submitted** (licensee has reported) to **closed** (processed), with **resubmitted** when a licensee sends a corrected report. Read the actual values from the describe. Reports have a submission deadline based on the agreement's reporting terms. If the agreement has no submission window set, there may be no deadline to compare against.

Each line is checked against the agreement. A line is **valid** only when every check passes, and **invalid** if any fails. The checks cover things like:

- the sale falls within the agreement and granted right dates
- the reporting period matches the agreement's schedule
- the brand, category, territory and channel are covered by a granted right
- the rate and royalty amount match the agreed rate
- the SKU was approved before the sale

Some checks may be switched off or overridden in a given org. The report shows how many of its lines are invalid. Reports with invalid lines usually need attention before they can be processed.

## Invoices

Flowhaven bills installments and royalty overages through invoices. An invoice for a licensee still working off their guarantee can correctly be zero, because their royalties are absorbed by the open balance.

## How the pieces answer common questions

| Question | Records involved |
|---|---|
| Who is behind on their MG? | Minimum guarantee, its agreement and licensee |
| What can this licensee sell in a market? | Granted rights and their territories, channels, categories |
| What is waiting for approval? | Submissions and their stage and status |
| Who has not reported for last quarter? | Royalty reports by agreement and period |
| Where do our royalties come from? | Report lines grouped by brand, territory or channel |

## Glossary

Licensing terms in plain language. Your org may use slightly different labels.

| Term | Meaning |
|---|---|
| Licensor | The company that owns a brand and lets others use it. |
| Licensee | The company that pays to make and sell products using a licensor's brand. |
| Brand or property | A character, logo, name or franchise that can be licensed. Brands can have sub-brands. |
| Agreement | The contract between licensor and licensee that sets what may be sold, where, for how long and for how much. |
| Deal memo | A short agreement of the main terms, often signed before the full contract. |
| Executed | Fully signed and in force. |
| Term | The period an agreement is valid, from start date to end date. |
| Sell-off period | Time after the term ends during which the licensee may still sell remaining stock. |
| Minimum guarantee (MG) | The least the licensee promises to pay over the agreement, whatever their sales. |
| Advance | An upfront payment, usually the first installment of the guarantee. |
| Installment | One scheduled payment toward the guarantee. Some orgs label it "Instalment". |
| Flat fee | A fixed payment that may not count toward the guarantee. |
| Commitment | The total value of the guarantee, the sum of its installments. |
| Invoiced | Part of the guarantee that has been billed. |
| Recoupment | Earning back the guarantee through royalties on sales. |
| Recouped | The amount of the billed guarantee covered by royalties so far. |
| Open balance | Billed guarantee not yet covered by royalties. |
| Unrecouped | Guarantee paid by the licensee that sales have not covered yet. |
| Overage | Royalties earned beyond the guarantee, billed in addition. |
| Granted right | A bundle of permissions in an agreement: brands, product categories, territories, channels and rates. |
| Product category | A type of product, such as plush toys or apparel. Categories form a tree. |
| Territory | A country or region where products may be sold. |
| Distribution channel | Where products are sold, such as mass retail, specialty stores or online. |
| Royalty rate | The percentage or fixed amount owed on each sale. |
| Royalty report | The licensee's statement of sales and royalties for one reporting period. |
| Reporting period | The time a royalty report covers, such as a month or quarter. |
| Report line | One row of a royalty report: product, territory, channel, quantity, sales and royalty. |
| Valid or invalid line | Whether a line passes all checks against the agreement. |
| Submission deadline | The date by which a royalty report is due. |
| Submission | A product design sent by the licensee for approval. |
| Design approval | The review of a submission through stages until it is approved or rejected. |
| Stage | A step in the approval process, such as concept or final. |
| SKU | A specific product, identified by a product code, approved through a submission. |
| Invoice | A bill for an installment or royalty overage. |
| Sandbox | A copy of a Salesforce org used for testing. |
| Production | The live Salesforce org the business runs on. |
| Connector | The link that lets Claude read from your Salesforce org. |
| Namespace | A prefix on the API names of an installed package's objects and fields. |
| Read-only user | A Salesforce user who can view records but not change them. |
