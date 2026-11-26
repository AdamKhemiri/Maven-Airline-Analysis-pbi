# Airline validation notes

The two source CSVs reproduce the 508,808 flights on the headline card. The same records reproduce 467,859 flights when the tier chart’s province filter is applied.

[Project overview](../README.md) · [Data dictionary](../Data_Dictionary.md) · [DAX documentation](../Dax%20Measures/README.md) · [Model explanation](../Model/README.md)

## Evidence and dates

- November 2–3, 2026. Live report interactions, 18 saved report screenshots, model diagram and 22 DAX definitions.
- November 24–25, 2026. The source activity and customer files establish annual and July flight totals, customer-key coverage and member counts. The local Power BI report file identifies the tier and education filters, the Active Members measure and the “Loyal Members” column in the Export table.

## Recorded headline values

The published report displayed these values under the selections indicated.

| Context | Recorded value |
|---|---:|
| All years: Flights Booked | 508,808 |
| 2017: Flights Booked | 223,262 |
| 2018: Flights Booked | 285,546 |
| July 2017 flights | 26,312 |
| July 2018 flights | 37,936 |
| All years: Active Members card | 15,167 |
| All years: Distance Traveled | 762,861,747 |
| All years: Points Issued | 796,549,007 |
| All years: Points Redeemed | 12,300,572 |
| All years: Customer Lifetime Value card | $43,480,906 |
| 2018 Promotion selected: Flights Booked | 44,877 |
| 2018 Promotion selected: Active Members card | 924 |

The headline values were recorded from the report. The November 24 source check confirmed the annual and July flight totals. The local report definition assigns the Active Members card to `M_Members (Flew in Period)`; 15,167 distinct members have positive flight activity in the source. The card therefore counts members who flew during the selected period.

The promotion selection describes the selected group. It does not establish a causal effect of the promotion.

## Arithmetic supporting the case study

| Calculation | Result |
|---|---:|
| Annual total: 223,262 + 285,546 | 508,808 flights |
| Annual difference: 285,546 − 223,262 | 62,284 flights |
| Annual change: 62,284 / 223,262 | 27.9% |
| July difference: 37,936 − 26,312 | 11,624 flights |
| July change: 11,624 / 26,312 | 44.2% |

Percentages are rounded to one decimal place.

The same annual and July totals were reproduced by summing `Total Flights` from the source activity CSV for each year and month.

The [Period screenshot](Screenshots/01-Period.png) shows the July peaks and their displayed flight values.

## Breakdown reconciliation

