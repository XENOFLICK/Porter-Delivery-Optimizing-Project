# Porter: Optimizing Delivery for the Digital Diner 📊🚚

## 📌 Project Overview
**Porter Delivery** is a fast-growing platform in the food delivery sector facing a critical business bottleneck: **plummeting customer satisfaction scores due to rising delivery times** across several key markets. 

This project aims to unpack the underlying bottlenecks in the food delivery supply chain, identify specific inefficiencies across restaurant categories and delivery markets, and deliver data-driven recommendations to stabilize operational performance and protect market share.

---

## 🎯 Problem Statement
The primary objective of this project is to analyze the complex web of operational and environmental factors influencing delivery times. By identifying high-variance markets, inefficient ordering protocols, and supply-demand mismatches, this analysis uncovers actionable strategies to **minimize delivery durations while protecting and improving overall service quality.**

---

## 👥 Stakeholders & Requirements

### Stakeholders
*   **Internal:** Operations Team, Marketing Department, Customer Service, and Logistics/Fleet Management.
*   **External:** Partner Restaurants and Delivery Fleet Personnel.

### Project Deliverables & Requirements
1.  **Metric Development:** Construct benchmarks for `average delivery time`, `on-time delivery rates (30-min window)`, and `peak performance ratios`.
2.  **Data Preprocessing:** Address missing values, fix schema type mismatches, and simplify high-cardinality features.
3.  **Feature Engineering:** Extract time-based dimensions (hour of day, day of week, total delivery duration) to model behavioral trends.
4.  **Actionable Insights:** Pinpoint systemic bottlenecks and build strategic recommendations for stakeholders.

---

## 💻 Tools & Technologies Used
*   **Programming Language:** Python 3.8+
*   **Data Manipulation:** Pandas, NumPy
*   **Data Visualization:** Matplotlib, Seaborn
*   **Statistical Analysis & Modeling:** SciPy (Correlation Analysis), Statsmodels / Scikit-Learn (Regression)
*   **Environment & Storage:** Jupyter Notebook, Git/GitHub

---

## 📂 Data Dictionary & Source
The underlying dataset is sourced from [Kaggle - Porter Delivery Time Estimation](https://kaggle.com). Each row represents a single unique delivery transaction.

| Feature Column | Data Type | Description |
| :--- | :--- | :--- |
| `market_id` | Integer | ID for the regional market where the restaurant operates. |
| `created_at` | Datetime (String) | Timestamp when the order was placed by the customer. |
| `actual_delivery_time` | Datetime (String) | Timestamp when the order was successfully marked delivered. |
| `store_primary_category` | Categorical | The primary cuisine/food category of the restaurant. |
| `order_protocol` | Integer (Code) | The method used to place the order (e.g., app, phone call, third-party). |
| `total_items` | Integer | Total count of individual items in the order. |
| `subtotal` *(total_items in raw text)* | Float | The final financial subtotal of the items in the order. |
| `num_distinct_items` | Integer | The count of unique/distinct SKU items within the order. |
| `min_item_price` / `max_item_price` | Float | The respective prices of the lowest and highest-cost items in the order. |
| `total_onshift_partners` | Integer | Number of active delivery personnel available at order creation. |
| `total_busy_partners` | Integer | Number of delivery personnel currently occupied with other tasks. |
| `total_outstanding_orders` | Integer | The current backlog of unfulfilled orders in that market area. |

---

## 🛠️ Data Preprocessing & Workflow
1.  **Missing Value Treatment:** Evaluated and handled structural missingness in key fields (`market_id`, `actual_delivery_time`, `store_primary_category`, and courier supply metrics) using context-aware imputation or targeted drops.
2.  **Temporal Type Casting:** Converted string timestamps (`created_at`, `actual_delivery_time`) to correct `datetime64` types.
3.  **Feature Engineering:** 
    *   `delivery_duration_seconds` = `actual_delivery_time` - `created_at`
    *   `order_hour` & `order_day_of_week` extracted from creation timestamps.
    *   `busy_to_onshift_ratio` to flag real-time partner strain.
4.  **Categorical Simplification:** Grouped low-frequency values in `store_primary_category` into an `"Other"` bucket to minimize noise and improve model stability.

---

## 💡 Key Insights & Findings
*   **Supply & Demand Mismatches:** Peak ordering hours exhibit sharp delivery time spikes not just because of order volume, but due to highly unoptimized active-to-busy courier ratios (`total_busy_partners` vs `total_onshift_partners`).
*   **High-Variance Categories:** Specific high-prep cuisine types under `store_primary_category` systematically delay couriers at the point of pickup, driving outliers in total delivery durations.
*   **Operational Inefficiencies:** Delivery times vary significantly by `order_protocol`. Digital app protocols outperform traditional legacy protocols, indicating a need to transition partners away from slow ingestion methods.
*   **High-Risk Markets:** Certain `market_id` clusters exhibit high standard deviations in delivery times, indicating unstable, non-standardized regional logistics ecosystems.

---

## ⚠️ Challenges Faced
*   **Data Integrity & Null Values:** Dealing with massive missing data windows in real-time courier tracking metrics (`total_onshift_partners`) without introducing synthetic bias to regression workflows.
*   **Outlier Distortions:** Extreme delivery durations skewed standard mathematical means. Employing strict statistical filtering (e.g., IQR methods) was critical to stabilizing performance views.
*   **High Cardinality:** Managing hundreds of distinct restaurant classifications required meticulous feature aggregation to maintain clean visualizations and robust operational buckets.

---

## 🚀 Strategic Recommendations
Based on the analysis, the following structural improvements are proposed for Porter Delivery:

1.  **Dynamic Courier Dispatching:** Implement a predictive driver-allocation framework that automatically scales up `total_onshift_partners` roughly 30 to 45 minutes ahead of identified historical peak hour windows.
2.  **SLA Differentiation by Store Prep Complexity:** Standardize variable delivery expectations on the customer app based on `store_primary_category` preparation speeds instead of a flat 30-minute benchmark.
3.  **App Protocol Mandates:** Phase out legacy, manual order-entry protocols in favor of integrated API configurations to reduce early-stage order stagnation.
4.  **Geographic Fleet Balancing:** Move excess idle courier capacity out of low-variance markets into high-variance, struggling `market_id` clusters during low-demand periods.

---
