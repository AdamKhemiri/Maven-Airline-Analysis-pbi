# Maven Airline Report – Power BI

Interactive **airline loyalty** report built in Power BI, covering **Jan 2017 – Dec 2018**.  
The focus is on **clean modeling**, **clear business KPIs**, and an **app-like UX** with drillthrough and tooltips.

---

## **1. Overview**

This report helps stakeholders answer:

- **Executives:** Are flights, members, distance, points, and CLV trending in the right direction?
- **Loyalty / Marketing:** Which geographies, tiers, and customer profiles are driving value?
- **Analysts:** Where should we drill down (period, location, profile) to explain spikes or drops?

The main landing page gives a high-level view; deeper pages and tooltips provide detail only when needed.

---

## **2. Report Experience**

The layout is designed to feel like a small app:

- **Left navigation**  
  - **Period** – Time-based trends (landing page)  
  - **Location** – Performance by province/city  
  - **Profile** – Demographic breakdowns (education, salary, gender, etc.)  
  - **Loyalty** – Enrolments, cancellations, net members  
  - **Export** – Detailed table for ad-hoc analysis / export

- **Top bar**
  - Year slicer (**All / 2017 / 2018**)  
  - Buttons for **Promotion vs Standard** enrollment type  
  - Author badge: *“By Adam Khemiri”*

- **KPI row (Period page)**
  - Flights Booked, Active Members, Distance Traveled, Points Issued, Points Redeemed, Customer Lifetime Value  
  - Each card uses a **sparkline** for quick trend and a **tooltip page** for extra context (e.g., top 5 cities for a selected month).

- **Drillthrough & tooltips**
  - Right-click on a period chart → **Drill through** to detailed pages (Flights, Members, Distance, Points Issued/Redeemed, CLV).  
  - Tooltips show focused summaries without cluttering the main canvas.

---

## **3. Data Model**

Star-schema model with one fact table, one main dimension, and a date dimension:

- **Calendar** (dimension)  
  Daily date table with **Year, Quarter, Month, Start of Month, Start of Quarter, Start of Year**, plus a **Year → Quarter → Month** hierarchy.

- **Customer Flight Activity** (fact – grain: **loyalty member × flight month**)  
  Holds **Total Flights, Distance, Points Accumulated / Redeemed, Dollar Cost of Points Redeemed**, and **Flight Booked Start of Month** for date linking.

- **Customer Loyalty History** (dimension – grain: **one row per loyalty member**)  
  Stores **location (Country/Province/City), demographics (Education, Salary, Gender, Marital Status), Loyalty Card tier, CLV, Enrollment Type**, and **Enrollment / Cancellation dates** plus a **Has Canceled** flag.

- **Measures Table** (utility)  
  A disconnected table that contains all explicit measures used in the report.

Key modeling choices:

- Use of **USERELATIONSHIP** in measures to work with multiple date fields (booking, enrollment, cancellation).  
- Consistent grain and clear separation between **activity** (fact) and **customer attributes** (dimension).

> Full column-level details are in `/docs/Data_Dictionary.md`.

---

## **4. Key Metrics & DAX**

Measures are grouped logically in `/dax/Measures.dax`:

- **Activity & value**
  - `M_Total Flights Booked`, `M_Total Distance Traveled`  
  - `M_Total Points Issued`, `M_Total Points Redeemed`  
  - `M_Total Dollar Cost Points Redeemed`, `M_Total CLV`

- **Loyalty health**
  - `M_Total Members`, `M_Total Loyalty Cancellations`  
  - `M_Loyalty Member Enrollments`, `M_Net Loyalty Members`  
  - `M_Members (Flew in Period)` – members with actual flight activity

- **Ratios & time intelligence**
  - `M_Redemption Rate` (Points Redeemed / Points Issued)  
  - `M_Flights Booked CY`, `M_Flights Booked LY`, `M_Flights Booked YoY %`  
  - `M_Loyal Members Sparkline` – active member count over time  
  - `M_Customer Average Salary`, `M_Avg CLV`, `M_Avg Distance per Flight`  
  - `M_Date Selection` – dynamic label for the current date range

DAX patterns used:

- **CY / LY / YoY%** pattern for trend analysis.  
- **SUMX / AVERAGEX over VALUES(Loyalty Number)** to aggregate CLV and salary at member level.  
- **ALL(Calendar)** + enrollment/cancellation windows to compute running active membership.

---

## **5. Repository Structure**

Intended structure for this project:

- `/DaxMeasures/Measures.dax` – all measures, organized by section.  
- `/Docs/Screenshots/` – screenshots of the project used for portfolio.  
- `/Model/` – screenshot of the model.
- `/Docs/Data_Dictionary.md` – table and measure documentation.  

---

## **6. What This Project Demonstrates**


- Ability to design a **clean semantic model** with an appropriate grain and clear relationships.  
- Use of **DAX best practices** (explicit measures, time intelligence, USERELATIONSHIP, iterators where needed).  
- Focus on **business-oriented KPIs** (redemption rate, net active members, CLV) instead of only raw metrics.  
- Skill in building an **app-like Power BI UX**: custom navigation, drillthrough, and tooltips that keep the main page simple but powerful.

# link for the embed report : 
 https://app.powerbi.com/view?r=eyJrIjoiNjA2MTM0M2YtZjJmOC00MzYzLThhYWYtMzJkMzg2ZDZlYjkxIiwidCI6ImIyZDQ1OTA3LTUwOWItNDQ5Ni05YmQzLTVhNjBjN2YwZjM4MCIsImMiOjN9
