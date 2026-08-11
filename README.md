# Retail Sales Visualization & Analysis

## 1. Project Overview

This project analyzes a **SuperStore retail dataset** using Python. The analysis focuses on **sales, profit, discount, stock, delivery time, and relationships between numerical variables**.

### Tools Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Google Colab

---

## 2. Import Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

**Purpose:** Imports libraries required for data analysis and visualization.

---

## 3. Load Dataset

```python
from google.colab import drive
drive.mount('/content/drive')

df = pd.read_csv("/content/drive/MyDrive/BIG DATA /SuperStore.csv.csv")
```

**Purpose:** Connects Google Drive and loads the SuperStore dataset into a DataFrame.

---

## 4. Initial Data Exploration

### `df.head()`

Displays the first 5 rows to understand the dataset structure.

### `df.info()`

Checks rows, columns, data types, and non-null values.

**Result:** Dataset contains **1000 records and 26 columns**.

### `df.describe()`

Provides statistical information such as mean, minimum, maximum, and standard deviation.

---

## 5. Date Conversion

```python
df['Order Date'] = pd.to_datetime(df['Order Date'])
df['Ship Date'] = pd.to_datetime(df['Ship Date'])
```

**Purpose:** Converts date columns into datetime format for date calculations.

---

## 6. Delivery Days

```python
df['Delivery Days'] = (
    df['Ship Date'] - df['Order Date']
).dt.days
```

**Purpose:** Calculates the number of days taken to ship each order.

A new column called **Delivery Days** is added.

---

## 7. Missing Value Check

```python
df.isnull().sum()
```

**Purpose:** Checks for missing values.

**Result:** All columns have **0 missing values**, so no missing-value treatment is required.

---

# 8. Data Visualization & Analysis

## A. Sales by Category – Bar Plot

```python
category_sales = df.groupby(
    'Category'
)['Sales Amount'].sum()

category_sales.plot(kind='bar')
```
OUTPUT
<img width="922" height="658" alt="image" src="https://github.com/user-attachments/assets/d98077d7-b72b-4c57-afca-33083b0baa45" />

**Purpose:** Compares total sales between product categories.

### Analysis

* Office Supplies has the **highest total sales**.
* Furniture has the **lowest total sales** among the four categories.
* Sales performance is relatively close across the categories.

**Insight:** Office Supplies is the strongest category in terms of total sales.

---

## B. Sales Distribution – Histogram

```python
sns.histplot(df['Sales Amount'], bins=30)
```
OUTPUT
<img width="912" height="604" alt="image" src="https://github.com/user-attachments/assets/92ae16b7-253e-4e15-9894-2f4ba339dbb5" />

**Purpose:** Shows how sales values are distributed.

### Analysis

Most orders are concentrated in the lower-to-middle sales ranges, while a smaller number of orders have very high sales values.

**Insight:** Sales are not evenly distributed; some high-value orders contribute significantly to total sales.

---

## C. Profit by Category – Bar Plot

```python
sns.barplot(
    data=df,
    x="Category",
    y="Profit"
)
```
OUTPUT
<img width="810" height="588" alt="image" src="https://github.com/user-attachments/assets/171296a4-c4ec-4a1d-8097-a549b8c96ce1" />

**Purpose:** Compares the average profit across categories.

### Analysis

The graph shows differences in average profit between product categories.

**Insight:** Some categories generate better profit per order even when their total sales are not the highest.

---

## D. Sales by Category – Bar Plot

```python
sns.barplot(
    data=df,
    x="Category",
    y="Sales"
)
```
OUTPUT
<img width="798" height="581" alt="image" src="https://github.com/user-attachments/assets/ffe5307b-3626-4c11-96b5-b42ceb056b38" />

**Purpose:** Compares sales values across categories.

### Analysis

The categories show similar sales performance, with Office Supplies performing strongly.

**Insight:** No single category completely dominates sales; performance is distributed across categories.

---

## E. Profit Distribution – Box Plot

