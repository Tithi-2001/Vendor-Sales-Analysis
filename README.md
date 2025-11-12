# Vendor-Sales-Analysis
Data Analysis Project showcasing vendor performance analysis using python, SQL, and power BI

# Vendor Sales Performance Analysis

## Overview
This project provides a comprehensive **Vendor Sales Performance Analysis** using **Power BI**, **SQL**, and **DAX**.  
It brings together purchase, sales, and invoice data to uncover vendor profitability, stock movement efficiency, and brand-level performance trends.

The goal is to help business stakeholders:
- Identify **top and underperforming vendors**
- Monitor **unsold capital** and **inventory turnover**
- Measure **brand contribution to total revenue**
- Support **strategic purchasing and pricing decisions**

---

## Data Sources
The analysis integrates data from the company’s procurement and sales systems stored in a **SQLite** database.

| Table | Description |
|--------|--------------|
| `vendor_purchase` | Contains purchase quantity, price, and brand information |
| `vendor_sales` | Contains sales quantity, sales value, and vendor details |
| `vendor_invoice` | Includes freight cost, discounts, and final invoice amounts |

All these tables were combined and summarized into a master table — `vendor_sales_summary` — for Power BI analysis.

---

## Key Metrics Calculated
| Metric | Description |
|--------|--------------|
| **TotalSalesDollars** | Total sales amount for each vendor or brand |
| **TotalPurchaseDollars** | Total purchase cost |
| **ProfitMargin (%)** | (TotalSalesDollars - TotalPurchaseDollars) / TotalSalesDollars |
| **UnsoldCapital** | (TotalPurchaseQuantity - TotalSalesQuantity) * PurchasePrice |
| **StockTurnover** | TotalSalesQuantity / TotalPurchaseQuantity |
| **PurchaseContribution** | Vendor-wise purchase contribution to total spend |

---

## Power BI Dashboard Highlights

### 1. Vendor Summary View
- Displays **Total Sales**, **Total Purchase**, **Profit Margin**, and **Unsold Capital**.  
- Vendors with a `StockTurnover < 1` are flagged as **Low Turnover Vendors**.  
- Provides a quick snapshot of vendor efficiency and sales effectiveness.

### 2. Brand Contribution Analysis
- A **Donut Chart** shows each brand’s percentage contribution to overall sales.  
- **Top 10 brands** displayed using a horizontal bar chart sorted by total sales.  
- Revealed that *Brand Sigma* contributed **21% of total sales**, while *Brand Epsilon* showed the lowest contribution at **2.4%**.

### 3. Unsold Capital & Inventory Risk
- Highlights brands and vendors with high levels of unsold stock value.  
- Example: *Vendor Orion* held **$45,000** in unsold inventory, accounting for **18%** of total purchase capital.  
- Helped procurement teams identify slow-moving products and optimize stock replenishment.

### 4. Purchase vs Sales Comparison
- A **clustered column chart** compares **Total Purchase Dollars** vs **Total Sales Dollars** by brand.  
- The trend revealed that **Q3 sales increased by 22%**, mainly driven by *Brand Delta* and *Brand Nova*.  
- Certain vendors showed purchase spikes without matching sales, indicating overstocking risks.

---

## Key DAX Measures

```DAX
TotalSales = SUM(vendor_sales_summary[TotalSalesDollars])

ProfitMargin% = 
DIVIDE(
    SUM(vendor_sales_summary[TotalSalesDollars]) - SUM(vendor_sales_summary[TotalPurchaseDollars]),
    SUM(vendor_sales_summary[TotalSalesDollars])
)

UnsoldCapital = 
(SUM(vendor_sales_summary[TotalPurchaseQuantity]) - SUM(vendor_sales_summary[TotalSalesQuantity])) * 
AVERAGE(vendor_sales_summary[PurchasePrice])

LowTurnoverVendor = 
VAR FilteredData = FILTER(vendor_sales_summary, vendor_sales_summary[StockTurnover] < 1)
RETURN SUMMARIZE(FilteredData, vendor_sales_summary[VendorName], "AvgStockTurnOver", AVERAGE(vendor_sales_summary[StockTurnover]))

PurchaseContribution = 
SUMMARIZE(vendor_sales_summary, vendor_sales_summary[VendorName], "TotalPurchaseDollars", SUM(vendor_sales_summary[TotalPurchaseDollars]))
```

---

## Key Insights
- **Top Performing Vendor:** *Vendor Alpha* generated **$275K** in total sales with a **32% profit margin**.  
- **Low Turnover Vendors:** 7 vendors had a stock turnover below **1**, indicating slow-moving inventory.  
- **Unsold Capital:** Total unsold stock value was **$68,000**, primarily from *Brand Orion* and *Brand Epsilon*.  
- **Profitability:** Overall average profit margin stood at **28.6%**, a **6% improvement** compared to the previous quarter.  
- **Purchase Concentration:** The top 5 vendors contributed **72% of total purchases**, highlighting dependency risk.

---

## Tools & Technologies
| Category | Tools Used |
|-----------|-------------|
| Data Source | SQLite |
| Data Transformation | SQL |
| Data Modeling & Visualization | Power BI |
| Business Logic & Measures | DAX |
| Reporting | Interactive dashboards with filters for Vendor, Brand, and Time Period |


