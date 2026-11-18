# Rocky Mountain Bikes: Sales Performance Dashboard (Power BI)

A 4-page Power BI report analyzing **3 fiscal years of sales (Jul 2023 to Jun 2026)** for Rocky Mountain Bikes (RMB), a bike manufacturer that sells online and through resellers.

**Tools:** Power BI Desktop, Power Query, DAX, star-schema data modelling
**Context:** Group assignment, Applied Data Analytics & Visualization program, Sheridan College (Fall 2026). I built the full report (data cleaning, model, measures and all pages) and shared it with my group for the presentation.

📄 [View the full report as a PDF](RMB-Sales-Report.pdf)

---

## Business questions

1. How did sales and profit change from FY2024 to FY2026, and which channel sells more?
2. What are the monthly patterns, and what might FY2027 look like?
3. Which products and channels make or lose money, and why does the reseller channel earn so little?
4. Where do sales come from, and who are the top customers?

## Key insights

- **Sales more than doubled in two years:** $24.1M in FY2024, $34.2M in FY2025 (+42%), $52.1M in FY2026 (+53%).
- **Resellers bring most of the revenue but almost none of the profit:** resellers are 73% of sales ($81.0M) at a 1.2% margin, while online sales earn a 41.1% margin. The reseller channel lost money overall in FY2026.
- **The likely cause is discounting:** resellers buy at about 41% below list price. Road and Touring bikes lost about $2.1M combined through resellers, and 328 of 633 active resellers were unprofitable.
- **Online orders jumped in FY2026:** orders grew from 4,776 to 23,469, while average order value fell from about $7,150 to $2,221 as accessories and clothing took off online.
- **Bikes are the business:** about 87% of sales. The top 3 products are all Mountain-200 bikes.
- **Sales are seasonal:** November, May, August and September are the strongest months. January and October are the weakest.
- **North America drives about 72% of sales,** led by the US Southwest ($24.3M).

## Recommendations

1. Review reseller discounts on bikes, or set a minimum reseller price, to protect margin.
2. Plan stock and promotions around the seasonal peaks.
3. Keep growing the online channel, which is far more profitable.

---

## Report pages

### 1. Executive Overview
![Executive Overview](images/01-executive-overview.png)

### 2. Trends and Forecast
![Trends and Forecast](images/02-trends-forecast.png)

### 3. Products and Channels
![Products and Channels](images/03-products-channels.png)

### 4. Customers and Geography
![Customers and Geography](images/04-customers-geography.png)

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

Fiscal calendar table, hierarchies with drill-down (product, geography, fiscal calendar), forecasting on the Analytics pane, scatter chart, decomposition tree, conditional formatting, slicers synced across pages, and page navigation buttons.

## Files in this repo

- `RockyMountainBikes_Group_4.pbix`: Power BI report file (open with Power BI Desktop)
- `RMB-Sales-Report.pdf`: PDF export of all pages
- `images/`: page screenshots and data model

*Raw course data files are not included.*
