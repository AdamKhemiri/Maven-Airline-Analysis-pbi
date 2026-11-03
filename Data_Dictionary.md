# Airline data dictionary

Field and measure definitions for the Maven Airline Loyalty Analysis Power BI report.

The report combines customer flight activity with loyalty-history attributes to explore flights, distance, points, membership, and customer lifetime value.

[Project overview](README.md) · [DAX documentation](Dax%20Measures/README.md) · [Model explanation](Model/README.md) · [Validation notes](Docs/Validation.md)

## Customer Flight Activity

The activity table contains member-level flight activity by reporting month.

The documented grain is loyalty member × flight month. Uniqueness at that combination has not been independently verified against the complete source data.

| Field | Definition |
|---|---|
| Loyalty Number | Member identifier used to associate activity with customer loyalty history. |
| Total Flights | Source flight-count field summed by the report's Flights Booked measure. |
| Distance | Source distance amount. The measurement unit is not established by the available documentation. |
| Points Accumulated | Points accumulated in the activity records, labeled Points Issued in the report. |
| Points Redeemed | Points redeemed in the activity records. |
| Dollar Cost Points Redeemed | Recorded monetary cost associated with redeemed points. |
| Flight Booked Start of Month | Month-start date used by the activity measures to select the flight-month relationship. |

“Flights Booked” is the report label for `Total Flights`. The original source definition is needed to distinguish booking activity from completed flights.

The reported flight-activity analysis covers January 2017–December 2018.

## Customer Loyalty History

The customer table provides membership dates, loyalty attributes, and demographic information.

The model diagram places this table on the one side of its relationship with Customer Flight Activity. Its intended grain is one record per Loyalty Number.

| Field | Definition |
|---|---|
| Loyalty Number | Customer identifier used to connect loyalty history with flight activity. |
| Country | Customer country. |
| Province | Customer province. |
| City | Customer city. |
| Postal Code | Customer postal code. |
| Gender | Customer demographic attribute. |
| Education | Customer education category. |
| Marital Status | Customer marital-status category. |
| Salary | Source salary attribute used in the average-salary measure. |
| Loyalty Card | Loyalty-tier category. The displayed tiers are Star, Nova, and Aurora. |
| CLV | Source customer lifetime value attribute. |
| Enrollment Type | Enrollment category, including Standard and 2018 Promotion in the report. |
| Enrollment Start of Month | Enrollment month represented as a month-start date. |
| Cancellation Start of Month | Cancellation month represented as a month-start date. |
| Has Canceled | Cancellation indicator summed by the cancellation measure. Its generation expression is not included in the measure file. |
| Country Hierarchy | Geography hierarchy: Country → Province → City. |

Customer geography describes where members are located. It does not establish flight origins or destinations.

The source methodology for CLV, salary units and missing-value treatment, and monetary currency codes are not established by the supplied measure expressions.

## Calendar

Calendar provides date attributes for reporting selections, labels, and calculations involving flight activity and membership dates.

| Field | Definition |
|---|---|
| Date | Date field referenced by activity and cancellation relationships and the reporting-period label. |
| Year | Calendar year, including the values selected by the fixed 2018-versus-2017 comparison. |
| Quarter Number | Calendar-quarter attribute. |
| Month | Month attribute shown in the model diagram. |
| Month Name | Month-name label. |
| Month Name (Short) | Abbreviated month-name label. |
| Day Name (Short) | Abbreviated day-name label. |
| Start of Month | Month-start date used by the membership sparkline cutoff. |
| Start of Quarter | Quarter-start date attribute. |
| Start of Year | Year-start date attribute. |
| Start of Year Hierarchy | Hierarchy containing year-start, quarter-start, and month-start attributes. |

### Date roles and coverage

The measures use different date roles:

- Flight month for activity measures.
- Cancellation month for cancellation counts.
- Enrollment and cancellation cutoffs for the membership sparkline.

The main report screenshots display January 2017–December 2018. Several saved detail and tooltip screenshots display January 2012–December 2018.

`M_Date Selection` reports the minimum and maximum Calendar dates in the current context. It does not establish that flight activity exists throughout the displayed range.

The complete Calendar generation expression, marked-date-table configuration, and exact relationship endpoints are not included in the measure file.

## Measures Table

The disconnected Measures Table organizes the report's calculations.

The following 22 definitions match the supplied expressions in [Measures.DAX](Dax%20Measures/Measures.DAX).

## Activity measures

These measures use `USERELATIONSHIP` between `Calendar[Date]` and `Customer Flight Activity[Flight Booked Start of Month]`.

| Measure | Definition |
|---|---|
| M_Total Flights Booked | Sum of `Customer Flight Activity[Total Flights]`. |
| M_Total Distance Traveled | Sum of `Customer Flight Activity[Distance]`. |
| M_Total Points Issued | Sum of `Customer Flight Activity[Points Accumulated]`. |
| M_Total Points Redeemed | Sum of `Customer Flight Activity[Points Redeemed]`. |
| M_Total Dollar Cost Points Redeemed | Sum of `Customer Flight Activity[Dollar Cost Points Redeemed]`. |

