# Sales Insights — Detailed Dashboard (Power BI)

[![Power BI](https://img.shields.io/badge/Tool-Power%20BI-blue)](#)  
**Author:** B. Nagaraj (Jeeva) · **Contact:** nagarajjeeva99@gmail.com

---

## Project overview
A polished, portfolio-ready **Power BI** sales dashboard that visualizes revenue and sales quantity across markets, products, and customers. The report includes KPI cards, time-series trend lines, market breakdowns, top-N lists, and interactive year/month slicers — designed to help stakeholders quickly spot trends and opportunities.

This README accompanies the screenshots you provided and a Power BI report template for easy sharing on GitHub.

---

## Preview / Screenshots

Add these screenshots to an `images/` folder in your repo so they render on GitHub:

- `images/Screenshot 2025-10-01 212030.png`  
- `images/Screenshot 2025-10-01 212108.png`  
- `images/Screenshot 2025-10-02 182418.png`

(If you rename the files, update the paths below accordingly.)
<img width="1280" height="716" alt="Screenshot 2025-10-03 212152" src="https://github.com/user-attachments/assets/998caad9-5b24-4942-b02e-247c11e02abd" />

<img width="1279" height="713" alt="Screenshot 2025-10-03 212211" src="https://github.com/user-attachments/assets/9c9104be-7d4c-4b6b-8412-1f003b890516" />

<img width="1300" height="730" alt="Screenshot 2025-10-03 212220" src="https://github.com/user-attachments/assets/7125a88b-51cb-4c98-9a8c-857500758085" />

---

## Key features
- Large KPI cards: Total Revenue and Sales Quantity.  
- Time-series line chart with monthly tooltips and year comparison.  
- Revenue and quantity breakdowns by market (horizontal bar charts).  
- Top 5 Customers and Top 5 Products with Top N filtering.  
- Year and Month slicers with "Select all" behavior.  
- Example DAX for on-the-fly currency conversion (USD → INR).

---

## Data model (recommended)
Use a simple star schema:

**Fact table: `Sales`**  
- SalesID (PK)  
- Date (date)  
- ProductID (FK)  
- CustomerID (FK)  
- Market (string)  
- Quantity (numeric)  
- SalesAmount (numeric)  
- Currency (string) — e.g. `INR`, `USD`

**Dimension tables:**  
- `Products` (ProductID, ProductName, Category, ...)  
- `Customers` (CustomerID, CustomerName, Segment, ...)  
- `Date` (Date, Year, MonthNumber, MonthName, Quarter, IsMonthStart, ...)

---

## Sample DAX measures

Use these sample DAX measures in Power BI (edit conversion rate as needed):

```dax
-- Convert SalesAmount to INR on the fly (example conversion rate: 83 for USD)
SalesAmount_INR =
SUMX(
    Sales,
    Sales[SalesAmount] *
    SWITCH(
        TRUE(),
        Sales[Currency] = "USD", 83,
        Sales[Currency] = "EUR", 90,   -- example
        1                              -- default (already INR)
    )
)

-- Total Revenue (INR)
Total Revenue (INR) = [SalesAmount_INR]

-- Sales Quantity
Sales Quantity = SUM(Sales[Quantity])

-- Revenue by Market (respecting current slicers)
Revenue by Market = CALCULATE([SalesAmount_INR], ALLSELECTED(Sales[Market]))
