# Blinkit Sales & Customer Analysis Dashboard

An end-to-end Power BI dashboard analyzing sales performance, delivery operations, product profitability, and customer retention for a simulated quick-commerce (Blinkit-style) business operating across 12 dark stores.

![Power BI](https://img.shields.io/badge/Power%20BI-F8CD51?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-161616?style=for-the-badge)

---

## 📌 Business Problem

Quick-commerce businesses run on thin margins and tight delivery windows. Leadership needs visibility into three things at once: **is the business growing profitably, are stores delivering on their promise, and are customers coming back?** This dashboard was built to answer those questions across a full sales funnel — from order-level transactions to store-level operations to customer behavior.

## 🛠️ Tools Used

- **Power BI Desktop** — data modeling, DAX, report design
- **DAX** — 20+ custom measures for revenue, profitability, delivery SLAs, and retention
- **Custom theme** — "Blinkit Dark Mode Pro" (yellow `#F8CD51` on near-black `#161616`), built from scratch to match brand identity

## 📊 Screenshots

### Executive Overview (KPIs, Revenue Trend, Orders by Hour, Revenue by Zone)
[Executive Overview](screenshots/page1_executive_overview.png)

### Delivery Performance (On-Time Delivery % by Store — 37%–80% range across 12 stores)
[Delivery Performance](screenshots/page2_delivery_performance.png)

### Product & Category Performance (Revenue vs Profit Margin Quadrant Analysis)
[Product & Category Performance](screenshots/page3_product_performance.png)

### Customer Insights (New vs Returning, Payment Method Mix, Repeat Order Rate)
[Customer Insights](screenshots/page4_customer_insights.png)

## 🔍 Key Insights

- **72% of all orders come from returning customers**, indicating strong retention despite a highly competitive quick-commerce market.
- **On-time delivery performance varies from 37% to 80% across the 12 dark stores** — the gap points to store-level operational issues rather than a systemic problem, making it fixable with targeted intervention.
- **Average order value is nearly identical between new (₹247.57) and returning (₹248.35) customers** — the retention advantage comes from order *frequency*, not basket size.
- **UPI is the dominant payment method at 42% of orders**, more than double the next closest method (Cash on Delivery at ~20%).
- **Personal Care is the top revenue category (₹3.2M)**, while **Bakery & Biscuits has the highest profit margin** despite lower revenue — showing that the highest-revenue category isn't always the most profitable one.

## 🗂️ Dataset Overview

The model uses a star schema with one fact table and two dimension tables:

| Table | Grain / Contents |
|---|---|
| `Sales_Fact` | Line-item level transactions — ProductID, OrderID, OrderHour, StoreID, Quantity, UnitPrice, UnitCost, DiscountPct, PromisedTimeMinutes, DeliveryTimeMinutes, OrderStatus, CustomerType, PaymentMethod |
| `Products_Dim` | Product catalog with Category and SubCategory hierarchy |
| `Stores_Dim` | 12 dark stores with built-in performance variance for realistic operational analysis |

## 🧮 DAX Highlights

A few measures that go beyond simple aggregation:

```DAX
Avg Delivery Time = 
AVERAGEX(
    SUMMARIZE(Sales_Fact, Sales_Fact[OrderID]),
    CALCULATE(AVERAGE(Sales_Fact[DeliveryTimeMinutes]))
)
```
*(SUMMARIZE-based to avoid double-counting delivery times across multiple line items in the same order)*

```DAX
Highest Margin Category = 
VAR _CategoryTable = 
    ADDCOLUMNS(
        VALUES(Products_Dim[Category]),
        "MarginValue", [Profit Margin %]
    )
VAR _TopCategory = 
    TOPN(1, _CategoryTable, [MarginValue], DESC)
RETURN
    MAXX(_TopCategory, Products_Dim[Category])
```

Other measures include: Total Revenue, Gross Profit, Profit Margin %, On-Time Delivery %, Late Delivery Rate %, Cancellation Rate %, Repeat Order Rate %, New/Returning Customer Orders, Revenue YoY %, and Avg Discount %.

## 🚀 How to Use

1. Download `Blinkit_Sales_Dashboard.pbix` from this repo
2. Open in Power BI Desktop (free download from Microsoft)
3. Use the Year, Zone, and Category slicers to explore the data interactively

A full PDF export of all four pages is also included for anyone without Power BI Desktop installed: `Blinkit_Dashboard_Export.pdf`

---

📫 Connect with me: [GitHub](https://github.com/Yash-0204)

