# Airline validation record

Review date: November 2, 2026. Evidence includes live report interactions, supplied local measures, documentation, model image, and existing screenshots. No full source dataset, PBIX, complete measure export, or visual-binding metadata was inspected. Existing screenshots were reused unchanged.

## Observed values

| Context | Value |
|---|---:|
| All years: Flights Booked | 508,808 |
| 2017: Flights Booked | 223,262 |
| 2018: Flights Booked | 285,546 |
| Annual change calculated from cards | 62,284; approximately 27.9% |
| July 2017 flights | 26,312 |
| July 2018 flights and tested drill-through | 37,936 |
| All years: Active Members label | 15,167; measure binding unverified |
| All years: Distance Traveled | 762,861,747; unit unverified |
| All years: Points Issued / Redeemed | 796,549,007 / 12,300,572 |
| All years: CLV label | $43,480,906; date and population meaning unverified |
| 2018 Promotion selected | 44,877 flights; 924 Active Members label; not evidence of causal promotion lift |

## Reconciliation failures to investigate

### Case-study arithmetic

- Annual difference: 285,546 − 223,262 = 62,284 flights.
- Annual growth: 62,284 / 223,262 = 27.9%, rounded to one decimal place.
- July difference: 37,936 − 26,312 = 11,624 flights.
- July growth: 11,624 / 26,312 = 44.2%, rounded to one decimal place.

These are calculations from the observed cards and monthly values, not a validation of the missing CY/LY expressions. The README retains the tier ranking as an explicitly chart-specific observation and excludes tier percentages, active-member growth, CLV growth, and promotion impact claims.

All-year loyalty flights: Star 212,596 + Nova 158,810 + Aurora 96,453 = 467,859. Headline 508,808 minus bars = **40,949**.

July 2018 drill-through: Star 15,991 + Nova 11,686 + Aurora 7,406 = 35,083. Period total 37,936 minus bars = **2,853**.

All-year loyalty member bars sum to 13,944 (6,378 + 4,709 + 2,857), versus a 15,167 card. This difference requires checking both member definitions and filter contexts; distinct counts are not automatically additive across overlapping groups.

No cause is confirmed. Inspect bindings and filters first, then relationship paths, blank/excluded categories, key coverage and duplicates. Do not publish tier shares as reconciled percentages of the headline.

## Interactions checked

- Period, Location, Profile, and Loyalty views opened.
- 2017 and 2018 year selections updated cards and charts; selection persisted when switching Profile to Period.
- The 2017 view still displayed YoY 28%; inspect CY/LY definitions and label context.
- 2018 Promotion selection updated results; reset restored All years and overview totals.
- Six drill-through destinations appeared. July 2018 Flights Booked detail retained 37,936 and the July date label; year dropdown displayed All despite inherited July context.
- Return from Flights Booked detail preserved the 2018 main-view selection.
- A standard July tooltip displayed 37,936. Dedicated custom tooltip pages and the other five detail destinations were not individually exercised.
- Export, mobile layout, and all cross-filter combinations were not tested.

## Priorities

**Before strong membership/tier claims:** reconcile tier/headline totals, identify Active Members bindings, replace the misdefined enrollment/net-members logic in the actual model, and validate customer/date keys.

**Before CLV or promotion conclusions:** establish CLV population/date meaning, source definitions, comparable cohort windows, and denominators. Observed cohort differences do not prove campaign impact.

**Documentation improvements in this copy:** correct folder links, use the tested dashboard URL, describe Loyalty as tier analysis, distinguish membership concepts, and document missing measures. These are completed documentation changes, not model fixes.

**Later presentation work:** keep large KPI values readable, display inherited filters on drill-through, and use an explicit no-comparison label when YoY is blank.

## Evidence needed to finish

1. PBIX/PBIP or complete relationship metadata, current expressions, and visual-field/filter bindings.
2. Original source files, download attribution, transformations, and any course/template contribution.
3. Tests for duplicate customer keys, duplicate member-month rows, unmatched loyalty IDs, missing tier labels, zero-flight records, and enrollment/cancellation dates.
4. Reconciliation results under All years, each year, July 2018, each tier, and each enrollment type after any fix.

The annual card comparison and visible summer peaks are descriptive observations. Underlying source accuracy, tier completeness, causal effects, and production readiness are not certified.
