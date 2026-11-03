# Airline DAX documentation

[Measures.DAX](Measures.DAX) documents the 22 measure definitions supplied from the Maven Airline Power BI model, grouped by reporting purpose.

The `M_` prefix is the project's measure-naming convention. The expressions are individual Power BI measure definitions for reference.

## Activity measures

The following measures use the relationship between `Calendar[Date]` and `Customer Flight Activity[Flight Booked Start of Month]`.

| Measure | Definition |
|---|---|
| `M_Total Flights Booked` | Sum of `Customer Flight Activity[Total Flights]`. |
| `M_Total Distance Traveled` | Sum of `Customer Flight Activity[Distance]`. |
| `M_Total Points Issued` | Sum of `Customer Flight Activity[Points Accumulated]`. |
| `M_Total Points Redeemed` | Sum of `Customer Flight Activity[Points Redeemed]`. |
| `M_Total Dollar Cost Points Redeemed` | Sum of `Customer Flight Activity[Dollar Cost Points Redeemed]`. |

Each expression uses `USERELATIONSHIP` to select the flight-month date relationship for the calculation.

“Flights Booked” is the report's label for the source `Total Flights` field.

## Membership measures

The report contains several membership calculations with different meanings.

| Measure | Current calculation |
|---|---|
| `M_Total Members` | Distinct loyalty numbers in the activity table under the flight-month relationship. No positive-flight condition is applied. |
| `M_Members (Flew in Period)` | Distinct loyalty numbers in activity records where `Total Flights > 0`, under the flight-month relationship. |
| `M_Total Loyalty Cancellations` | Sum of `Has Canceled` under the cancellation-month relationship. |
| `M_Loyalty Member Enrollments` | `M_Total Members` minus `M_Total Loyalty Cancellations`. |
| `M_Net Loyalty Members` | `M_Loyalty Member Enrollments` minus `M_Total Loyalty Cancellations`. |
| `M_Loyal Members Sparkline` | Counts distinct loyalty-history members satisfying enrollment and cancellation cutoffs for the selected month. |

### Interpretation

`M_Total Members` counts members with activity records in context. It does not require a positive flight count and is distinct from `M_Members (Flew in Period)`.

The current expression named `M_Loyalty Member Enrollments` does not count enrollment events by enrollment date. It subtracts cancellations from activity-table member counts.

The current expression named `M_Net Loyalty Members` subtracts cancellations again. Algebraically, it equals:

`M_Total Members − 2 × M_Total Loyalty Cancellations`

These two expressions are documented as implemented. They are not used in the case study as evidence of enrollment-event counts or net membership growth.

### Membership sparkline

`M_Loyal Members Sparkline` takes the maximum selected `Calendar[Start of Month]` as its cutoff. Its filter includes members whose enrollment month is on or before that cutoff and whose cancellation month is either blank or after the cutoff.

A member cancelled in the cutoff month is excluded by the expression's strict greater-than comparison.

The expression removes Calendar filters through `ALL(Calendar)`. Its complete behavior also depends on the surrounding filter context and model relationships.

## Customer-level measures

| Measure | Definition |
|---|---|
| `M_Total CLV` | Sums the maximum source CLV value for each distinct visible loyalty number. |
| `M_Avg CLV` | Averages the maximum source CLV value for each distinct visible loyalty number. |
| `M_Customer Average Salary` | Averages the maximum source salary value for each distinct visible loyalty number. |

These measures iterate over distinct members in `Customer Loyalty History`.

CLV is a source customer attribute. The measures aggregate that attribute; they do not calculate a new lifetime-value estimate or represent period revenue.

The customer population included in a result depends on filter context and model relationships. Interpreting CLV by period therefore requires identifying which date relationship filters that population.

## Ratios

| Measure | Definition |
|---|---|
| `M_Avg Distance per Flight` | Total distance divided by total flights, using `DIVIDE` without an alternate result. |
| `M_Redemption Rate` | Points redeemed divided by points issued, using `DIVIDE` with an alternate result of zero. |

The redemption rate compares redemption and issuance within the selected reporting context. Redeemed points may have been earned in an earlier period, so this ratio is not a cohort redemption probability.

## Fixed-year flight comparison

| Measure | Definition |
|---|---|
| `M_Flights Booked CY` | Total flights calculated with `Calendar[Year] = 2018`. |
| `M_Flights Booked LY` | Total flights calculated with `Calendar[Year] = 2017`. |
| `M_Flights Booked YoY %` | `(CY − LY) / LY`, using `DIVIDE` without an alternate result. |

Despite their CY and LY names, these expressions explicitly select 2018 and 2017. They do not automatically select the current calendar year or the year preceding a user's selection.

The year conditions replace a filter on the same `Calendar[Year]` column. Other filters can still influence the result.

The badge should therefore be interpreted as a 2018-versus-2017 comparison within the remaining applicable filter context.

## Presentation helpers

| Measure | Purpose |
|---|---|
| `M_Date Selection` | Formats the minimum and maximum selected `Calendar[Date]` values as `MMM/YY - MMM/YY`. |
| `M_Flights Booked YOY% Formatted` | Prefixes the formatted flight-comparison percentage with `YOY: `. |
| `M_Transparent` | Returns `RGBA(255,255,255,0)` for transparent formatting. |

The date label describes the Calendar range in context. It does not establish that every date in that range contains flight activity.

The formatted YoY measure returns text. The underlying numeric measure supports calculations.

## Model dependencies

| Table | Fields referenced by the measures |
|---|---|
| `Customer Flight Activity` | Loyalty Number, Total Flights, Distance, Points Accumulated, Points Redeemed, Dollar Cost Points Redeemed, Flight Booked Start of Month |
| `Customer Loyalty History` | Loyalty Number, CLV, Salary, Has Canceled, Enrollment Start of Month, Cancellation Start of Month |
| `Calendar` | Date, Year, Start of Month |

The measure file documents calculation expressions. It does not define table relationships, source transformations, cancellation-flag generation, or which measures are assigned to individual visuals.

## Validation scope

The supplied expressions resolve the earlier missing definitions for current-year flights, prior-year flights, average CLV, and average distance per flight.

Differences between headline cards, breakdown charts, and saved tooltip views require checking their assigned measures and filter contexts. The expressions alone do not establish the cause of those differences.

See the [validation notes](../Docs/Validation.md) for recorded values and verification scope.

## Related documentation

- [Project overview](../README.md)
- [Data dictionary](../Data_Dictionary.md)
- [Model explanation](../Model/README.md)
- [Validation notes](../Docs/Validation.md)