```python
sns.boxplot(data=df, y="Profit")
```
OUTPUT
<img width="848" height="528" alt="image" src="https://github.com/user-attachments/assets/e44972f3-7c75-46da-8833-3a8b72307d1a" />

**Purpose:** Understands the overall distribution of profit.

### Analysis

The box plot shows the median, spread, and possible high-profit outliers.

**Insight:** Profit varies considerably between orders, with some orders generating much higher profit than typical orders.

---

## F. Profit Variation Across Categories – Box Plot

```python
sns.boxplot(
    data=df,
    x="Category",
    y="Profit"
)
```
OUTPUT
<img width="777" height="576" alt="image" src="https://github.com/user-attachments/assets/1d54a109-c616-4b0d-81e4-5a6ca0d33162" />

**Purpose:** Compares profit variation between categories.

### Analysis

The categories have different median profits and profit spreads. Some categories also contain higher-value profit observations.

**Insight:** Profitability is not uniform across product categories.

---

## G. Discount Values

```python
df["Discount (%)"].unique()
```

**Result:**

```text
0%, 5%, 10%, 15%, 20%
```

**Purpose:** Identifies the different discount levels used in the dataset.

---

## H. Discount vs Profit – Scatter Plot

```python
sns.scatterplot(
    data=df,
    x="Discount (%)",
    y="Profit"
)
```
OUTPUT
<img width="827" height="578" alt="image" src="https://github.com/user-attachments/assets/39b5a03c-40dc-4cc4-822b-482f2b4d80e8" />

**Purpose:** Studies the relationship between discount and profit.

### Analysis

The points are widely scattered and do not show a strong clear pattern.

The correlation is approximately **-0.080**.

**Insight:** Discount has a **very weak negative relationship** with profit in this dataset.

---

# 9. Correlation Analysis

## Selecting Numerical Columns

```python
numeric_df = df.select_dtypes(include="number")
```

**Purpose:** Selects numerical columns for correlation analysis.

---

## Correlation Matrix

```python
corr = numeric_df.corr()
```

**Purpose:** Measures relationships between numerical variables.

### Important Findings

| Relationship                  | Correlation | Analysis               |
| ----------------------------- | ----------: | ---------------------- |
| Sales Amount – Profit         |       0.868 | Strong positive        |
| Sales Amount – Cost Price     |       0.984 | Very strong positive   |
| Quantity – Sales Amount       |       0.620 | Moderate positive      |
| Stock Left – Reorder Quantity |      -0.663 | Moderate negative      |
| Discount – Profit             |      -0.080 | Very weak negative     |
| Delivery Days – Profit        |       0.022 | Almost no relationship |

---

# 10. Correlation Heatmap

```python
sns.heatmap(corr, annot=True)
```
OUTPUT
<img width="807" height="635" alt="image" src="https://github.com/user-attachments/assets/7b997d59-c2b8-40ee-b095-6e855345e75a" />

**Purpose:** Displays all numerical correlations visually.

### Analysis

The heatmap clearly shows that:

* **Sales Amount and Profit** have a strong positive relationship.
* **Sales Amount and Cost Price** have a very strong relationship.
* **Stock Left and Reorder Quantity** have a negative relationship.
* **Discount and Profit** have very little relationship.
* **Delivery Days** has very little relationship with sales and profit.

---

# 11. Overall Insights

From the graphs and correlation analysis:

1. **Office Supplies** has the highest total sales.
2. Sales values are mostly concentrated in lower-to-middle ranges, with some high-value orders.
3. Profit varies considerably between orders and categories.
4. **Sales Amount and Profit** have a strong positive relationship.
5. **Discount does not strongly affect Profit** in this dataset.
6. Lower stock levels are generally associated with higher reorder quantities.
7. Delivery time has very little relationship with sales or profit.

---

# 12. Conclusion

The project uses **Pandas for data analysis** and **Matplotlib/Seaborn for visualization** to convert raw retail data into meaningful business insights.

The analysis shows that **sales performance and profit are strongly related**, while **discount and delivery time have little direct relationship with profit**. The visualizations make it easier to understand category performance, profit variation, sales distribution, and relationships between business variables.
