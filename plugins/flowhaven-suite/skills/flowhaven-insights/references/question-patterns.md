# Question patterns

Everyday questions grouped by role. For each one: the records involved and the approach. Field and object names are found by describe at run time, so none are listed here. Always state the org, the time of the query and the basis for each number.

General approach for every question:

1. Identify the records (see `concepts.md`).
2. Describe those objects and pick fields by label and type.
3. Check picklist values on the describe before filtering on a status or stage.
4. Query with filters and aggregates, selecting only what is needed.
5. Answer, then show "How I got this".

## Licensing manager

1. **Which agreements expire in the next 90 days?**
   Agreements with an end date between today and 90 days out, active status only. Show licensee, end date and any sell-off period.

2. **What can Acme Toys sell in Germany, and through which channels?**
   Find the licensee's active agreements, then their granted rights, then the territory and channel children of those rights. Filter territories by the country name. List the brands and categories alongside.

3. **Which agreements are still at deal memo or draft stage?**
   Read the stage or status picklist values first, then count and list agreements in the early values. Group by licensee.

4. **Which licensees have no royalty report for last quarter?**
   Work out the last completed quarter. List active agreements, then reports for that period, and show the agreements with none.

5. **Which brands are licensed in the Nordic countries?**
   Ask or confirm which countries count as Nordic. Find granted rights with those territories, then their brands. Group by brand with the number of licensees.

6. **Show me everything for the Acme Toys plush agreement.**
   One agreement. Summarize term, status, minimum guarantee, granted rights scope and the last few reports. Keep it to a summary, not every field.

7. **Which granted rights end before their agreement does?**
   Compare granted right end dates with their agreement's end date. List mismatches.

## Finance

8. **Which licensees are behind on their MG?**
   Minimum guarantees on active agreements. Compare open balance with what was invoiced and with how much of the term has passed. Rank by open balance. Group by currency.

9. **What is our total open MG balance by licensee?**
   Aggregate open balance on minimum guarantees, grouped by licensee and currency.

10. **Which installments fall due in the next 60 days?**
    Installments with a due date in that window and not yet invoiced. Show agreement, licensee, amount and date.

11. **How much did we earn in royalties last quarter, by brand?**
    Report lines on reports for that period, summing the royalty amount, grouped by brand and currency. Say whether you included only processed reports.

12. **Which licensees are paying overages?**
    Minimum guarantees with an overage above zero. Show the overage and the agreement.

13. **Do our MG balances reconcile?**
    Run the minimum guarantee checks under Health checks below and show the table.

14. **Which agreements have a minimum guarantee but no installments?**
    Minimum guarantees with no installment children, or with a commitment of zero.

## Approvals

15. **Which submissions have been waiting more than 10 days?**
    Submissions in a pending stage or status (read the values first) where the last change or submitted date is more than 10 days ago. Show licensee, brand and stage.

16. **How many submissions did we approve and reject last month?**
    Count submissions by final status for decisions in that month. If the decision date is not clear from the fields, say which date you used.

17. **What is in the approvals queue for the Northwind Licensing brands?**
    Open submissions filtered by the brand or licensor. Group by stage.

18. **Which products were sold before they were approved?**
    Report lines flagged by the SKU approval date check, or lines whose sale date is earlier than the SKU's approval date. List SKU, licensee and period.

19. **What is the average time from submission to approval?**
    For approved submissions with both dates available, compute the average and median gap. Label it as computed by Claude from the dates shown.

## Leadership

20. **Give me a one-page summary of the licensing business this quarter.**
    Active agreements count, royalties this quarter versus last, total open MG balance, reports outstanding, approvals pending. Each figure with its basis. Group money by currency.

21. **Who are our top 10 licensees by royalties this year?**
    Report lines for this year, summed by licensee, top 10. Say whether only processed reports are included.

22. **Which territories are growing?**
    Royalties by territory for this period versus the same period last year. Show both values and the change. Flag small bases.

23. **Where is our biggest risk on guarantees?**
    Largest open balances on agreements nearest their end date. Present as a ranked table and label it as an indicator, not a forecast.

## Operations and data quality

24. **Which royalty reports have invalid lines?**
    Reports with an invalid line count above zero. Show the count per report. For one report, list which checks failed on its lines.

25. **Which reports are past their deadline and still open?**
    Open reports whose submission deadline has passed. Note reports with no deadline set.

26. **Are any report lines missing a product?**
    Count lines with no SKU, grouped by report.

## Tips

