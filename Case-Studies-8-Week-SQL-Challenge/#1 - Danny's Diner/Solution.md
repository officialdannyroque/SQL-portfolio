# 🍣 Danny's Diner – Case Study Solutions

Welcome to the solution write-up for the **Danny’s Diner** case study. This project explores customer behavior using SQL queries to extract insights from sales data.

We’ll walk through each question with the associated query and result set.

---

## Case Study Questions

1. What is the total amount each customer spent at the restaurant?
2. How many days has each customer visited the restaurant?
3. What was the first item from the menu purchased by each customer?
4. What is the most purchased item on the menu and how many times was it purchased by all customers?
5. Which item was the most popular for each customer?
6. Which item was purchased first by the customer after they became a member?
7. Which item was purchased just before the customer became a member?
8. What is the total items and amount spent for each member before they became a member?
9. If each $1 spent = 10 points and sushi has a 2x multiplier, how many points would each customer have?
10. In the first week after a customer joins, they earn 2x points on all items. How many points do customers A and B have at the end of January?

---

## ✅ Solutions

### 1. What is the total amount each customer spent at the restaurant?

```sql
WITH product_spend AS (SELECT customer_id, s.product_id, price * COUNT(s.product_id) AS spend
		       FROM sales s
		       INNER JOIN menu m
		       ON s.product_id = m.product_id
		       GROUP BY customer_id, s.product_id, price)

SELECT customer_id, SUM(spend) AS total_spend
FROM product_spend
GROUP BY customer_id
ORDER BY customer_id;
```

**Result set:**

| customer_id | total_sales |
|-------------|-------------|
| A           | 76          |
| B           | 74          |
| C           | 36          |

---

### 2. How many days has each customer visited the restaurant?

```sql
SELECT customer_id, COUNT(DISTINCT order_date) AS num_visits
FROM sales
GROUP BY customer_id;
```

**Result set:**

| customer_id | num_visits  |
|-------------|-------------|
| A           | 4           |
| B           | 6           |
| C           | 2           |

---

### 3. What was the first item from the menu purchased by each customer?

```sql
WITH ranked_orders AS (SELECT customer_id, product_id, order_date,
			      RANK() OVER(PARTITION BY customer_id ORDER BY order_date) AS earliest_order_rank
			      FROM sales)

SELECT 		r.customer_id, m.product_name
FROM   		ranked_orders r
INNER JOIN  	menu m ON r.product_id = m.product_id
WHERE		r.earliest_order_rank = 1
ORDER BY 	customer_id;
```

**Result set:**

| customer_id | product_name |
|-------------|--------------|
| A           | curry        |
| A           | sushi        |
| B           | curry        |
| C           | ramen        |

---

### 4. What is the most purchased item on the menu and how many times was it purchased by all customers? 

```sql
WITH product_count AS (SELECT product_id, COUNT(*) AS num_purchased
		       FROM sales
		       GROUP BY product_id)

SELECT 	   m.product_name, num_purchased
FROM 	   product_count p
INNER JOIN menu m ON p.product_id = m.product_id
ORDER BY   num_purchased DESC
LIMIT 1;
```

**Result set:**

| product_name | num_purchased  |
|--------------|----------------|
| ramen        | 8              |

---

### 5. Which item was the most popular for each customer?

```sql
WITH cust_product_count AS (SELECT   customer_id, product_id, COUNT(*) AS order_count
			    FROM     sales
			    GROUP BY customer_id, product_id),

cust_product_rank AS (SELECT c.customer_id, m.product_name, c.order_count,
			     RANK() OVER(PARTITION BY c.customer_id ORDER BY c.order_count DESC) AS oc_rank
			     FROM cust_product_count c
			     INNER JOIN menu m
			     ON c.product_id = m.product_id)

SELECT customer_id, product_name, order_count
FROM   cust_product_rank
WHERE  oc_rank = 1;
```

**Result set:**

| customer_id | product_name | order_count |
|-------------|--------------|-------------|
| A           | ramen        | 3           |
| B           | curry        | 2           |
| B           | sushi        | 2           |
| B           | ramen        | 2           |
| C           | ramen        | 3           |

---

### 6. Which item was purchased first by the customer after they became a member?

```sql
WITH member_orders AS (SELECT     mem.customer_id, m.product_name, mem.join_date, s.order_date,
			          RANK() OVER(PARTITION BY mem.customer_id ORDER BY order_date)
		       FROM       members mem
		       INNER JOIN sales s ON s.customer_id = mem.customer_id
		       LEFT JOIN  menu m ON m.product_id = s.product_id
		       WHERE      order_date >= join_date)

SELECT customer_id, product_name, join_date, order_date
FROM   member_orders
WHERE  rank = 1;
```

**Result set:**

