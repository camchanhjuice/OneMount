Link to the presentation: https://docs.google.com/presentation/d/123XNDEyY5EULtGTBQ0Qlz6yC7X1aO4m8i8Kkn22M6I8/edit?usp=sharing

# FINAL METHODOLOGY LAYOUT

### Problem Statement
The objective of this analysis is to evaluate the performance of an e-commerce platform's voucher system. This involves understanding how vouchers operate, identifying which categories and vouchers are most effective, and determining the best voucher types for different customer segments.

### Data Overview
The dataset is composed of five interconnected tables:

- **fact**: Captures user interactions with vouchers, including claims and redemptions.
- **scheme**: Details promotion schemes associated with each voucher, including discount types and amounts.
- **voucher**: Contains information about the inventory of vouchers, including their availability.
- **merchant**: Provides details about the categories and subcategories of the merchants offering vouchers.
- **user**: Contains demographic information about users, such as gender, age, and location.

### Data Issues and Assumptions
* **SERIAL_NUMBER**: The purpose of this column is unclear and might be redundant.
* **VOUCHER_NAME**: Multiple voucher codes can share the same name, which might introduce ambiguity.
* **CALENDAR_DIM_ID**: This column is not defined and its purpose is unclear.
* **discount_type**: The specific types of discounts are not detailed.
* **discount_anunt** and **discount_percent**: It's unclear how these columns relate to each other and which one is applicable in different scenarios.

## Analysis Objectives and Metrics

### Objective 1: Assess Voucher Operation
**Questions:**
1. How many vouchers are currently active?
2. is the redemption rate of vouchers?
3. What are the overall claim and redemption rates for vouchers?
4. How do these rates vary over time?
5. What is the average discount of redeemed vouchers, and how does it influence redemption rates?
6. Does the validity period of vouchers affect their claim and redemption rates?

**Metrics:**
- Total vouchers issued, claimed, and redeemed.
- Number of active/expired vouchers (calculated from `voucher` table using `display_date_from` and `display_date_to`)
- Claim rate = (Total claimed vouchers / Total issued vouchers) * 100.
- Redemption rate = (Total redeemed vouchers / Total claimed vouchers) * 100.
- Average discount amount and percentage = Sum of `discount_amount` or `discount_percent` from the `scheme` table for redeemed vouchers.
- Average validity period of vouchers.
- Claim and redemption rates segmented by time (daily, weekly, monthly).

**Combinations of Features:**
- Join `fact`, `voucher`, and `scheme` tables using `VOUCHER_CODE`.
- Analyze voucher usage across demographics by incorporating the `user` table.

**Approach:**
- Aggregate the number of claimed and redeemed vouchers from the `fact` table.
- Calculate the claim-to-redemption ratio by dividing the total number of redeemed vouchers by the total number of claimed vouchers.
- Determine active and expired vouchers using the `voucher` table's `display_date_to` and `available_to` columns.
- Use voucher usage rate to understand the effectiveness of the vouchers.

### Objective 2: Identify High-Performing Categories and Vouchers
**Questions:**
1. Which merchant categories and subcategories exhibit the highest claim and redemption rates?
2. What is the average discount offered per category?
3. What types of vouchers (by discount type and amount) are most effective?
4. How do various voucher schemes (e.g., for new users, based on purchase amount) perform?

**Metrics:**
- Claim and redemption rates/number by category and subcategory.
- Average discount amount and percentage by category and subcategory.
- Voucher performance metrics by discount type and amount.

**Combinations of Features:**
- Join the `fact`, `merchant`, `voucher`, and `scheme` tables.
- Analyze voucher performance by category and subcategory based on the `merchant` table's `category` and `sub-category` columns.

**Approach:**
- Aggregate the number of claimed and redeemed vouchers by `category` and `sub-category`.
- Calculate the redemption rate by dividing the number of redeemed vouchers by the number of claimed vouchers for each category and subcategory.
- Calculate the average discount value using `discount_amount` and `discount_percent` from the `scheme` table.
- Estimate revenue generated from redeemed vouchers using the discount values and the number of redemptions.

