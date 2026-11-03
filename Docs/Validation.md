# Airline validation notes

This document records the evidence supporting the Maven Airline case study, the scope of the checks performed, and relevant interpretation limits.

[Project overview](../README.md) · [Data dictionary](../Data_Dictionary.md) · [DAX documentation](../Dax%20Measures/README.md) · [Model explanation](../Model/README.md)

## Review scope

The November 2, 2026 review recorded live report interactions and examined the available documentation, DAX expressions, model diagram, and saved screenshots.

The November 3, 2026 documentation review inspected all 18 saved report screenshots, the model diagram, and the repository documents. The report author subsequently supplied 22 current measure definitions.

The documentation review did not repeat the earlier live interaction checks. No complete source dataset, PBIX, or visual-field assignment metadata was inspected.

Saved screenshots represent particular report states. Differences between screenshots must be interpreted alongside their displayed dates, selections, and visual configurations.

## Recorded headline values

The earlier review recorded the following values.

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

The headline values are report observations. The Active Members card's assigned measure, the distance unit, and the CLV population represented by the card have not been independently confirmed.

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

These calculations use recorded annual and monthly values. They do not establish complete source-data reconciliation or independent execution testing of the DAX measures.

The [Period screenshot](Screenshots/01-Period.png) shows the July peaks and their displayed flight values.

## Breakdown reconciliation

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

The chart establishes the ranking of the displayed tiers. Its bars do not reconcile to the headline total in that saved state.

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

This shared difference supports investigating their common measure assignments, customer filtering, and relationship paths. It does not establish a specific cause.

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

The earlier live review separately recorded July 2018:

| Item | Flights |
|---|---:|
| Star | 15,991 |
| Nova | 11,686 |
| Aurora | 7,406 |
| Sum of displayed tiers | 35,083 |
| Period total | 37,936 |
| Difference | 2,853 |

The July 2017 saved screenshot and the July 2018 live-review record represent different periods.

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

The following results were recorded during the November 2, 2026 live review.

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

The subsequent screenshot review inspected their saved appearance, not their live interactions.

## DAX documentation alignment

The report author supplied 22 current measure definitions.

These include the four expressions absent from the earlier repository export:

- M_Flights Booked CY
- M_Flights Booked LY
- M_Avg CLV
- M_Avg Distance per Flight

The current-year and prior-year flight measures explicitly select 2018 and 2017. They are fixed-year expressions rather than dynamically selected current and previous years.

The repository reference groups and formats the supplied expressions without changing their calculation behavior. This documentation update does not modify the published report.

## Membership calculation interpretation

The supplied expressions distinguish several membership concepts:

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

The case study therefore does not use these two expressions as evidence of enrollment-event counts or net membership growth.

The Active Members card's assigned measure remains unconfirmed.

## Remaining verification scope

| Area | Verification not established |
|---|---|
| Visual assignments | Exact measures used by headline cards, breakdown charts, sparklines, detail pages, and tooltips. |
| Visual filters | Agreement of visual-, page-, report-, and inherited drill-through filters. |
| Breakdown totals | Explanation of the differences between displayed category totals and headline values. |
| Relationships | Exact date endpoints, active filtering paths, customer-key coverage, and relationship configuration. |
| Source quality | Duplicate customer keys, duplicate member-month rows, unmatched loyalty numbers, and missing category values. |
| Membership dates | Enrollment and cancellation transformations, cancellation-flag generation, and cutoff behavior. |
| CLV | Source methodology, currency, customer population, and date role governing each visual. |
| Dataset provenance | Original download source, preparation steps, and course or template contributions. |
| Full interaction coverage | Remaining detail pages, live custom tooltips, export behavior, mobile layout, and cross-filter combinations. |

## Business interpretation

The annual comparison and visible summer peaks are descriptive observations from the report.

The Star-tier finding describes the ranking within the displayed chart. It does not establish reconciled tier shares or higher engagement per member.

CLV is a source customer attribute aggregated by the report. It is not period revenue or a newly estimated lifetime-value model.

Recommendations are proposed follow-up analyses. No causal promotion effect, realized business improvement, or production-readiness claim is made.