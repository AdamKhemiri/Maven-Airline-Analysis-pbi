# Maven Airline Loyalty Analysis — Power BI

**How does flight activity change across seasons and loyalty tiers?**

Flight activity increased from 223,262 in 2017 to 285,546 in 2018. July was the busiest month in both years, while Star led the three displayed loyalty tiers in total flights. This report explores those patterns through time, location, customer profile, and loyalty views.

[Live dashboard](https://app.powerbi.com/view?r=eyJrIjoiMmVlM2ZlNTctYjU4MS00OTAxLTg4MmUtYWNkNjg1Y2Q3MGYzIiwidCI6ImIyZDQ1OTA3LTUwOWItNDQ5Ni05YmQzLTVhNjBjN2YwZjM4MCIsImMiOjN9) · [Data dictionary](Data_Dictionary.md) · [Model](Model/README.md) · [DAX documentation](Dax%20Measures/README.md) · [Validation notes](Docs/Validation.md)

## Project scope

- **Period:** January 2017–December 2018.
- **Data tables:** Customer Flight Activity and Customer Loyalty History, supported by Calendar.
- **Reporting perspectives:** time, province, education, and loyalty tier.
- **Tools:** Power BI, DAX, and data modeling.

The report combines flight activity with customer attributes to compare flights, distance, points, membership, and customer lifetime value. The model separates activity records from customer history and uses different date relationships for activity and cancellation measures.

The repository includes the report screenshots, model diagram, and DAX expressions. Dataset attribution and preparation details remain to be added; see the validation notes.

## Key findings

The annual comparison uses the year selector. Tier figures use All years with the enrollment-type filter cleared.

### 1. Flight activity increased by 27.9%

The report shows **223,262 flights in 2017** and **285,546 in 2018**—an increase of **62,284 flights**. Together, the annual totals reconcile to the **508,808** full-period card.

**Next action:** compare monthly and cohort contributions to identify where the increase occurred. This calculation describes the annual change; it does not attribute the increase to the 2018 promotion.

### 2. July's peak grew faster than the annual total

July flight activity rose from **26,312 in 2017** to **37,936 in 2018**, an increase of **44.2%**, compared with **27.9%** for the year overall. July was the busiest month in both displayed years.

**Next action:** examine which customer groups contributed to the summer peak and use that analysis to plan seasonal loyalty communications. Two years of activity provide a starting point, rather than a complete demand forecast.

![Flight activity and other measures over time](Docs/Screenshots/01-Period.png)

### 3. Star leads the displayed loyalty tiers in flight volume

The loyalty view shows **212,596 flights for Star**, **158,810 for Nova**, and **96,453 for Aurora**. These figures establish the ranking within the chart, but they sum to **467,859**, below the **508,808** headline total. Tier shares of the overall total are therefore excluded from this analysis until the difference is resolved.

**Next action:** reconcile the chart and headline, then compare flights per member using a consistent member definition. A larger total alone does not establish stronger engagement.

[View the loyalty-tier comparison](Docs/Screenshots/04-Loyalty.png)

## Model and report design

- **Activity measures:** flights, distance, issued points, redeemed points, and redemption cost use the flight-month relationship.
- **Member measures:** separate expressions address members with activity rows, members who flew, cancellations, and active-membership history.
- **Customer-level calculations:** CLV and average salary iterate over distinct loyalty numbers.
- **Navigation:** Period, Location, Profile, and Loyalty views, with year and enrollment-type selections.
- **Detail views:** six drill-through destinations and saved tooltip designs support closer inspection.
- **Display helpers:** M_Date Selection produces the reporting-period label; M_Transparent returns a transparent color; the formatted YoY measure supplies the badge text.

[Model diagram](Model/README.md) · [Flight drill-through screenshot](Docs/Screenshots/07-Flights-Booked-Drill-Through.png) · [Tooltip example](Docs/Screenshots/13-Flights-Booked-Tooltips.png)

## Validation and limitations

Year and promotion filters were checked. July 2018's **37,936 flights** carried into the Flights Booked drill-through, and returning preserved the 2018 selection. The annual growth and July comparisons above were calculated directly from displayed totals, independently of the YoY badge.

Three calculation questions remain:

1. **Tier totals:** the all-year chart is **40,949 flights below** the headline. The cause still needs model-level investigation.
2. **Membership:** the supplied enrollment expression subtracts cancellations from activity-table member counts, and the net-members expression subtracts them again. These definitions need correction before using enrollment or net-membership findings.
3. **Customer value:** CLV is a source customer attribute. Its period grouping must be established before interpreting the chart as a change in customer value.

The [validation record](Docs/Validation.md) separates completed checks from remaining work. Proposed corrections have not been applied to the published report.

## Repository guide

| File or folder | Contents |
|---|---|
| [Data_Dictionary.md](Data_Dictionary.md) | Fields, member definitions, ratios, and display helpers |
| [Dax Measures](Dax%20Measures/README.md) | Supplied expressions, dependencies, and correction requirements |
| [Model](Model/README.md) | Relationship diagram and date-filter explanation |
| [Validation](Docs/Validation.md) | Reconciliation results, interaction checks, and remaining work |
| Docs/Screenshots | Main report, detail pages, and tooltips |

The repository contains documentation, DAX expressions, and screenshots. The source files and Power BI file are not included. Current-year and prior-year flight expressions and two average-measure definitions are also missing from the supplied DAX export.

