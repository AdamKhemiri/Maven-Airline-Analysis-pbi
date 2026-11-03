# Airline DAX documentation

The supplied `Measures.DAX` is preserved unchanged for traceability. It is a partial collection of expressions, not a certified model export or bulk import script. No proposed change below has been applied to the live report.

## Confirmed issues in the supplied expressions

1. M_Loyalty Member Enrollments uses activity-table members minus cancellations. Enrollment events must instead be counted from enrollment dates in loyalty history.
2. M_Net Loyalty Members subtracts cancellations again. Algebraically it equals Total Members − 2 × cancellations. It does not establish net membership growth or active memberships.
3. M_Total Members includes zero-flight rows. Its definition must not imply a member actually flew.
4. YoY depends on absent M_Flights Booked CY and LY definitions. The 2017 filtered report still showed 28%, so label/selection behavior needs checking even though 2018 versus 2017 card arithmetic yields about 27.9%.
5. M_Avg CLV and M_Avg Distance per Flight definitions are absent.

## Display helpers included in the export

| Measure | Behavior |
|---|---|
| M_Transparent | Returns `RGBA(255,255,255,0)`, a transparent color string. |
| M_Date Selection | Formats the minimum and maximum Calendar[Date] values as `MMM/YY - MMM/YY`. |
| M_Flights Booked YOY% Formatted | Joins `YOY: ` with the numeric YoY measure formatted as `00%`. |

These are implemented expressions in the supplied file, not proposed additions. The exact Transparent and Date Selection formulas are also shown in the [data dictionary](../Data_Dictionary.md). The original Measures.DAX remains unchanged.

M_Loyal Members Sparkline is documented separately as a membership calculation: although used for a sparkline, its date-cutoff logic affects the business meaning of the result.

## Correct business definitions before implementation

| Intended metric | Required definition |
|---|---|
| Enrollment events | Distinct members whose enrollment date falls in the selected period. |
| Cancellation events | Distinct members whose cancellation date falls in the selected period, with duplicate/flag validation. |
| Net membership change | Enrollment events minus cancellation events during the same period. |
| Active memberships at period end | Members enrolled by the cutoff and not cancelled under an explicitly chosen cutoff convention. |
| Members who flew | Distinct activity members with positive flights in the selected period. |

Do not silently substitute net change for active membership. Define whether a cancellation month is included and how blank dates are treated. Establish the opening membership balance if reconciling ending membership to flows.

## Relationship-sensitive checks

USERELATIONSHIP activates the named date relationship for a calculation; do not assume it eliminates every other route from Calendar to the activity table. Inspect the active Calendar → Loyalty History route and the customer → activity route alongside the flight-date relationship. Exact endpoints must be read from Power BI.

Before writing replacement measures, compare the same base flight measure on the headline and each tier visual. Inspect visual filters, hidden/blank categories, unmatched loyalty numbers, duplicate keys, and whether the tier chart uses a different measure. Reconcile the 40,949-flight full-period difference and 2,853-flight July difference. The cause is not yet established.

## CLV and ratios

The existing CLV iterator aggregates a source attribute once per visible member. It does not estimate CLV or prove a customer-value time series. Inspect which date filters the customer set; if enrollment date drives the series, label it as CLV of enrollment cohorts. Test duplicate members with conflicting CLV values rather than relying on MAX to hide conflicts.

For redemption rate, establish a business rule for zero issued points. Returning 0 may hide an undefined ratio. Explain that same-period redemption and issuance need not belong to the same points cohort.

## Acceptance checks for future changes

Export complete current measures and bindings, record baselines, implement in a working PBIX copy, and test year, month, tier, enrollment type, and empty selections. Confirm additive totals, member distinctions, zero denominators, date cutoffs, and return navigation before publishing. Documentation edits alone do not update the model.
