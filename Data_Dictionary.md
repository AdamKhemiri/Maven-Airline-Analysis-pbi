## Customer Flight Activity (fact, grain: loyalty member × flight month)

| Column                      | Type         | Description                                                      |
|-----------------------------|--------------|------------------------------------------------------------------|
| Loyalty Number              | Text         | Unique identifier of the loyalty program member                  |
| Total Flights               | Whole number | Number of flights taken in the given month                       |
| Distance                    | Whole number | Total distance flown in the given month                          |
| Points Accumulated          | Whole number | Loyalty points earned in the given month                         |
| Points Redeemed             | Whole number | Loyalty points redeemed in the given month                       |
| Dollar Cost Points Redeemed | Currency     | Cost to the airline for the points redeemed in the given month   |
| Flight Booked Start of Month| Date         | First day of the booking month (used to link to the Calendar)    |


## Customer Loyalty History (dimension, grain: one row per loyalty member)

| Column                    | Type         | Description                                                       |
|---------------------------|--------------|-------------------------------------------------------------------|
| Loyalty Number            | Text         | Key – unique loyalty member identifier                            |
| Country                   | Text         | Member country of residence                                       |
| Province                  | Text         | Member state/province                                             |
| City                      | Text         | Member city                                                       |
| Postal Code               | Text         | Postal/ZIP code                                                   |
| Gender                    | Text         | Gender of the member                                              |
| Education                 | Text         | Highest education level (High School, College, Bachelor, etc.)    |
| Salary                    | Currency     | Annual salary band/estimate for the member                        |
| Marital Status            | Text         | Marital status (Single, Married, etc.)                            |
| Loyalty Card              | Text         | Loyalty tier (e.g., Star, Nova, Aurora)                           |
| CLV                       | Currency     | Customer Lifetime Value for the member                            |
| Enrollment Type           | Text         | Loyalty plan type (e.g., Standard, Promotion)                     |
| Enrollment Start of Month | Date         | Month when the member enrolled in the loyalty program             |
| Cancellation Start of Month| Date        | Month when the membership was cancelled (if applicable)           |
| Has Canceled              | Whole number | 1 if member has cancelled, 0 otherwise                            |
| Country Hierarchy         | Hierarchy    | Navigation hierarchy: Country → Province → City                   |


## Calendar (dimension)

| Column                | Type         | Description                                                     |
|-----------------------|--------------|-----------------------------------------------------------------|
| Date                  | Date         | Calendar date (daily grain)                                     |
| Start of Year         | Date         | First day of the calendar year for each row                     |
| Start of Quarter      | Date         | First day of the quarter for each row                           |
| Start of Month        | Date         | First day of the month for each row                             |
| Year                  | Whole number | Calendar year                                                   |
| Month                 | Text         | Month number                                                    |
| Month Name            | Text         | Full month name (January, February, …)                          |
| Month Name (Short)    | Text         | Abbreviated month name (Jan, Feb, …)                            |
| Day Name (Short)      | Text         | Abbreviated day name (Mon, Tue, …)                              |
| Quarter Number        | Whole number | Quarter of the year (1–4)                                       |
| Start of Year Hierarchy| Hierarchy   | Year → Quarter → Month (used for drill-down in visuals)         |


## Core Measures (selection)

| Measure                         | Definition (plain)                                                           |
|---------------------------------|------------------------------------------------------------------------------|
| M_Total Flights Booked          | Total number of flights booked (using the booking month relationship)        |
| M_Total Points Issued           | Total loyalty points accumulated by members                                  |
| M_Total Points Redeemed         | Total loyalty points redeemed by members                                     |
| M_Total Distance Traveled       | Total distance flown by loyalty members                                      |
| M_Total Members                 | Distinct count of loyalty members with flight activity in the selected period|
| M_Total Loyalty Cancellations   | Total number of cancelled loyalty memberships                                |
| M_Total CLV                     | Sum of Customer Lifetime Value across all members                            |
| M_Loyalty Member Enrollments    | Members enrolled (Total Members − Total Cancellations)                       |
| M_Net Loyalty Members           | Net active loyalty members after cancellations                               |
| M_Members (Flew in Period)      | Distinct members who actually flew (Total Flights > 0) in the selected period|
| M_Redemption Rate               | Points Redeemed / Points Issued                                              |
| M_Loyal Members Sparkline       | Running count of active members over time, ignoring future dates             |
| M_Flights Booked CY             | Flights booked in the current year                                           |
| M_Flights Booked LY             | Flights booked in the previous year                                          |
| M_Flights Booked YoY %          | Year-over-year % change in flights booked ((CY − LY) / LY)                   |
| M_Flights Booked YOY% Formatted | Text-formatted version of Flights Booked YoY % (e.g., `YOY: 27%`)            |
| M_Customer Average Salary       | Average salary per loyalty member                                            |
| M_Avg CLV                       | Average Customer Lifetime Value per loyalty member                           |
| M_Avg Distance per Flight       | Average distance traveled per flight                                         |
| M_Date Selection                | Text showing current date range (e.g., `Jan/17 - Dec/18`)                    |
| M_Transparent                   | Helper measure returning transparent RGBA color for cosmetic formatting      |