The [Maven Analytics source archive](https://mavenanalytics.io/data-playground/airline-loyalty-program) contains the activity and customer files. In the Power BI report, the loyalty-tier and education charts each have a **Top 6 Province** visual filter ranked by `M_Total Flights Booked`. The headline card uses the full selection.

To reproduce the comparison, join `Customer Flight Activity.csv` to `Customer Loyalty History.csv` on `Loyalty Number`. All 392,936 activity rows match exactly one of 16,737 customer records. Sum `Total Flights` by province, rank descending, then retain Ontario, British Columbia, Quebec, Alberta, New Brunswick, and Manitoba. Group these rows by loyalty tier or education. The two tables below match the filtered results exactly. Across all provinces the tier totals are 231,051 Star, 172,817 Nova, and 104,940 Aurora.

### Full-period flights by loyalty tier

The [Loyalty screenshot](Screenshots/04-Loyalty.png) displays:

| Tier | Flights |
|---|---:|
| Star | 212,596 |
| Nova | 158,810 |
| Aurora | 96,453 |
| Sum of displayed tiers | 467,859 |
| Headline total | 508,808 |
| Difference | 40,949 |

The other five provinces account for all 40,949 flights outside this visual.

Tier shares of the headline total are excluded from the case-study findings.

### Full-period flights by education

The [Profile screenshot](Screenshots/03-Profile.png) displays:

| Education category | Flights |
|---|---:|
| Bachelor | 290,741 |
| College | 119,515 |
| High School or Below | 22,581 |
| Doctor | 20,634 |
| Master | 14,388 |
| Sum of displayed categories | 467,859 |
| Headline total | 508,808 |
| Difference | 40,949 |

The education and loyalty-tier breakdowns have the same displayed subtotal and headline difference.

Applying the same six-province filter reproduces each education bar. Both charts reconcile to 508,808 when the excluded provinces are restored.

### July flight detail

The saved [Flights Booked drill-through screenshot](Screenshots/07-Flights-Booked-Drill-Through.png) shows July 2017:

| Item | Flights |
|---|---:|
| Star | 10,961 |
| Nova | 8,190 |
| Aurora | 4,999 |
| Sum of displayed tiers | 24,150 |
| Period total | 26,312 |
| Difference | 2,162 |

The live drill-through showed these values for July 2018.

| Item | Flights |
|---|---:|
| Star | 15,991 |
| Nova | 11,686 |
| Aurora | 7,406 |
| Sum of displayed tiers | 35,083 |
| Period total | 37,936 |
| Difference | 2,853 |

### Membership displays

The full-period Loyalty screenshot shows member bars totaling:

`6,378 + 4,709 + 2,857 = 13,944`

The headline Active Members card displays 15,167, a difference of 1,223.

The [Export screenshot](Screenshots/05-Export.png) separately shows a Members total of 16,737. The saved [Active Members tooltip](Screenshots/14-Active-Members-Tooltips.png) displays approximately 17K.

These values cannot be treated as the same membership metric without confirming their assigned measures and filter contexts. Distinct-count measures also require checking whether groups overlap before expecting additivity.

## Date labels and CLV context

Main report screenshots show January 2017–December 2018.

Several saved detail and tooltip screenshots show January 2012–December 2018, including the Points Issued, Points Redeemed, and CLV detail pages and the standalone tooltip images.

The current date-label measure formats the minimum and maximum Calendar dates in context. A wider label therefore does not, by itself, establish flight activity before 2017.

The main CLV card displays $43,480,906. The saved [CLV tooltip](Screenshots/18-CLV-Tooltips.png) displays approximately $134M under the wider January 2012–December 2018 label.

The [CLV detail screenshot](Screenshots/12-CLV--Drill-Through.png) also displays the wider date range and different customer-group totals from the main views.

These observations require checking the customer population, date context, and measure assignments used by each visual. They do not establish an arithmetic error or CLV growth.

## Recorded interaction checks

These interactions were tested in the published report on November 2, 2026.

| Check | Recorded result |
|---|---|
| Period, Location, Profile, and Loyalty pages | Opened successfully. |
| 2017 and 2018 selections | Updated cards and charts. Selection persisted when switching from Profile to Period. |
| YoY badge under the 2017 selection | Continued to display 28%. |
| 2018 Promotion selection | Updated results. Reset restored All years and the overview totals. |
| Drill-through destinations | Six destinations appeared. |
| July 2018 Flights Booked detail | Retained 37,936 flights and the July reporting-period label. The year dropdown displayed All despite the inherited July context. |
| Return from Flights Booked detail | Preserved the 2018 main-view selection. |
| Standard July tooltip | Displayed 37,936 flights. |

The current expressions clarify that the YoY measure explicitly compares 2018 with 2017. Its continued display under a 2017 year selection is therefore not, by itself, evidence of a broken calculation.

Dedicated custom tooltip behavior and the other five drill-through destinations were not individually exercised during that live review. Export behavior, mobile layout, and all cross-filter combinations were not tested.

## Membership calculation interpretation

The [DAX reference](../Dax%20Measures/Measures.DAX) documents 22 expressions, including six definitions related to membership.

| Measure | Current behavior |
|---|---|
| M_Total Members | Counts distinct activity-table members without requiring positive flights. |
| M_Members (Flew in Period) | Counts distinct activity-table members with positive flights. |
| M_Total Loyalty Cancellations | Sums the cancellation indicator under the cancellation-month relationship. |
| M_Loyalty Member Enrollments | Subtracts cancellations from Total Members. |
| M_Net Loyalty Members | Subtracts cancellations again from the preceding result. |
| M_Loyal Members Sparkline | Uses enrollment and cancellation cutoffs to select members for the reporting month. |

The expression named M_Loyalty Member Enrollments does not count enrollment events by enrollment date.

M_Net Loyalty Members algebraically equals:

`M_Total Members − 2 × M_Total Loyalty Cancellations`

In the local Power BI report, the Export table assigns this measure to its “Loyal Members” column. This double subtraction prevents that column from measuring membership growth.

The local report's Active Members card uses the positive-flights measure. The source check reproduced its full-period count.

## Remaining verification scope

| Area | Verification not established |
|---|---|
| Visual assignments | Assignments beyond the checked Active Members card, Export membership column and full-period tier and education charts. |
| Visual filters | Filter combinations beyond the six-province checks and recorded live interactions. |
| Breakdown totals | Other date selections and breakdowns beyond the verified full-period tier and education charts. |
| Relationships | Exact date endpoints and active filtering paths. Customer keys are unique and every source activity row matches one customer. |
| Source quality | Business interpretation of repeated member-month records. The source has 389,065 distinct member-months across 392,936 rows. Summing flight activity retains those records. |
| Membership dates | Enrollment and cancellation transformations, cancellation-flag generation, and cutoff behavior. |
| CLV | Source methodology, currency, customer population, and date role governing each visual. |
| Project materials | The source CSVs come from Maven Analytics. The repository does not establish the full Power Query transformation history or the origin of every report design asset. |
| Full interaction coverage | Remaining detail pages, live custom tooltips, export behavior, mobile layout, and cross-filter combinations. |

## Business interpretation

The annual comparison and visible summer peaks are descriptive observations from the report.

The Star-tier finding describes the ranking within the displayed chart. It does not establish reconciled tier shares or higher engagement per member.

CLV is a source customer attribute aggregated by the report. It is not period revenue or a newly estimated lifetime-value model.

Recommendations are proposed follow-up analyses. No causal promotion effect, realized business improvement, or production-readiness claim is made.