| customer_id | product_name | join_date  | order_date |
|-------------|--------------|------------|------------|
| A           | curry        | 2021-01-07 | 2021-01-07 |
| B           | sushi        | 2021-01-09 | 2021-01-11 |

---

### 7. Which item was purchased just before the customer became a member?

```sql
WITH member_orders AS (SELECT mem.customer_id, m.product_name, mem.join_date, s.order_date,
		RANK() OVER(PARTITION BY mem.customer_id ORDER BY order_date DESC)
FROM members mem
INNER JOIN sales s ON s.customer_id = mem.customer_id
LEFT JOIN menu m ON m.product_id = s.product_id
WHERE order_date < join_date)

SELECT customer_id, product_name, join_date, order_date
FROM member_orders
WHERE rank = 1;
```

**Result set:**

| customer_id | product_name | join_date  | order_date |
|-------------|--------------|------------|------------|
| A           | sushi        | 2021-01-07 | 2021-01-01 |
| A           | curry        | 2021-01-07 | 2021-01-01 |
| B           | sushi        | 2021-01-09 | 2021-01-04 |

---

### 7. Which item was purchased just before the customer became a member?

```sql
WITH member_orders AS (SELECT mem.customer_id, m.product_name, mem.join_date, s.order_date,
		RANK() OVER(PARTITION BY mem.customer_id ORDER BY order_date DESC)
FROM members mem
INNER JOIN sales s ON s.customer_id = mem.customer_id
LEFT JOIN menu m ON m.product_id = s.product_id
WHERE order_date < join_date)

SELECT customer_id, product_name, join_date, order_date
FROM member_orders
WHERE rank = 1;
```

**Result set:**

| customer_id | product_name | join_date  | order_date |
|-------------|--------------|------------|------------|
| A           | sushi        | 2021-01-07 | 2021-01-01 |
| A           | curry        | 2021-01-07 | 2021-01-01 |
| B           | sushi        | 2021-01-09 | 2021-01-04 |

---

### 7. Which item was purchased just before the customer became a member?

```sql
WITH member_orders AS (SELECT mem.customer_id, m.product_name, mem.join_date, s.order_date,
		RANK() OVER(PARTITION BY mem.customer_id ORDER BY order_date DESC)
FROM members mem
INNER JOIN sales s ON s.customer_id = mem.customer_id
LEFT JOIN menu m ON m.product_id = s.product_id
WHERE order_date < join_date)

SELECT customer_id, product_name, join_date, order_date
FROM member_orders
WHERE rank = 1;
```

**Result set:**

| customer_id | product_name | join_date  | order_date |
|-------------|--------------|------------|------------|
| A           | sushi        | 2021-01-07 | 2021-01-01 |
| A           | curry        | 2021-01-07 | 2021-01-01 |
| B           | sushi        | 2021-01-09 | 2021-01-04 |

---

### 8. What is the total items and amount spent for each member before they became a member?

```sql
SELECT 	   mem.customer_id, COUNT(s.product_id) AS num_products, SUM(price) AS total_spend
FROM 	   members mem
INNER JOIN sales s ON s.customer_id = mem.customer_id
INNER JOIN menu m ON m.product_id = s.product_id
WHERE 	   order_date < join_date
GROUP BY   mem.customer_id
ORDER BY   customer_id;
```

**Result set:**

| customer_id | num_products | total_spend  |
|-------------|--------------|--------------|
| A           | 2            | 25           |
| B           | 3            | 40           |

---

### 9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier. How many points would each customer have?

```sql
SELECT s.customer_id,
       SUM(CASE WHEN m.product_name = 'sushi' THEN price * 20
	   ELSE price * 10 END) AS total_points
FROM 	   sales s
INNER JOIN menu m
ON 	   s.product_id = m.product_id
GROUP BY   s.customer_id
ORDER BY   s.customer_id;
```

**Result set:**

| customer_id | total_points |
|-------------|--------------|
| A           | 860          |
| B           | 940          |
| C           | 360          |

---

### 10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi. How many points do customer A and B have at the end of January

```sql
WITH point_program AS (SELECT s.customer_id, order_date, mem.join_date, (mem.join_date + INTERVAL '6 days')::DATE AS program_end, m.product_name, m.price
			      FROM sales s
			      INNER JOIN members mem ON s.customer_id = mem.customer_id
			      LEFT JOIN menu m ON m.product_id = s.product_id)

SELECT   customer_id, 
	 SUM(CASE WHEN order_date BETWEEN join_date AND program_end THEN 20 * price
		  WHEN order_date NOT BETWEEN join_date AND program_end AND product_name = 'sushi' THEN 20 * price
		  ELSE 10 * price END) AS total_points
FROM     point_program
WHERE    order_date < '2021-02-01' AND order_date >= join_date
GROUP BY customer_id
ORDER BY customer_id;
```

**Result set:**

| customer_id | total_points |
|-------------|--------------|
| A           | 1020         |
| B           | 320          |