- If a question uses a word that could mean several things ("active", "late", "region"), say how you interpreted it.
- For lists, show the top rows and the total count, and offer the rest.
- Do not include contact names, emails or phone numbers unless the question needs them.

## Picking fields and writing queries

- **Retired fields.** Skip fields whose label marks them as retired or unused, for example a "zzz" or "old" prefix, when a current field with the same meaning exists.
- **Same label, two fields.** An object can have two fields with the same or similar label, such as a status picklist and a status lookup. Check the type and values of each, use the one that fits the question, and say which one you used.
- **Mostly empty fields.** Before filtering on a type or status field, check how many records have it filled in (group by it). If most are blank, do not filter on it, since that would silently drop most records. Use another field with the same meaning if one exists, and tell the user.
- **Record types can share a name.** Group by the developer name or ID, and mention duplicates if they affect the answer.
- **Comparing two fields** (for example invoiced against recouped plus open) is not possible inside a query. Fetch the rows with only the fields needed and compare them yourself.
- **Money.** Some amounts carry more than two decimals. Round to cents when presenting, and compare with a small tolerance. In a multi-currency org, query totals are in the corporate currency; record values are in each record's own currency. Do not compare one with the other.
- **Formula fields** often cannot be grouped, and sometimes cannot be filtered. Filter on them instead of grouping, or group the rows yourself after a narrow query.
- **`SELECT COUNT() FROM ...`** returns the answer in `totalSize` with an empty record list. An empty list does not mean zero.
- **Aliases** work only in aggregate queries.
- **Dates.** Use date literals such as `TODAY`, `LAST_N_DAYS:10` or `LAST_QUARTER` rather than typed dates where you can.

## Health checks

Users can ask Claude to check whether their Flowhaven data hangs together. Each check below is stated as an outcome that should be true. Build the queries by describe at run time, run them read-only, and report the result as a table with the query basis.

If a check fails, report it as a finding: which records, by how much, and what a person on the team could look at. Never change or adjust data to make a check pass, and never present your own recalculation as the official balance.

### Minimum guarantees

1. **Everything billed is either earned back or still open.** For each minimum guarantee that still has an open balance, recouped plus open balance should equal what was invoiced. A fully recouped guarantee (open balance zero) can show a recouped amount larger than what was invoiced, because royalties beyond the guarantee may be counted there too. For those, recouped minus invoiced should match the overage. Report fully recouped guarantees separately from true mismatches, and let the team confirm how their org records royalties beyond the guarantee.
2. **The commitment matches the schedule.** The guarantee's total commitment should equal the sum of its scheduled installments that count toward the guarantee. A gap often means a missing or misplaced installment.
3. **The commitment matches the contract.** If the user has the signed amount, compare it with the commitment in the org.
4. **Nothing is overdue for billing by surprise.** Installments dated in the past and not yet invoiced should be the ones the team meant to leave unbilled, such as those on draft or deal memo agreements.

### Royalties

5. **Earned royalties are accounted for.** Every royalty reported on processed reports should show up as recouped or as overage, counted once. Orgs record this in one of two ways: either recouped already includes royalties beyond the guarantee, or recouped stops at the guarantee and the rest is overage. Work out which applies on one or two guarantees that reconcile cleanly, say which you found, then check the rest the same way.
6. **A report's total equals its lines.** The total royalty on a report should equal the sum of its lines.
7. **Processed reports are clean.** Reports that have been processed should have no invalid lines. Reports that are submitted with invalid lines are the ones to work on next.
8. **Every line points at a product.** Report lines should carry a SKU. Lines without one usually mean a reported product code did not match.

### Coverage

9. **Every reporting period is there.** For each active agreement, there should be one report per period from the start of the term to the last completed period. List the gaps.
10. **Open reports are not past their deadline.** Reports still open after their submission deadline are late. Note that some agreements may have no deadline set.
11. **Granted rights are tied to a guarantee where one exists.** On agreements with a minimum guarantee, list granted rights that are not linked to it.
12. **Rights are being used.** Compare what was granted (categories, territories, channels) with what appears in reported sales. Unused scope is a business conversation, not an error. Treat it as an indicator only, since line detail can be coarse.

### How to present results

| Check | Result | Records affected | Basis |
|---|---|---|---|
| Billed equals earned back plus open | Holds for 11 of 12 MGs | MG for Acme Toys, plush 2026 | query on minimum guarantees |

Always give the counts behind "holds" and "fails", the time of the query, and the org name.
