

# DAX Measures & Calculated Columns

## 1. Total Quantity

```DAX
Total Quantity :=
SUM(Order_items[quantity])
```

**Meaning:** Total kitne units/products sell hue.

---

## 2. Total Sales

```DAX
Total Sales :=
SUM(Order_items[Revenue])
```

**Meaning:** Total revenue/sales generated.

---

## 3. Total Orders

❌ Current formula:

```DAX
Total Orders := SUM(Orders[order_id])
```

✅ Correct:

```DAX
Total Orders :=
DISTINCTCOUNT(Orders[order_id])
```

**Meaning:** Total unique orders.

---

## 4. AOV

```DAX
AOV :=
DIVIDE(
    [Total Sales],
    [Total Orders]
)
```

**Meaning:** Ek order ki average value.

**Formula:** Sales ÷ Orders

---

## 5. ASP

```DAX
ASP :=
DIVIDE(
    [Total Sales],
    [Total Quantity]
)
```

**Meaning:** Ek unit ki average selling price.

**Formula:** Sales ÷ Quantity

---

## 6. Total Profit

```DAX
Total Profit :=
SUM(Order_items[Profit])
```

**Meaning:** Total profit generated.

---

## 7. Profit Margin %

```DAX
Profit Margin % :=
DIVIDE(
    [Total Profit],
    [Total Sales]
)
```

**Meaning:** Sales ke ₹100 mein kitna profit hai.

Example: 30% → ₹100 sales par ₹30 profit.

---

## 8. Unique Products Sold

Aapke current formula mein:

```DAX
DISTINCTCOUNT(Products[product_id])
```

ye **total products in Products table** count karega.

Agar specifically **sold products** chahiye:

```DAX
Unique Products Sold :=
DISTINCTCOUNT(Order_items[product_id])
```

**Meaning:** Kitne different products actually sell hue.

---

## 9. Total Customers

```DAX
Total Customers :=
DISTINCTCOUNT(Customers[customer_id])
```

**Meaning:** Total unique customers.

---

# Time Intelligence

## 10. Total YTD

```DAX
Total YTD :=
TOTALYTD(
    [Total Sales],
    'Calendar'[Date]
)
```

**Meaning:** Year ke beginning se selected date tak ki sales.

---

## 11. Total MTD

```DAX
Total MTD :=
TOTALMTD(
    [Total Sales],
    'Calendar'[Date]
)
```

**Meaning:** Current month ke beginning se selected date tak ki sales.

---

## 12. Total QTD

```DAX
Total QTD :=
TOTALQTD(
    [Total Sales],
    'Calendar'[Date]
)
```

**Meaning:** Current quarter ke beginning se selected date tak ki sales.

---

# Customer Analysis

## 13. Repeat Customers

```DAX
Repeat Customers :=
COUNTROWS(
    FILTER(
        VALUES(Orders[customer_id]),
        CALCULATE([Total Orders]) > 1
    )
)
```

**Meaning:** Aise customers jinhone 1 se zyada orders kiye.

---

# Product Ranking

## 14. Product Sales Rank

```DAX
Product Sales Rank :=
RANKX(
    ALL(Products[product_name]),
    [Total Sales],
    ,
    DESC
)
```

**Meaning:** Products ko sales ke basis par rank karta hai.

**1 = highest sales**

---

## 15. Top 5 Product Sales

```DAX
Top 5 Product Sales :=
IF(
    [Product Sales Rank] <= 5,
    [Total Sales],
    BLANK()
)
```

**Meaning:** Sirf top 5 products ki sales show karta hai.

---

# Previous Year / Growth

## 16. Sales Previous Year

```DAX
Sales Previous Year :=
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR(Calendar[Date])
)
```

**Meaning:** Same period ki previous-year sales.

---

## 17. Sales YoY %

```DAX
Sales YoY % :=
DIVIDE(
    [Total Sales] - [Sales Previous Year],
    [Sales Previous Year]
)
```

**Meaning:** Previous year ke comparison mein sales kitni % change hui.

---

# Cost & Returns

## 18. Total Cost