## Membership measures

| Measure | Current calculation |
|---|---|
| M_Total Members | Distinct loyalty numbers in activity records under the flight-month relationship. |
| M_Members (Flew in Period) | Distinct loyalty numbers in activity records with `Total Flights > 0`, under the flight-month relationship. |
| M_Total Loyalty Cancellations | Sum of `Has Canceled` under the relationship between Calendar Date and Cancellation Start of Month. |
| M_Loyalty Member Enrollments | Total Members minus Total Loyalty Cancellations. |
| M_Net Loyalty Members | Loyalty Member Enrollments minus Total Loyalty Cancellations. |
| M_Loyal Members Sparkline | Distinct loyalty-history members satisfying enrollment and cancellation cutoffs for the selected month. |

### Membership interpretation

These calculations represent different concepts.

`M_Total Members` does not require a positive flight count. `M_Members (Flew in Period)` applies that condition explicitly.

Despite its name, `M_Loyalty Member Enrollments` does not count enrollment events by enrollment date. Its expression subtracts cancellations from activity-table member counts.

`M_Net Loyalty Members` subtracts cancellations a second time. Its expression therefore equals:

`M_Total Members − 2 × M_Total Loyalty Cancellations`

These definitions document the supplied model expressions. They are not used in the case study as verified enrollment-event counts or net membership growth.

### Membership sparkline

The sparkline expression uses the maximum selected `Calendar[Start of Month]` as its cutoff.

It includes loyalty-history records with:

- Enrollment month on or before the cutoff.
- A blank cancellation month, or a cancellation month after the cutoff.

Cancellation in the cutoff month is excluded by the strict greater-than comparison.

The expression removes Calendar filters through `ALL(Calendar)`. The resulting population still depends on the surrounding model and filter context.

### Visual labels

The saved report contains labels such as Members, Active Members, and Loyal Members. The measure assigned to each visual must be identified before treating these labels as interchangeable definitions.

## Customer-level measures

| Measure | Definition |
|---|---|
| M_Total CLV | Sums the maximum source CLV value for each distinct visible loyalty number. |
| M_Avg CLV | Averages the maximum source CLV value for each distinct visible loyalty number. |
| M_Customer Average Salary | Averages the maximum source salary value for each distinct visible loyalty number. |

These measures iterate over distinct members in Customer Loyalty History.

CLV is a source customer attribute. The measures aggregate that attribute rather than estimate a new lifetime value. CLV is not period revenue.

The visible customer population depends on filter context and model relationships. A CLV chart grouped by period must be interpreted according to the date role filtering that population.

Using the maximum value per member does not establish that duplicate or conflicting customer records have been validated.

## Ratios

| Measure | Definition |
|---|---|
| M_Avg Distance per Flight | Total distance divided by total flights, using `DIVIDE` without an alternate result. |
| M_Redemption Rate | Points redeemed divided by points issued, using `DIVIDE` with an alternate result of zero. |

Average distance per flight is calculated from aggregate distance and flight totals. Its distance unit follows the source Distance field.

Redemption rate compares points redeemed and issued within the selected context. Redeemed points may have been earned earlier, so this ratio does not measure the redemption probability of a single issuance cohort.

## Fixed-year flight comparison

| Measure | Definition |
|---|---|
| M_Flights Booked CY | Total flights calculated with `Calendar[Year] = 2018`. |
| M_Flights Booked LY | Total flights calculated with `Calendar[Year] = 2017`. |
| M_Flights Booked YoY % | `(CY − LY) / LY`, using `DIVIDE` without an alternate result. |

The comparison explicitly selects 2018 and 2017. The CY and LY names do not indicate a dynamically selected current year and previous year.

The year conditions replace filtering on the same Calendar Year column. Other applicable filters can still affect the calculation.

## Presentation helpers

| Measure | Definition |
|---|---|
| M_Date Selection | Formats the minimum and maximum selected Calendar dates as `MMM/YY - MMM/YY`. |
| M_Flights Booked YOY% Formatted | Prefixes the formatted flight-comparison percentage with `YOY: `. |
| M_Transparent | Returns `RGBA(255,255,255,0)` for transparent formatting. |

The date label describes Calendar filter context rather than the number of periods containing activity.

The formatted YoY measure returns text. The underlying numeric comparison measure supports calculations.

## Documentation scope

The measure definitions reflect the 22 expressions supplied from the current Power BI model.

The documentation does not establish completed source-data reconciliation, relationship-key validation, or the measure assignments and filters of every visual.

Saved screenshots show differences between some headline totals and breakdown charts, and between some main-page and tooltip values. Those differences require checking the relevant measures, filters, and date context before assigning a cause.

See the [validation notes](Docs/Validation.md) for recorded values and verification scope.