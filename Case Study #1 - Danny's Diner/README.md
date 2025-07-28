# Case Study #1: Danny's Diner

![Danny's Diner ERD](https://github.com/user-attachments/assets/c1e2b31b-fbdb-4ccb-873f-2ee98b77743f) <!-- replace with your actual ERD image path if using -->

---

## 📚 Table of Contents

- [Introduction](#introduction)
- [Problem Statement](#-problem-statement)
- [Datasets Used](#-datasets-used)
- [Entity Relationship Diagram](#entity-relationship-diagram)
- [Case Study Questions + Bonus](#case-study-questions--bonus)
- [Relevant Links](#-relevant-links)
- [Contributing](#contributing)
- [Support](#support)

---

## Introduction

Danny is a die-hard fan of Japanese cuisine and in early 2021 decided to launch a cozy restaurant offering his three favorite dishes: **sushi**, **curry**, and **ramen**.

Although the food has heart, the business needs help, Danny has gathered some basic transactional data but lacks the skills to extract meaningful insights. That’s where this case study begins.

As part of my data analyst portfolio, this project demonstrates how SQL can be used to support business decisions in the restaurant industry.

---

## 🎯 Problem Statement

Danny is interested in understanding customer behavior to improve operations and customer retention. Specifically, he wants to uncover:

- How often customers visit
- What they order most frequently
- How much they’ve spent
- How effective the current loyalty program has been

With these insights, he hopes to enhance the dining experience and decide whether to expand his loyalty rewards system.

---

## 🧾 Datasets Used

Three key tables form the foundation of this case study:

- `sales`: Records each customer's order history including `customer_id`, `order_date`, and `product_id`
- `menu`: Maps each `product_id` to its `product_name` and `price`
- `members`: Tracks when each customer joined the loyalty program via `join_date`

---

## Entity Relationship Diagram

![Entity Relationship Diagram](https://github.com/user-attachments/assets/c621b967-ad68-43a3-a99b-d77699a195ae)  
*Diagram created via [dbdiagram.io](https://dbdiagram.io)*

---

## Case Study Questions + Bonus

1. What is the total amount each customer spent at the restaurant?  
2. How many days has each customer visited the restaurant?  
3. What was the first item from the menu purchased by each customer?  
4. What is the most purchased item on the menu and how many times was it purchased by all customers?  
5. Which item was the most popular for each customer?  
6. Which item was purchased first by the customer after they became a member?  
7. Which item was purchased just before the customer became a member?  
8. What is the total items and amount spent for each member before they became a member?  
9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?  
10. In the first week after a customer joins (including join date), they earn 2x points on all items, how many points do customers A and B have at the end of January?

### Bonus Questions

- Join All The Things  
- Rank All The Things

---

## 🔗 Relevant Links

- [Entity Relationship Diagram – dbdiagram.io](https://dbdiagram.io/d/Dannys-Diner-608d07e4b29a09603d12edbd?utm_source=dbdiagram_embed&utm_medium=bottom_open)
- [DB Fiddle SQL Schema & Playground](https://www.db-fiddle.com)

---

## Contributing

Contributions are always welcome!

If you’d like to suggest improvements or fix something, feel free to fork the repo and open a pull request.

---

## Support

Have questions or suggestions?  
You can reach me via [LinkedIn](https://linkedin.com/in/YOUR-NAME-HERE) or open an issue in this repository.

If this project helped you, inspired you, or gave you ideas of your own, please consider giving it a ⭐!
