# Airline data dictionary

Field and measure definitions for the Airline report, based on its model diagram and supplied DAX. Remaining source and model checks are listed in Docs/Validation.md.

## Customer Flight Activity

Documented grain: loyalty member × flight month. Test uniqueness before relying on that grain; duplicate member-month records have not been ruled out.

| Field | Meaning / limitation |
|---|---|
| Loyalty Number | Member identifier; repeats across activity periods. Validate joins against loyalty history. |
| Total Flights | Source flight-count field summed by the report. “Flights Booked” is the report label; booking versus flown semantics require the original source definition. |
| Distance | Source distance total. Unit not verified. |
| Points Accumulated | Points issued/earned in activity records. |
| Points Redeemed | Points redeemed in activity records. |
| Dollar Cost Points Redeemed | Monetary redemption-cost field; currency code and valuation method unverified. |
| Flight Booked Start of Month | Month-start date used by the exported flight-activity measures. Transformation not supplied. |

## Customer Loyalty History

Intended grain: one record per Loyalty Number. The diagram shows this table on the one side of the activity relationship; source uniqueness is untested.

| Fields | Meaning / limitation |
|---|---|
| Loyalty Number | Intended customer key. |
| Country, Province, City, Postal Code | Customer geography; not flight origin/destination. |
| Gender, Education, Marital Status | Customer attributes for descriptive grouping. |
| Salary | Source salary attribute; units, missing-value treatment, and negative/zero values require inspection. |
| Loyalty Card | Tier label; observed Star, Nova, Aurora. Completeness unverified. |
| CLV | Source customer lifetime value attribute; calculation methodology and currency unverified. Not period revenue. |
| Enrollment Type | Enrollment category, including Standard and 2018 Promotion in the report. |
| Enrollment Start of Month | Derived enrollment month. |
| Cancellation Start of Month | Derived cancellation month; missing values are treated as not cancelled by the active-membership expression. |
| Has Canceled | Intended 1/0 cancellation indicator; generation logic not included. |
| Country Hierarchy | Country → Province → City hierarchy. |

## Calendar and measures table

Calendar contains Date, Year, Quarter Number, Month, Month Name, Month Name (Short), Day Name (Short), Start of Month, Start of Quarter, and Start of Year. Daily continuity, date-table marking, coverage, and exact relationship endpoints are unverified. The measures table is disconnected and organizes expressions.

## Metric definitions: distinguish these concepts

| Measure | Exported behavior |
|---|---|
| M_Total Flights Booked | SUM of Total Flights using the flight-month relationship. |
| M_Total Distance Traveled | SUM of Distance using the flight-month relationship. |
| M_Total Points Issued / Redeemed | SUM of the corresponding activity field using the flight-month relationship. |
| M_Total Dollar Cost Points Redeemed | SUM of redemption cost with the flight-month relationship. |
| M_Total Members | DISTINCTCOUNT of activity-table Loyalty Number; no positive-flight condition. Counts members with rows, not necessarily members who flew or active memberships at period end. |
| M_Members (Flew in Period) | Distinct activity members with Total Flights > 0 under the flight-month relationship. |
| M_Total Loyalty Cancellations | SUM of Has Canceled with the cancellation-month relationship. Requires validated flags and customer grain. |
| M_Loyalty Member Enrollments | Currently Total Members − Total Loyalty Cancellations. This does not count enrollment events. |
| M_Net Loyalty Members | Currently the preceding measure − Total Loyalty Cancellations, subtracting cancellations twice from Total Members. Not a validated net-membership measure. |
| M_Loyal Members Sparkline | Counts loyalty-history members enrolled by the selected month and not cancelled by that month, removes Calendar filters, retains other context. Cancellation month is excluded via `Cancellation > CurrentMonth`. Remaining relationship/filter behavior must be tested. |
| M_Total CLV | Iterates distinct visible loyalty numbers and sums MAX(CLV) per member. Avoids repeating a value across duplicate rows but can mask conflicting CLV values; time behavior depends on relationships/context. |
| M_Customer Average Salary | Averages MAX(Salary) per visible loyalty number. Does not explicitly remove invalid salary values. |
| M_Redemption Rate | Redeemed / Issued with 0 fallback. Period ratio, not cohort redemption probability; redeemed points may have been earned earlier. |
| M_Flights Booked YoY % | (CY − LY) / LY; CY and LY definitions are absent from the supplied export. |
| M_Date Selection | Displays MIN/MAX Calendar dates; does not prove all intervening periods have activity. |
| M_Transparent | Returns the string `RGBA(255,255,255,0)` for transparent-color formatting. |
| M_Flights Booked YOY% Formatted | Prefixes the formatted percentage with `YOY: ` using `FORMAT([M_Flights Booked YoY %], "00%")`. A blank ratio can leave a label without a percentage. |

## Display helper expressions

Both helpers below are present in the supplied Airline DAX file:

```dax
M_Transparent =
    "RGBA(255,255,255,0)"

M_Date Selection =
    FORMAT(MIN('Calendar'[Date]), "MMM/YY")
        & " - "
        & FORMAT(MAX('Calendar'[Date]), "MMM/YY")
```

M_Date Selection describes the selected Calendar range, such as `Jan/17 - Dec/18`. It does not count activity months. M_Transparent is a color string, unlike the numeric `Transparent = 0` expression supplied in the Sales project.

M_Avg CLV and M_Avg Distance per Flight appear in documentation/model but have no definitions in the supplied file. The Active Members card's binding is unverified. Do not use one membership concept as a substitute for another.
