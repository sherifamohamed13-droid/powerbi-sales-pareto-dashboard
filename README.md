# Sales Performance Dashboard: Pareto & ABC Product Analysis

A four-page Power BI dashboard that analyzes sales for a fictional bicycle and accessories company. It focuses on two questions: **which products drive most of the revenue** (Pareto / ABC analysis) and **which orders are unusually large** (IQR outlier detection).

> **Headline finding:** 69 of 250 products (27.6%) generate 80% of net revenue.

---

## Dashboard Preview

| Overview | Performance & YoY |
|---|---|
| ![Overview](images/01-overview.png) | ![Performance and YoY](images/02-performance-yoy.png) |

| Products & Pareto | Classes & Outliers |
|---|---|
| ![Products and Pareto](images/03-products-pareto.png) | ![Classes and Outliers](images/04-classes-outliers.png) |

---

## Business Questions

- How is revenue trending, and where does it come from (territory, product category)?
- How does performance compare with the same period last year?
- Which products form the "vital few" behind 80% of revenue?
- Which unusually large orders could distort averages such as AOV?

## Data

A fictional sample dataset of a company that sells bicycles and related accessories, taken from an online course. The data covers 2011 to mid-2014; the first and last quarters are partial. The model includes a product dimension, a calendar table, and a small disconnected helper table used for the ABC summary.

---

## Pages & Key Insights

### 1. Overview
- **Net revenue:** $6.92M across 1.47K orders, of which 374 were completed.
- **AOV:** $18.50K. **Order fulfillment rate:** 25.53% (completed orders / total orders). Orders still approved or in process count against this rate, so it should be read with the status chart.
- **Territory:** Canada leads with $2.49M, followed by Northwest ($1.66M).
- **Category:** Bikes generate about 80% of revenue ($5.5M), followed by Components ($1.2M).

### 2. Performance & YoY
- Quarterly net revenue is compared with the same quarter last year.
- Revenue peaks at $1.12M in 2013 Q3 and $1.10M in 2014 Q1; YoY growth in 2014 Q1 is about 80%.
- The 812% growth in 2012 Q2 reflects a very low 2011 base, not a typical growth rate.
- Average order value is highest in France ($22.6K) and the United Kingdom ($22.0K), and lowest in Australia ($6.6K).
- **Note:** the first (2011 Q2) and last (2014 Q2) quarters are partial, so comparisons involving them should be read with care.

### 3. Products & Pareto
- **69 of 250 products (27.6%)** produce **80.03%** of revenue.
- Top product: Mountain-200 Black, 38 ($0.37M).
- Mountain, Road, and Touring Bikes are the top three subcategories ($2.28M, $1.88M, and $1.35M).
- An ABC summary table shows product counts and revenue share per class.

### 4. Classes & Outliers
- **Revenue by class:** A = $5.54M (80.03%), B = $1.04M (15.09%), C = $0.34M (4.89%).
- **Outliers:** 13 orders worth $1.18M (17% of net revenue), roughly five times the average order value per order. Bikes account for $0.86M of that and Components for $0.27M.
- **Quantity vs. revenue:** Clothing sells the most units (6.6K) but contributes only about 2.65% of revenue, while Bikes dominate revenue with fewer units.

---

## Methodology

**Pareto analysis**
Products are ranked by revenue in descending order. A cumulative revenue percentage is calculated for each product and plotted as a line over the revenue columns, with a target line at 80%. The Pareto chart shows the products up to that point, and a dynamic subtitle reports the count.

**ABC classification**
- **A:** products that make up the first 80% of cumulative revenue (the product that crosses the threshold stays in A).
- **B:** products between 80% and 95%.
- **C:** the remaining 5%.

**Outlier detection**
Order value is calculated per order, and the upper bound uses the IQR method: `Q3 + 1.5 × IQR`. Any order above the upper bound is flagged as an outlier. Only unusually large orders are flagged.

---

## Key DAX Measures

```dax
Cumulative Revenue % =
DIVIDE(
    [Cumulative Revenue],
    CALCULATE([Revenue], ALLSELECTED('Product-Dim'[Product]))
)
```

```dax
Target 80% = 0.8
```

```dax
ABC Class =
VAR _cum = [Cumulative Revenue %]
VAR _revPct =
    DIVIDE(
        [Revenue],
        CALCULATE([Revenue], ALLSELECTED('Product-Dim'[Product]))
    )
VAR _before = _cum - _revPct
RETURN
    SWITCH(
        TRUE(),
        _before < 0.8, "A",
        _before < 0.95, "B",
        "C"
    )
```

<!-- Add here after receiving them: Revenue, Cumulative Revenue, Product Rank, Revenue YoY %, and the outlier measures -->

---

## Limitations

- The analysis is **revenue-based**. Cost and profit data are not available, so low-revenue products should not be dropped without checking profitability first.
- The first and last quarters of the data are partial, which affects year-over-year comparisons.
- Pareto and ABC results describe the current period and may change over time.

## Next Steps

- Add profitability (margin) to the Pareto and ABC analysis once cost data is available.
- Repeat the Pareto analysis by quantity sold, customer, and subcategory.
- Check whether Class A products stay stable from year to year.

## Tools

Power BI Desktop, DAX

## Author

Sherif Ashraf · [LinkedIn]([https://www.linkedin.com/in/your-profile](https://www.linkedin.com/in/sherif-ashraf-ba4802185/))