### Objective 3: Customer Segmentation and Voucher Targeting
**Questions:**
1. What are the key customer segments based on demographics and behavior?
2. Which voucher types are favored by different customer segments?
3. What is the impact of targeted voucher campaigns on customer behavior?
4. How do targeted voucher campaigns influence customer behavior?

**Metrics:**
- Customer segments based on demographics (age, gender, location) and behavior (purchase frequency, average order value, redemption rates).
- Average discount value redeemed by each segment.
- Voucher claim and redemption rates by customer segment.
- Impact of targeted campaigns on sales and customer retention.
- Preferred categories and subcategories by each customer group
- Lift in customer retention for targeted voucher campaigns

**Combinations of Features:**
- Join the `fact`, `user`, `merchant`, and `scheme` tables to analyze customer preferences for different vouchers.
- Group customers by demographics using the `user` table's `age`, `gender`, and `province_name` columns.

### Example Feature Combinations for Analysis: *
* **Voucher performance analysis:**
  * Voucher code, voucher name, discount type, discount amount, discount percent, available from, available to, claim count, redeem count, redemption rate, claim rate
* **Category performance analysis:**
  * Merchant code, category, subcategory, voucher code, claim count, redeem count, redemption rate, claim rate, average discount
* **Customer segmentation:**
  * User ID, gender, age, province, purchase history, voucher claim/redeem history

**Approach:**
- Perform customer segmentation based on demographics.
- Aggregate the number of claimed and redeemed vouchers for each customer segment.
- Calculate the redemption rate for each customer segment.
- Identify preferred categories and subcategories by analyzing the number of vouchers claimed and redeemed by each customer group.

## Additional Insights and Potential Questions
1. **Seasonal trend/Peak Times for Voucher Claims and Redemptions:**
   - Analyzing redemption rates over time can reveal seasonal patterns.
   - Analyze `CALENDAR_DIM_ID` and `ACTION` columns in the `fact` table.
   - Metrics: Claims and redemptions by time of day and day of the week.

2. **Voucher Performance by Province:**
   - Utilize the `province_name` column in the `user` table.
   - Metrics: Claims and redemptions by province, redemption rates by province.

3. **Effectiveness of Different Discount Types/How do different discount types affect redemption rates?:**
   - Comparing percentage discounts vs. fixed amount discounts can yield insights into customer preferences.
   - Investigate `discount_type`, `discount_amount`, and `discount_percent` in the `scheme` table.
   - Metrics: Redemptions by discount type, average redemption rates by type.

4. **Impact of Competition and Voucher Fraud:**
   - Explore external factors and potential fraudulent activities affecting voucher performance.

5. **Effectiveness of Distribution Channels:**
   - Metrics: Compare performance of vouchers distributed via app notifications, email, and social media.

6. **What is the impact of promotional campaigns on voucher redemption?**
   - Evaluating the effectiveness of different promotional schemes can help refine marketing strategies.

## Dashboard Design for Stakeholders

To effectively communicate insights from the analysis, the dashboard should include the following metrics and visualizations:

### **Key Metrics to Display:**

- **Operational Status:**
  - Total number of vouchers claimed and redeemed
  - Effectiveness of different discount types.
  - Total active/expired vouchers 
  - Redemption rate (percentage)
  - Average discount value of redeemed vouchers
  - Total number of vouchers claimed and redeemed.
  - Claim-to-redemption ratio.
  - Voucher usage rate.
  - Voucher performance by province.

- **Performance by Category:**
  - Claims and redemptions by category and subcategory.
  - Redemption rates by category and subcategory (bar chart)
  - Average discount per category/subcategory (line chart)
  - Revenue generated from redeemed vouchers by category and subcategory.

- **Customer Segmentation:**
  - Total vouchers redeemed by demographic segments (pie chart)
  - Average discount value redeemed by segment (bar chart)
  - Preferred categories and subcategories by customer group.
  - Customer segmentation metrics.


