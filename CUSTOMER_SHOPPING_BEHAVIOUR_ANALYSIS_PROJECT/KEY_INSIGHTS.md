<div align="center">

# 🛍️ CUSTOMER SHOPPING BEHAVIOUR
## KEY INSIGHTS & BUSINESS FINDINGS

**From cleaned customer data to actionable business understanding**

![Project](https://img.shields.io/badge/PROJECT-CUSTOMER%20SHOPPING%20BEHAVIOUR-0F172A?style=for-the-badge)
![Database](https://img.shields.io/badge/DATABASE-MYSQL-0F766E?style=for-the-badge&logo=mysql&logoColor=white)
![Analysis](https://img.shields.io/badge/ANALYSIS-SQL%20EDA-0D9488?style=for-the-badge)
![Records](https://img.shields.io/badge/RECORDS-3%2C900-2563EB?style=for-the-badge)

![Midnight Navy](https://img.shields.io/badge/PALETTE-MIDNIGHT%20NAVY-0F172A?style=flat-square)
![Teal](https://img.shields.io/badge/ACCENT-TEAL-0F766E?style=flat-square)
![Blue](https://img.shields.io/badge/ACCENT-BLUE-2563EB?style=flat-square)
![Amber](https://img.shields.io/badge/ACCENT-AMBER-D97706?style=flat-square)

---

### <span style="color:#0F766E">A data-driven view of customers, products, purchasing patterns and satisfaction.</span>

</div>

## 📌 Executive Summary

This project examines **3,900 customer records** to understand purchasing behaviour, product performance, customer preferences and satisfaction. The raw CSV was cleaned and enriched using **Python and Pandas**, loaded into **MySQL**, and explored through a structured set of SQL analyses—from descriptive summaries to CTEs, joins and window functions.

The strongest overall finding is that **Clothing is the largest product category**, while customer activity is distributed fairly evenly across age groups, purchase-frequency labels, payment methods, shipping types and seasons. The analysis also identifies a clear association between subscription status and discount availability: every subscribed customer in this dataset has a discount recorded.

> [!IMPORTANT]
> The source file labels the monetary field **Purchase Amount (USD)**. Accordingly, this report uses USD. The results describe this dataset; they should not automatically be treated as representative of an entire retail market.

## 📊 Dataset at a Glance

<table>
  <tr>
    <td align="center"><h2>3,900</h2><strong>Customer records</strong></td>
    <td align="center"><h2>3,900</h2><strong>Unique customer IDs</strong></td>
    <td align="center"><h2>19</h2><strong>Cleaned columns</strong></td>
    <td align="center"><h2>25</h2><strong>Products</strong></td>
  </tr>
  <tr>
    <td align="center"><h2>$233,081</h2><strong>Total purchase amount</strong></td>
    <td align="center"><h2>$59.76</h2><strong>Average purchase amount</strong></td>
    <td align="center"><h2>3.75 / 5</h2><strong>Average review rating</strong></td>
    <td align="center"><h2>44.07</h2><strong>Average customer age</strong></td>
  </tr>
</table>

| Dataset metric | Result |
|:--|--:|
| Original CSV shape | 3,900 rows × 18 columns |
| Cleaned dataset shape | 3,900 rows × 19 columns |
| Age range | 18–70 years |
| Product categories | 4 |
| Product colors | 25 |
| Locations | 50 US states |
| Seasons | 4 |
| Payment methods | 6 |
| Shipping methods | 6 |
| Purchase-frequency labels | 7 |

## 🧭 Contents

1. [Data Preparation & Quality](#-data-preparation--quality)
2. [Executive KPI Snapshot](#-executive-kpi-snapshot)
3. [Customer Demographics](#-customer-demographics)
4. [Product Category & Product Performance](#-product-category--product-performance)
5. [Customer Purchasing Patterns](#-customer-purchasing-patterns)
6. [Seasonal & Location Analysis](#-seasonal--location-analysis)
7. [Preferences: Size & Color](#-preferences-size--color)
8. [Customer Satisfaction](#-customer-satisfaction)
9. [Advanced Segmentation & Cross-Analysis](#-advanced-segmentation--cross-analysis)
10. [Business Recommendations](#-business-recommendations)
11. [Analytical Caveats](#-analytical-caveats)
12. [SQL Skills Demonstrated](#-sql-skills-demonstrated)

---

## 🧹 Data Preparation & Quality

The source dataset contained 18 columns. The cleaned dataset contains **19 columns** after removing one redundant field and creating two analytical features.

### Cleaning and validation summary

| Activity | Result | Status |
|:--|--:|:--:|
| Original records | 3,900 | ✅ |
| Original columns | 18 | ✅ |
| Missing review ratings in raw data | 37 | ✅ Treated |
| Missing values after cleaning | 0 | ✅ |
| Duplicate rows | 0 | ✅ |
| Duplicate customer IDs | 0 | ✅ |
| Invalid age values (18–100 check) | 0 | ✅ |
| Invalid purchase amounts (≤ 0 check) | 0 | ✅ |
| Invalid review ratings (outside 1–5) | 0 | ✅ |
| Invalid previous-purchase counts (< 0) | 0 | ✅ |
| IQR outliers in checked numeric columns | 0 detected | ✅ |
| Categorical leading/trailing whitespace | 0 detected | ✅ |
| Unmapped purchase-frequency labels | 0 | ✅ |
| Product names mapped to multiple categories | 0 | ✅ |

### Important preparation decisions

- **Review-rating imputation:** Missing `review_rating` values were filled using the median rating within each product category.
- **Redundant field removal:** `promo_code_used` was removed because it matched `discount_applied` in all 3,900 source records. The retained field is `discount_applied`.
- **Age segmentation:** `age_group` was created with four approximately equal-sized age quartiles using `pd.qcut()`.
- **Purchase-frequency feature:** `purchase_frequency_days` was mapped from the source labels (for example, Weekly = 7, Fortnightly/Bi-Weekly = 14, Monthly = 30, Quarterly/Every 3 Months = 90, Annually = 365).
- **Database loading:** The cleaned 3,900 × 19 dataset was uploaded to the MySQL table `customer` in the `customer_behavior` database and checked in MySQL Workbench.

<details>
<summary><strong>ℹ️ Note about the engineered features</strong></summary>

The age-group names are **dataset-derived quartile labels**, not universal demographic definitions. The purchase-frequency day values are **estimated intervals mapped from categorical labels**, not intervals calculated from actual transaction timestamps.
</details>

## 🎯 Executive KPI Snapshot

| KPI | Result | What it tells us |
|:--|--:|:--|
| Total customer records | 3,900 | Scope of the analysis |
| Unique customer IDs | 3,900 | One record per unique customer in this dataset |
| Total purchase amount | **$233,081** | Aggregate recorded purchase value |
| Average purchase amount | **$59.76** | Average recorded value per customer row |
| Average review rating | **3.75 / 5** | Overall average rating after imputation |
| Average age | **44.07 years** | Average age across the records |
| Age range | 18–70 years | Observed customer age span |

---

## 👥 Customer Demographics

### Gender distribution

| Gender | Customers | Share |
|:--|--:|--:|
| Male | 2,652 | **68.00%** |
| Female | 1,248 | **32.00%** |

**Insight:** Male customers represent about two-thirds of the dataset. This describes the composition of the supplied records; it does not establish the gender composition of the wider market.

![Male share](https://img.shields.io/badge/MALE-68%25-2563EB?style=for-the-badge) ![Female share](https://img.shields.io/badge/FEMALE-32%25-DB72A5?style=for-the-badge)

### Age-group purchasing profile

| Age group | Customers | Average age | Total purchase amount | Average purchase amount | Share of total amount |
|:--|--:|--:|--:|--:|--:|
| Young Adult | 1,028 | 24.65 | $62,143 | $60.45 | **26.66%** |
| Middle-aged | 986 | 51.10 | $59,197 | $60.04 | 25.40% |
| Adult | 942 | 37.97 | $55,978 | $59.42 | 24.02% |
| Senior | 944 | 63.96 | $55,763 | $59.07 | 23.92% |

**Key findings**

- Young Adults lead in both customer count (1,028) and aggregate purchase amount ($62,143).
- Young Adults also have the highest average purchase amount ($60.45) among the four segments.
- The four age groups contribute relatively similar shares of total purchase amount (23.92%–26.66%).

### Combined age-group and gender segmentation

| Age group | Gender | Customers | Share of all records | Total purchase amount | Average purchase amount | Average rating |
|:--|:--|--:|--:|--:|--:|--:|
| Young Adult | Male | 703 | 18.03% | $42,001 | $59.75 | 3.80 |
| Middle-aged | Male | 653 | 16.74% | $39,391 | $60.32 | 3.72 |
| Senior | Male | 654 | 16.77% | $38,279 | $58.53 | 3.74 |
| Adult | Male | 642 | 16.46% | $38,219 | $59.53 | 3.75 |
| Young Adult | Female | 325 | 8.33% | $20,142 | **$61.98** | **3.81** |
| Middle-aged | Female | 333 | 8.54% | $19,806 | $59.48 | 3.69 |
| Adult | Female | 300 | 7.69% | $17,759 | $59.20 | 3.70 |
| Senior | Female | 290 | 7.44% | $17,484 | $60.29 | 3.77 |

**Insight:** Young Adult Male customers form the largest demographic segment and generate the highest total purchase amount ($42,001). Young Adult Female customers have the highest average purchase amount ($61.98) and average rating (3.81) among the eight combinations.

---

## 🛒 Product Category & Product Performance

### Category-level performance

| Category | Purchases | Purchase share | Total purchase amount | Share of total amount | Average purchase amount |
|:--|--:|--:|--:|--:|--:|
| Clothing | 1,737 | **44.54%** | **$104,264** | **44.73%** | $60.03 |
| Accessories | 1,240 | 31.79% | $74,200 | 31.83% | $59.84 |
| Footwear | 599 | 15.36% | $36,093 | 15.49% | **$60.26** |
| Outerwear | 324 | 8.31% | $18,524 | 7.95% | $57.17 |

**Key findings**

- Clothing is the leading category by purchase count and total purchase amount.
- Clothing and Accessories together account for **76.33% of purchase records** and **76.57% of total purchase amount**.
- Footwear has the highest average purchase amount per record ($60.26), despite a smaller overall purchase count.
- Outerwear has the lowest category-level average purchase amount ($57.17).

### Top 10 products by total purchase amount

| Product | Purchases | Total purchase amount | Average purchase amount | Average rating |
|:--|--:|--:|--:|--:|
| Blouse | 171 | **$10,410** | $60.88 | 3.68 |
| Shirt | 169 | $10,332 | $61.14 | 3.62 |
| Dress | 166 | $10,320 | **$62.17** | 3.75 |
| Pants | 171 | $10,090 | $59.01 | 3.72 |
| Jewelry | 171 | $10,010 | $58.54 | 3.76 |
| Sunglasses | 161 | $9,649 | $59.93 | 3.75 |
| Belt | 161 | $9,635 | $59.84 | 3.76 |
| Scarf | 157 | $9,561 | $60.90 | 3.71 |
| Sweater | 164 | $9,462 | $57.70 | 3.76 |
| Shorts | 157 | $9,433 | $60.08 | 3.71 |

**Insight:** Blouse leads the displayed product ranking by total purchase amount ($10,410), while Dress has the highest average purchase amount ($62.17) among these ten products.

### Leading product within each category

| Category | Leading product | Purchases | Total purchase amount | Average purchase amount | Average rating |
|:--|:--|--:|--:|--:|--:|
| Accessories | Jewelry | 171 | $10,010 | $58.54 | 3.76 |
| Clothing | Blouse | 171 | $10,410 | $60.88 | 3.68 |
| Footwear | Shoes | 150 | $9,240 | $61.60 | 3.75 |
| Outerwear | Coat | 161 | $9,275 | $57.61 | 3.73 |

<details>
<summary><strong>Outerwear ranking note</strong></summary>

The supplied source data contains only **two distinct Outerwear products**: Coat and Jacket. Therefore, the top-three-per-category ranking correctly returns only two Outerwear products; there is no third distinct Outerwear product in this dataset.
</details>

---

## 🧾 Customer Purchasing Patterns

### Purchase frequency

| Purchase-frequency label | Mapped days | Customers | Customer share | Total purchase amount | Average purchase amount |
|:--|--:|--:|--:|--:|--:|
| Weekly | 7 | 539 | 13.82% | $31,786 | $58.97 |
| Fortnightly | 14 | 542 | 13.90% | $32,007 | $59.05 |
| Bi-Weekly | 14 | 547 | 14.03% | $33,200 | **$60.69** |
| Monthly | 30 | 553 | 14.18% | $32,810 | $59.33 |
| Quarterly | 90 | 563 | 14.44% | $33,771 | $59.98 |
| Every 3 Months | 90 | 584 | **14.97%** | **$35,088** | $60.08 |
| Annually | 365 | 572 | 14.67% | $34,419 | $60.17 |

**Insight:** Customer counts are distributed fairly evenly across the seven labels. Every 3 Months has the largest group (584), while Bi-Weekly has the highest average purchase amount ($60.69). Fortnightly/Bi-Weekly and Quarterly/Every 3 Months share mapped day values, but remain separate source labels.

### Payment method

| Payment method | Customers | Customer share | Total purchase amount | Average purchase amount |
|:--|--:|--:|--:|--:|
| PayPal | **677** | **17.36%** | $40,109 | $59.25 |
| Credit Card | 671 | 17.21% | **$40,310** | $60.07 |
| Cash | 670 | 17.18% | $40,002 | $59.70 |
| Debit Card | 636 | 16.31% | $38,742 | **$60.92** |
| Venmo | 634 | 16.26% | $37,374 | $58.95 |
| Bank Transfer | 612 | 15.69% | $36,544 | $59.71 |

**Insight:** PayPal is the most frequently used payment method, Credit Card produces the highest total purchase amount, and Debit Card has the highest average purchase amount. The distribution is relatively even across all six methods.

### Shipping method

| Shipping method | Customers | Customer share | Total purchase amount | Average purchase amount |
|:--|--:|--:|--:|--:|
| Free Shipping | **675** | **17.31%** | **$40,777** | $60.41 |
| Standard | 654 | 16.77% | $38,233 | $58.46 |
| Store Pickup | 650 | 16.67% | $38,931 | $59.89 |
| Next Day Air | 648 | 16.62% | $37,993 | $58.63 |
| Express | 646 | 16.56% | $39,067 | $60.48 |
| 2-Day Shipping | 627 | 16.08% | $38,080 | **$60.73** |

**Insight:** Free Shipping has the highest count and total purchase amount. 2-Day Shipping has the highest average purchase amount ($60.73). The data does not show that a shipping option caused customers to spend more.

### Subscription status

| Subscription status | Customers | Share | Total purchase amount | Average purchase amount | Average rating |
|:--|--:|--:|--:|--:|--:|
| No | 2,847 | **73.00%** | $170,436 | **$59.87** | 3.75 |
| Yes | 1,053 | **27.00%** | $62,645 | $59.49 | 3.75 |

**Insight:** 27% of records belong to subscribed customers. Average purchase amounts and average review ratings are very similar between the two subscription groups.

### Discount usage

| Discount applied | Customers | Share | Total purchase amount | Average purchase amount | Average rating |
|:--|--:|--:|--:|--:|--:|
| No | 2,223 | **57.00%** | $133,670 | **$60.13** | 3.76 |
| Yes | 1,677 | **43.00%** | $99,411 | $59.28 | 3.74 |

**Insight:** 43% of records show a discount. Non-discounted records have a slightly higher average purchase amount ($60.13 versus $59.28); this descriptive comparison does not establish the effect of discounts.

---

## 🌦️ Seasonal & Location Analysis

### Seasonal performance

| Season | Customers | Customer share | Total purchase amount | Average purchase amount | Average rating |
|:--|--:|--:|--:|--:|--:|
| Fall | 975 | 25.00% | **$60,018** | **$61.56** | 3.73 |
| Spring | 999 | **25.62%** | $58,679 | $58.74 | **3.79** |
| Winter | 971 | 24.90% | $58,607 | $60.36 | 3.75 |
| Summer | 955 | 24.49% | $55,777 | $58.41 | 3.73 |

**Insight:** Fall leads in total and average purchase amount. Spring has the most customer records and the highest average review rating.

### Seasonal performance by category

| Category | Strongest season by total amount | Total purchase amount | Highest seasonal average amount | Average amount |
|:--|:--|--:|:--|--:|
| Accessories | Fall | **$19,874** | Fall | $61.34 |
| Clothing | Spring | **$27,692** | Fall | $61.41 |
| Footwear | Spring | **$9,555** | Fall | $63.71 |
| Outerwear | Fall | **$5,259** | Fall | $59.76 |

**Insight:** Fall leads in total purchase amount for Accessories and Outerwear, while Spring leads for Clothing and Footwear. Fall has the highest average purchase amount within each of the four categories.

### Location highlights — top 10 by total purchase amount

| Location | Customers | Total purchase amount | Average purchase amount | Average rating |
|:--|--:|--:|--:|--:|
| Montana | 96 | **$5,784** | $60.25 | 3.82 |
| Illinois | 92 | $5,617 | $61.05 | 3.64 |
| California | 95 | $5,605 | $59.00 | **3.83** |
| Idaho | 93 | $5,587 | $60.08 | 3.79 |
| Nevada | 87 | $5,514 | $63.38 | 3.72 |
| Alabama | 89 | $5,261 | $59.11 | 3.78 |
| New York | 87 | $5,257 | $60.43 | 3.76 |
| North Dakota | 83 | $5,220 | $62.89 | 3.75 |
| West Virginia | 81 | $5,174 | **$63.88** | 3.59 |
| Nebraska | 87 | $5,172 | $59.45 | 3.73 |

**Location note:** The earlier result pasted into the conversation contained eight rows. The two additional leading rows (Montana and Illinois) are included here after recalculating the location summary from the uploaded source CSV. California is third by total amount in the complete top-10 ranking, not first. West Virginia has the highest average purchase amount among these ten locations; California has the highest average rating among them.

---

## 🎨 Preferences: Size & Color

### Size preference

| Size | Purchases | Share | Total purchase amount | Average purchase amount | Average rating |
|:--|--:|--:|--:|--:|--:|
| M | **1,755** | **45%** | **$105,167** | $59.92 | 3.72 |
| L | 1,053 | 27% | $61,667 | $58.56 | 3.75 |
| S | 663 | 17% | $40,468 | **$61.04** | **3.80** |
| XL | 429 | 11% | $25,779 | $60.09 | **3.80** |

**Insight:** Medium is the most common size, representing 45% of the records. Medium and Large together account for 72%. Small has the highest average purchase amount; Small and XL share the highest average rating (3.80).

### Color preference

**Standout colors across the 25 available color labels:**

| Measure | Color | Result |
|:--|:--|--:|
| Highest purchase count | Olive | 177 records (4.54%) |
| Highest total purchase amount | Green | **$11,104** |
| Highest average purchase amount | Green | **$65.70** |
| Highest average review rating | Gray | **3.88 / 5** |
| Lowest purchase count | Gold | 138 records (3.54%) |
| Lowest average review rating | Beige | **3.61 / 5** |

Other high-value colors by total purchase amount include Teal ($10,459), Yellow ($10,308), Olive ($10,292) and Cyan ($10,274).

**Insight:** Olive is the most frequently purchased color, but Green leads in both total and average purchase amount. Gray has the highest average rating. Purchase frequency, purchase value and customer satisfaction therefore point to different color leaders.

---

## ⭐ Customer Satisfaction

Review ratings were grouped into four project-defined categories using SQL `CASE WHEN` logic.

| Rating category | Rating band | Customers | Share | Average rating | Average purchase amount |
|:--|:--:|--:|--:|--:|--:|
| Excellent | ≥ 4.5 | 831 | 21.31% | **4.73** | **$60.65** |
| Good | ≥ 4.0 and < 4.5 | 803 | 20.59% | 4.19 | $60.39 |
| Average | ≥ 3.0 and < 4.0 | **1,586** | **40.67%** | 3.45 | $59.52 |
| Below Average | < 3.0 | 680 | 17.44% | 2.73 | $58.51 |

**Key findings**

- The Average segment is the largest group (40.67%).
- Good and Excellent ratings together account for **41.90%** of records.
- Below Average ratings account for 17.44% of records.
- The Excellent segment has the highest average purchase amount ($60.65), while Below Average has the lowest ($58.51).

**Business opportunity:** Investigate the product, delivery and service experiences associated with lower ratings. The ratings alone do not reveal the reason for dissatisfaction; review text or support data would help identify root causes.

---

## 🔬 Advanced Segmentation & Cross-Analysis

### Purchase-value segments

Customers were split into three approximately equal-sized groups with `NTILE(3)`, ordered by individual recorded purchase amount from high to low.

| Segment | Customers | Share | Total purchase amount | Average purchase amount | Observed amount range | Average rating | Average age |
|:--|--:|--:|--:|--:|:--:|--:|--:|
| High | 1,300 | 33.33% | **$113,366** | **$87.20** | $73–$100 | **3.77** | 44.20 |
| Medium | 1,300 | 33.33% | $77,619 | $59.71 | $46–$73 | 3.75 | 43.67 |
| Low | 1,300 | 33.33% | $42,096 | $32.38 | $20–$45 | 3.73 | 44.33 |

**Insight:** The High segment contributes about **48.64% of total purchase amount** ($113,366 of $233,081). Its average rating (3.77) is slightly higher than Medium (3.75) and Low (3.73). The rating difference is only 0.04 points between High and Low, so it should be described as a small observed difference—not a strong or proven relationship.

> [!NOTE]
> These are **single-record purchase-value tiers**, not customer lifetime value segments. The dataset contains one record per unique customer. Because the ranking orders individual purchase amounts, tied values can cross a bucket boundary; the reported High and Medium ranges both include $73.

### Product category preference by gender

| Gender | Category | Purchases | Share within gender | Total purchase amount | Average purchase amount | Average rating |
|:--|:--|--:|--:|--:|--:|--:|
| Female | Clothing | 556 | 44.55% | $33,636 | $60.50 | 3.70 |
| Female | Accessories | 392 | 31.41% | $23,819 | **$60.76** | 3.76 |
| Female | Footwear | 199 | 15.95% | $11,835 | $59.47 | **3.82** |
| Female | Outerwear | 101 | 8.09% | $5,901 | $58.43 | 3.73 |
| Male | Clothing | 1,181 | 44.53% | $70,628 | $59.80 | 3.73 |
| Male | Accessories | 848 | 31.98% | $50,381 | $59.41 | 3.78 |
| Male | Footwear | 400 | 15.08% | $24,258 | **$60.65** | 3.78 |
| Male | Outerwear | 223 | 8.41% | $12,623 | $56.61 | 3.75 |

**Insight:** Category preferences are remarkably similar across genders: Clothing is approximately 44.5% and Accessories approximately 31.5% for both. Female customers have a slightly higher Footwear share (15.95% vs. 15.08%); Male customers have a slightly higher Outerwear share (8.41% vs. 8.09%).

### Subscription and discount relationship

| Subscription status | Discount applied | Customers | Share within subscription group | Total purchase amount | Average purchase amount | Average rating |
|:--|:--|--:|--:|--:|--:|--:|
| No | No | 2,223 | 78.08% | $133,670 | **$60.13** | 3.76 |
| No | Yes | 624 | 21.92% | $36,766 | $58.92 | 3.73 |
| Yes | Yes | 1,053 | **100.00%** | $62,645 | $59.49 | 3.75 |

The validation query confirmed:

| Subscription status | Total customers | With discount | Without discount | Discount rate |
|:--|--:|--:|--:|--:|
| No | 2,847 | 624 | 2,223 | 21.92% |
| Yes | 1,053 | 1,053 | 0 | **100.00%** |

**Insight:** Every subscribed customer in this dataset has `discount_applied = Yes`, while 21.92% of non-subscribed customers have a discount. This is a strong data relationship worth investigating. It does not, by itself, prove that subscription caused discount eligibility or that discounts caused subscription uptake.

---

## 💡 Business Recommendations

The recommendations below are hypotheses to test using the observed patterns—not claims of proven causal impact.

| Priority | Opportunity | Recommended action | Suggested measure |
|:--:|:--|:--|:--|
| 🔴 High | Category concentration | Protect availability and review demand for Clothing and Accessories, which contribute 76.57% of total purchase amount. | Category purchase amount, stock-out rate |
| 🔴 High | Customer satisfaction | Investigate the causes of ratings below 3.0 and test service or product improvements. | Average rating, below-average rating share |
| 🔴 High | Subscription/discount link | Audit discount eligibility rules and compare retention or repeat-purchase outcomes when longitudinal data becomes available. | Subscription rate, renewal rate, discount cost |
| 🟠 Medium | Seasonal planning | Test category-specific seasonal inventory plans: Fall for Accessories/Outerwear; Spring for Clothing/Footwear. | Seasonal category sales, inventory turnover |
| 🟠 Medium | Product-level merchandising | Monitor category leaders such as Blouse, Jewelry, Shoes and Coat; evaluate margin before increasing stock. | Product sales, gross margin, return rate |
| 🟠 Medium | Size availability | Review size-level inventory, especially M and L, which represent 72% of records. | Size-level sales, stock-out rate |
| 🟠 Medium | Color strategy | Investigate Green's high purchase value and Olive's high purchase count; check product mix and margin by color. | Color-level sales, margin, rating |
| 🟡 Explore | Demographic targeting | Test personalized campaigns for Young Adult segments while measuring conversion and satisfaction. | Conversion rate, average order value |
| 🟡 Explore | Shipping options | Compare shipping cost, delivery time and repeat purchases before changing shipping strategy. | Shipping cost, delivery performance, repeat rate |

### Recommended next data to collect

To move from descriptive analysis toward stronger business decisions, future datasets should ideally include transaction dates, order IDs, product costs, discounts as monetary amounts, returns/refunds, delivery time, customer repeat transactions and customer feedback text. These fields would enable time-series analysis, profit analysis, true customer lifetime value, retention analysis and more reliable campaign evaluation.

---

## ⚠️ Analytical Caveats

1. **One row per customer:** The 3,900 customer IDs are unique. The dataset therefore supports customer-level comparisons of the recorded purchase, but not true repeat-purchase or lifetime-value analysis.
2. **Purchase amount is not profit:** Product cost, shipping cost, operating expenses and margin are not available. High purchase amount does not necessarily mean high profitability.
3. **Association is not causation:** Differences by discount, subscription, shipping, season or demographic group are descriptive. Controlled tests or stronger study designs are needed for causal claims.
4. **Imputed ratings:** The raw CSV contained 37 missing review ratings, filled with category-wise medians. Rating averages and satisfaction segments include those imputed values.
5. **Quartile age groups:** `age_group` was created from this dataset's age distribution. Its labels should not be treated as universal demographic definitions.
6. **Mapped purchase intervals:** `purchase_frequency_days` is a mapping from categorical labels, not a measured time between purchases. Fortnightly and Bi-Weekly both map to 14 days; Quarterly and Every 3 Months both map to 90 days.
7. **IQR result:** Zero IQR outliers were flagged in the checked numeric fields using the 1.5 × IQR rule. This does not guarantee that every value is error-free or free of unusual behaviour.
8. **Rounded percentages:** Some displayed percentages may sum to 99.99% or 100.01% because values are rounded to two decimal places.
9. **Location ranking:** The top-10 location table in this report was recalculated from the uploaded raw CSV because the result previously shared in the conversation contained only eight rows.

---

## 🧠 SQL Skills Demonstrated

| SQL skill | How it was used |
|:--|:--|
| `SELECT`, `WHERE`, `LIMIT` | Retrieve and filter records |
| `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()` | Calculate descriptive metrics |
| `GROUP BY`, `ORDER BY` | Aggregate and rank categories |
| Subqueries | Calculate shares and overall denominators |
| `CASE WHEN` | Create review-rating segments |
| CTEs (`WITH`) | Break complex analysis into readable stages |
| `JOIN` | Combine gender totals with category-level results |
| `DENSE_RANK()` | Rank products within each category |
| `NTILE()` | Divide records into purchase-value tiers |
| Window functions (`OVER`, `PARTITION BY`) | Calculate within-group percentages and rankings |

## ✅ Project Conclusion

The analysis shows a customer base with a broad spread of demographic and purchasing patterns, but a clear concentration of purchase value in **Clothing and Accessories**. Young Adults form the strongest age-group segment by total purchase amount; Fall leads overall seasonal purchase value; and the subscription-discount relationship is especially notable because every subscribed record has a discount applied.

The most actionable opportunities are to protect performance in the leading categories, investigate the sizeable Average and Below Average satisfaction segments, and validate whether subscription-linked discounts improve retention or profitability using richer transaction history.

<div align="center">

![Status](https://img.shields.io/badge/SQL%20EDA-ANALYSIS%20COMPLETE-0F766E?style=for-the-badge)
![Prepared with](https://img.shields.io/badge/PREPARED%20WITH-PYTHON%20%7C%20PANDAS%20%7C%20MYSQL-0F172A?style=for-the-badge)

**Turning customer data into clearer business decisions.**

</div>
