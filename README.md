# Association Rule Mining — B2B Eyewear Transactions

### Uncovering Purchasing Trends Through Market Basket Analysis

## Description

This project applies association rule mining to a large dataset of B2B eyewear transactions. The customers in this dataset are independent optometry offices and optical shops that purchase product either through a field sales representative or an online B2B ordering portal.

The goal is to identify purchasing trends by finding which combinations of items are frequently bought together, and to evaluate how those results can be used to provide tactical selling recommendations to a sales team and power "Amazon-style" product recommendations in an online ordering portal.

## Key Questions Answered

* Which SKUs and brands appear most frequently in transactions?
* Which combinations of items are most frequently purchased together?
* How strong are the association rules between frequent item sets?
* Do brands tend to sell in silos or do they mix in purchasing patterns?
* Can association rules validate existing selling recommendations?

## Tech Stack

* **Language:** Python
* **Framework:** PySpark
* **Algorithm:** FP Growth (Frequent Pattern Growth)
* **Environment:** Jupyter Notebook
* **Libraries:**

  * `PySpark` — distributed data processing and FP Growth implementation
  * `pandas` — data manipulation
  * `matplotlib` — data visualization and histograms

## Data Set Description

* **Format:** CSV files
* **Size:** \~50,000 rows, \~12,000 transactions (\~10,000 with positive value)
* **Scope:** Sales transactions for 2024 within a subset customer group of \~500 doors
* **Transaction data includes:** Product specific codes (SKUs), brand names, order numbers, sales dollar values, color codes, sizes, and state-level geographic data
* **Brands covered:** Brand 1, Brand 2, Brand 3, Brand 4, Brand 5, Brand 6

## Methods

### Data Cleaning

* Extracted color codes from product specific codes using the `substring` function
* Filtered out rows with missing color or size data (indicating discontinued frames)
* Removed transactions with sales values under $1 (returns and promotions)
* Removed discontinued or non-applicable brand rows using `.filter()`

### Data Exploration

* Performed `GroupBy` on brand name to find total units sold per brand
* Analyzed transaction distribution — found most transactions are small (6 units or less)
* Explored geographic distribution by state to identify highest-volume markets

### FP Growth Implementation

* Grouped data by order number and aggregated material codes into item lists
* Applied FP Growth model with tuned `minSupport` and `minConfidence` parameters:

  * **minSupport = 0.005** — SKU must appear in at least 50 of 10,000 transactions
  * **minConfidence = 0.40** — antecedent must lead to consequent in at least 40% of transactions
* Re-ran analysis on subsets with dominant brands removed to surface trends across remaining brands

## Key Metrics Explained

* **Support** — proportion of transactions containing a specific item or itemset
* **Confidence** — likelihood that the consequent appears given the antecedent
* **Lift** — strength of association; values greater than 1 indicate a positive relationship

## Results Summary

* **Brand 1** dominated the initial results, representing the top-selling brand by a significant margin
* Association rules showed confidence ranges of **40%–50%**, with all Lift values well above 1, indicating strong positive purchasing relationships
* **23 out of 30** top frequent item sets included top-performing SKUs, validating that sales representatives are effectively recommending high-priority frames
* After removing Brand 1 rows, **Brand 2** emerged as the next most prevalent, followed by **Brand 3**
* After further removing Brand 2, **Brand 6** surfaced with the strongest individual association rule — a confidence of **75%** and a Lift of **52.79**, indicating a very strong purchasing relationship
* Brands generally sold within silos — association rules rarely mixed SKUs across different brands

## Conclusion

Association rule mining successfully confirmed observed brand trends while uncovering deeper recommendation patterns. The strong Lift values across all rules indicate that the purchasing associations identified are meaningful and actionable. Next steps include:

1. Reproducing the analysis across different customer subsets to validate or expand findings
2. Providing the sales team with data-driven SKU pairing recommendations
3. Implementing "Amazon-style" recommendations in the B2B online ordering portal — surfacing frequently co-purchased items when a specific frame is added to the cart

## Author

Juan Delgado — Syracuse University, IST 718 (Big Data Analytics), March 2025

