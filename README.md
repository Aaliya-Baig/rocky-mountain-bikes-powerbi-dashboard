# Rocky Mountain Bikes: Sales Performance Dashboard (Power BI)

A 4-page Power BI report analyzing **3 fiscal years of sales (Jul 2023 to Jun 2026)** for Rocky Mountain Bikes (RMB), a bike manufacturer that sells online and through resellers.

**Tools:** Power BI Desktop, Power Query, DAX, star-schema data modelling
**Context:** Group assignment, Applied Data Analytics & Visualization program, Sheridan College (Fall 2026). I built the full report (data cleaning, model, measures and all pages) and shared it with my group for the presentation.

📄 [View the full report as a PDF](RMB-Sales-Report.pdf)

---

## Business questions

1. How did sales and profit change from FY2024 to FY2026, and which channel sells more?
2. Which products and channels make or lose money?
3. Where do sales come from, and who are the top customers?
4. What are the monthly patterns, and what might FY2027 look like?

## Key insights

- **Sales more than doubled in two years:** $24.1M in FY2024, $34.2M in FY2025 (+42%), $52.1M in FY2026 (+52.5%).
- **Resellers bring most of the revenue but almost none of the profit:** resellers are 73% of sales ($81.0M) at a 1.2% margin, while online sales earn a 41.1% margin. The reseller channel lost money overall in FY2026.
- **The likely cause is discounting:** resellers buy at about 41% below list price. Bikes sold through resellers have a -0.7% margin, compared with 40.6% online, and many individual resellers are unprofitable.
- **Online customers grew five-fold in FY2026:** from 3.4K to 17.6K. Orders grew from 4,776 to 23,469, while average order value fell from about $7,150 to $2,221 as many more people placed smaller orders.
- **Bikes are the business:** $95M of $110M in sales (about 87%).
- **Sales are seasonal:** November, May, August and September are the strongest months. January and October are the weakest.
- **North America drives about 72% of sales,** led by the US Southwest ($24.3M), then Canada ($16.4M) and the US Northwest ($16.2M).

## Recommendations

1. Review reseller discounts on bikes, or set a minimum reseller price, to protect margin.
2. Plan stock and promotions around the seasonal peaks.
3. Keep growing the online channel, which is far more profitable.

---

## Report pages

### 1. Executive Overview
![Executive Overview](images/01-executive-overview.png)

### 2. Profit by Product and Channel
![Profit by Product and Channel](images/02-profit-by-product-and-channel.png)

### 3. Customers and Geography
![Customers and Geography](images/03-customers-and-geography.png)

### 4. Trends and Forecast
![Trends and Forecast](images/04-trends-and-forecast.png)

---

## Data model

Star schema with one fact table (**Sales**, 121,253 rows) and five dimension tables: **Dates** (custom fiscal calendar, July to June), **Products**, **Customers**, **Resellers** and **Territories**.

![Data model](images/05-data-model.png)

## Data sources and cleaning

Five source files in four formats: CSV, Excel, JSON and Access (.accdb). Issues found and fixed in Power Query:

| Issue | Fix |
|---|---|
| Duplicate rows in Products and Territories | Removed duplicates so relationships work |
| Territories loaded as a JSON list | Converted to a table and expanded records |
| "NA" placeholder in product colour | Replaced with "No Color" |
| Mixed postal codes (numbers and letters) | Set Postal Code to Text |
| Accented customer names | Loaded with Western European encoding |
| Orders and order lines split across two tables | Merged into one Sales fact table |
| No sales amount column | Added Sales Amount, Total Cost and Days to Ship columns |
| Internet sales have no reseller, reseller sales have no customer | Kept and relabelled the -1 "Not Applicable" rows so every sale links to the model |

## DAX measures

All measures are stored in a dedicated `_Measures` table.

| Category | Measures |
|---|---|
| Core | Total Sales, Total Cost, Gross Profit, Profit Margin %, Units Sold |
| Orders and customers | Order Count, Average Order Value, Customer Count, Reseller Count |
| Channel | Internet Sales, Reseller Sales |
| Time intelligence | Sales Last Year, Sales YoY Growth %, Sales Fiscal YTD (year ending June 30) |
| Pricing and operations | List Price Value, Average Discount %, Avg Days to Ship, Sales by Ship Date (uses an inactive relationship with USERELATIONSHIP) |

Examples:

```DAX
Profit Margin % = DIVIDE ( [Gross Profit], [Total Sales] )

Sales Fiscal YTD = TOTALYTD ( [Total Sales], Dates[Date], "6/30" )

Average Discount % = 1 - DIVIDE ( [Total Sales], [List Price Value] )
```

## Features used

Fiscal calendar table, drill-down hierarchies, 12-month forecast on the Analytics pane, scatter chart, decomposition tree, conditional formatting (red = loss), and fiscal year and channel slicers on every page.

## Files in this repo

- `RockyMountainBikes_Group_4.pbix`: Power BI report file (open with Power BI Desktop)
- `RMB-Sales-Report.pdf`: PDF export of all pages
- `images/`: page screenshots and data model

*Raw course data files are not included.*
