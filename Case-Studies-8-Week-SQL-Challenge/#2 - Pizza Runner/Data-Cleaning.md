## 🧹 Data Cleaning & Transformation

---

### 🧾 Table 1: `customer_orders`

Upon observing the `customer_orders` table, we can see:

- Missing values `''` and `'null'` in the `exclusions` column.
- Missing values `''` and `'null'` in the `extras` column.

#### 📊 Result set before removing null values:

| order_id | customer_id | pizza_id | exclusions | extras | order_time              |
|----------|-------------|----------|------------|--------|--------------------------|
| 1        | 101         | 1        |            |        | 2020-01-01T18:05:02.000Z |
| 2        | 101         | 1        |            |        | 2020-01-01T19:00:52.000Z |
| 3        | 102         | 1        |            |        | 2020-01-02T23:51:23.000Z |
| 3        | 102         | 2        |            | null   | 2020-01-02T23:51:23.000Z |
| 4        | 103         | 1        | 4          |        | 2020-01-04T13:23:46.000Z |
| 4        | 103         | 1        | 4          |        | 2020-01-04T13:23:46.000Z |
| 4        | 103         | 2        | 4          |        | 2020-01-04T13:23:46.000Z |
| 5        | 104         | 1        | null       | 1      | 2020-01-08T21:00:29.000Z |
| 6        | 101         | 2        | null       | null   | 2020-01-08T21:03:13.000Z |
| 7        | 105         | 2        | null       | 1      | 2020-01-09T23:54:33.000Z |
| 8        | 102         | 1        | null       | null   | 2020-01-11T12:25:09.000Z |
| 9        | 103         | 1        | 4          | 1,5    | 2020-01-11T12:25:09.000Z |
| 10       | 104         | 1        | null       | null   | 2020-01-11T18:34:49.000Z |
| 10       | 104         | 1        | 2,6        | 1,4    | 2020-01-11T18:34:49.000Z |

---

To clean this data, we will:

- ✅ Create a temporary table with all the columns.
- ✅ Replace `''` and `'null'` values in `exclusions` and `extras` with actual `NULL`.

```sql
CREATE TEMP TABLE temp_customer_orders AS
SELECT
  order_id,
  customer_id,
  pizza_id,
  CASE
    WHEN exclusions = '' OR exclusions = 'null' THEN NULL
    ELSE exclusions
  END AS exclusions,
  CASE
    WHEN extras = '' OR extras = 'null' THEN NULL
    ELSE extras
  END AS extras,
  order_time
FROM
  pizza_runner.customer_orders;

SELECT * 
FROM temp_customer_orders;
```