```DAX
Total Cost :=
SUM(Order_items[Total_Cost])
```

**Meaning:** Total product/order cost.

---

## 19. Returned Orders

```DAX
Returned Orders :=
CALCULATE(
    [Total Orders],
    Orders[order_status] = "Returned"
)
```

**Meaning:** Returned status wale unique orders.

---

## 20. Return Rate %

```DAX
Return Rate % :=
DIVIDE(
    [Returned Orders],
    [Total Orders]
)
```

**Meaning:** Total orders mein se kitne % orders returned hain.

---

# Product Contribution

## 21. Sales Contribution %

```DAX
Sales Contribution % :=
DIVIDE(
    [Total Sales],
    CALCULATE(
        [Total Sales],
        ALL(Products)
    )
)
```

**Meaning:** Product/category ki total business sales mein contribution.

Example: **Laptop = 29%** → displayed context mein laptop sales total sales ka 29% contribute karti hai.

---

# Calculated Columns

Ye **Measures nahi**, row-level Calculated Columns hain.

## 22. Total Cost per Order Item

```DAX
=Order_items[quantity] * RELATED(Products[cost_price])
```

**Meaning:** Quantity × Cost Price = total cost.

---

## 23. Revenue per Order Item

```DAX
=Order_items[quantity] * Order_items[unit_price]
```

**Meaning:** Quantity × Selling Price = revenue.

---

## 24. Profit Margin per Row

```DAX
=DIVIDE(
    Order_items[Profit],
    Order_items[Revenue]
)
```

**Meaning:** Individual order-item row ka profit margin.

⚠️ Overall dashboard **Profit Margin** ke liye Measure use karna better hai:

```DAX
Profit Margin % :=
DIVIDE([Total Profit], [Total Sales])
```

---

## 25. Profit per Order Item

Aapka formula:

```DAX
=Order_items[Revenue] - [Total_Cost]
```

Agar `Total_Cost` **column** hai, to:

```DAX
=Order_items[Revenue] - Order_items[Total_Cost]
```

**Meaning:** Revenue − Cost = Profit.

---

# Calendar Columns

Ye Calendar table mein **Calculated Columns / Excel formulas** ke roop mein use hote hain.

## 26. Year

```DAX
=YEAR([Date])
```

**Meaning:** Date se year nikalta hai.

Example: `2026`

---

## 27. Month Number

```DAX
=MONTH([Date])
```

**Meaning:** Month ka number.

January = 1
December = 12

---

## 28. Month Name

```DAX
=FORMAT([Date],"MMMM")
```

**Meaning:** Month ka naam.

Example:

`January`, `February`, `March`

---

## 29. Month-Year

```DAX
=FORMAT([Date],"MM-YYYY")
```

**Meaning:** Month aur year ko ek saath show karta hai.

Example:

`09-2026`

---

## 30. Weekday Number

```DAX
=WEEKDAY([Date])
```

**Meaning:** Week ke day ka number.

---

## 31. Day Name

```DAX
=FORMAT([Date],"DDDD")
```

**Meaning:** Day ka naam.

Example:

`Monday`, `Tuesday`

---

## 32. Quarter

```DAX
="Q"&ROUNDUP(MONTH([Date])/3,0)
```

**Meaning:** Date ko quarter mein convert karta hai.

```text
Jan–Mar  → Q1
Apr–Jun  → Q2
Jul–Sep  → Q3
Oct–Dec  → Q4
```
# ⭐ GitHub ke liye Short Summary


| Area         | Measures / Columns                |
| ------------ | --------------------------------- |
| Sales        | Total Sales, Total Quantity       |
| Orders       | Total Orders, AOV                 |
| Products     | ASP, Product Rank, Top 5          |
| Profit       | Total Profit, Profit Margin       |
| Customers    | Total Customers, Repeat Customers |
| Returns      | Returned Orders, Return Rate      |
| Growth       | Previous Year, YoY Growth         |
| Time         | YTD, MTD, QTD                     |
| Contribution | Sales Contribution %              |
| Calendar     | Year, Month, Quarter, Weekday     |


