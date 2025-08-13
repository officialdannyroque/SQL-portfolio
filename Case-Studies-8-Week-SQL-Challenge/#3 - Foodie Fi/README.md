## [Case Study 3: Foodie-Fi](https://8weeksqlchallenge.com/case-study-3/)  

<p align="center">
  <img src="https://8weeksqlchallenge.com/images/case-study-designs/3.png" alt="Foodie-Fi Logo" width="450" height="450">
</p>

---

## Table of Contents  
- [Introduction](#introduction)  
- [Dataset](#dataset)  
- [Entity Relationship Diagram](#entity-relationship-diagram)  
- [Solutions](#solutions)  
- [Contributing](#contributing)  
- [Support](#support)  

---

## Introduction  
Foodie-Fi is a **subscription-based video streaming service** that launched in 2020, specializing exclusively in **food-related content**. Think *Netflix*, but only for cooking shows.  

The business model: offer **monthly and annual subscriptions** with unlimited on-demand access to global culinary content.  

This case study uses **SQL data analysis** to answer important business questions, such as:  
- Understanding **customer lifecycle and churn patterns**  
- Analyzing **subscription plan performance**  
- Identifying **revenue optimization opportunities**  

This project was completed as part of my SQL portfolio to **demonstrate practical business analytics skills** with real-world subscription-style datasets.  

---

## Dataset  

### 1. **`plans`**  
Details about Foodie-Fi's subscription plans:  

| Plan Name      | Description                                                                                  | Price      | Billing Cycle |
|----------------|----------------------------------------------------------------------------------------------|------------|---------------|
| Basic          | Limited access, streaming only                                                               | $9.90      | Monthly       |
| Pro            | No watch time limits, offline downloads                                                      | $19.90     | Monthly       |
| Pro Annual     | Same as Pro, billed annually                                                                 | $199.00    | Annual        |
| Trial          | 7-day free trial → auto-converts to Pro Monthly unless cancelled/downgraded/upgraded early   | Free       | 7 days        |
| Churn          | Null price, marks the end of service, access remains until end of billing period             | N/A        | N/A           |

---

### 2. **`subscriptions`**  
Tracks the exact **start date** of a customer's subscription plan and changes over time.  

- **Upgrades** take effect immediately.  
- **Downgrades/cancellations** take effect at the end of the billing period.  
- **Churned customers** retain access until the end of the current billing cycle.  

---

## Entity Relationship Diagram  

![ERD](https://github.com/faizanxmulla/sql-portfolio/assets/71728480/8267170d-39fc-4907-8425-ed6c185f14be)  

---

## Solutions  

- [1. Customer Journey](1.%20Customer-Journey.md)  
- [2. Data Analysis](2.%20Data-Analysis.md)  
- [3. Challenge Payment Question](3.%20Challenge-Payment.md)  
- [4. Outside the Box Questions](4.%20Outside-the-Box-Questions.md)  

---

## Contributing  
Contributions are welcome!  
1. Fork the repository  
2. Create a new branch for your feature or fix  
3. Submit a pull request  

---

## Support  
If you have questions or suggestions, feel free to open an issue or reach out.  


If you have any doubts, queries or, suggestions then, please connect with me on [LinkedIn](https://www.linkedin.com/in/faizanxmulla/).

Do ⭐ the repository, if it inspired you, gave you ideas of your own or helped you in any way !!
