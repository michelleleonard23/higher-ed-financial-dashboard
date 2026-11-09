# Higher Education Financial Dashboard (Power BI)

A multi-page Power BI report I designed and built from scratch on a fictional six-institution public college system. It shows how I approach multi-source data reconciliation, data modeling, DAX, and dashboard design for financial and enrollment reporting.

**All data is fictional.** Institution names, dollar amounts and records are randomly generated. No real student, financial or personnel data is used anywhere in this model. What *is* realistic is the structure: the tables, relationships and reconciliation.

![Financial Overview](screenshots/financial-overview.png)

**[View the full report as a PDF](Higher-Ed-Financial-Dashboard.pdf)** (no Power BI needed) or download the [Power BI file](Higher-Ed-Financial-Dashboard.pbix).

## Report Pages

| Page | What it shows |
| --- | --- |
| **Financial Overview** | Comparative income statement with line items down the side and institutions across the top (revenue, salaries, fringe, operating expenses, capital outlay, transfers, net position), a fiscal year slicer, summary cards, and net position by fiscal year |
| **Ancillary Revenue** | Non-tuition revenue such as building rentals, theatre, daycare and parking |
| **Waiver Overview** | Waiver count, total waiver amount and waiver rate, broken down by waiver type (Senior Citizen, Athletic, Academic Merit, National Guard and more), institution and school |
| **School Costing** | Revenue, indirect cost and net margin rolled up by school |
| **Course Costing** | Pick a single course and compare its direct revenue, allocated indirect cost, net margin and tuition rate across institutions |
| **Bonds** | Bond authorization tracking: authorized, spent, encumbered and available |
| **Enrollment** | Headcount and FTE trends by institution and term, filterable by student level |
| **About** | Data scope, definitions, assumptions and methodology |

## Data Model

- **Financial transactions:** fiscal years 2024 to 2026 (7/1/2023 to 6/30/2026), actuals only (budget, expenditure, revenue)
- **Enrollment, waivers, financial aid and course costing:** Fall 2023 to Spring 2026 (three academic years)
- **Bond authorizations:** 2023 to 2026
- Related across institution, fund, department, account, academic year and term, so every page responds to the same slicers

## Key Measures and Logic

```DAX
Total Direct Revenue = SUM(course_costing[Direct Revenue])

Net Margin % = DIVIDE([Total Net Revenue], [Total Direct Revenue], 0)

Waiver Count =
CALCULATE(
    COUNTROWS(
        SUMMARIZE(ar_waiver, ar_waiver[Student ID], ar_waiver[Term])
    ),
    ar_waiver[Waiver Amount] > 0
)
```

| Measure | Logic |
| --- | --- |
| **Net Position** | Revenue minus expenditures, on actual closed-period totals |
| **Total Available (Bonds)** | Authorized amount minus spent minus encumbered |
| **Net Margin % (weighted)** | Net revenue divided by direct revenue, calculated from summed totals so higher-revenue periods carry more weight |
| **Avg Tuition Rate Per Credit** | Simple average across the terms in scope |
| **Waiver Count** | Students who received a waiver in a term, counted once per student per term even though each student has separate tuition, fees, housing and meal plan charge lines |
| **Waiver Rate %** | Total waiver amount divided by total student charges (tuition, fees, housing, meal plans) |
| **Indirect Cost Allocation** | Cost-per-credit-hour: each building's annual cost (operating plus depreciation) divided by credit hours taught there, applied to each course by its credit hours |

## Methodology Notes

- **Course costing** uses a cost-per-credit-hour allocation, a recognized activity-based costing approach in higher education finance. A 50/50 split between operating and capital/depreciation cost is a simplifying assumption for this model.
- **Bond tracking** shows a complete Authorized / Spent / Encumbered / Available reconciliation as if all of that data lived in one system. In practice, encumbrances are often tracked in a different system than authorizations and spending, which is exactly the kind of gap this model is designed to make visible.
- **FTE** uses a simplified ratio (85% undergraduate, 60% graduate) rather than registrar credit-hour calculations.
- **Transfers net to zero** across the full system on the Financial Overview, because money moved from the State System Office to the institutions cancels out. That is a built-in reconciliation check.
- **Course catalog** is illustrative (three sample courses per school per term), so per-course costs would change with a full catalog.

## Screenshots

### Ancillary Revenue
![Ancillary Revenue](screenshots/ancillary-revenue.png)

### Waiver Overview
![Waiver Overview](screenshots/waiver-overview.png)

### School Costing
![School Costing](screenshots/school-costing.png)

### Course Costing
![Course Costing](screenshots/course-costing.png)

### Bonds
![Bonds](screenshots/bonds.png)

### Enrollment
![Enrollment](screenshots/enrollment.png)

## Tools

Power BI Desktop, DAX, Power Query, data modeling, Excel

## About Me

I'm Michelle Leonard, a finance and data professional with 20+ years of experience turning messy multi-system data into reporting that leaders trust and use. LinkedIn: https://www.linkedin.com/in/michelle-leonard-331126b

More of my work: https://github.com/michelleleonard23/overtime-dashboard
