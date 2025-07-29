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

SELECT order_id, customer_id, pizza_id,
	     CASE WHEN exclusions = '' OR exclusions = 'null' THEN NULL
		   ELSE exclusions END AS exclusions,
		   CASE WHEN extras = '' OR extras = 'null' THEN NULL
		   ELSE extras END AS extras,
		   order_time
FROM customer_orders;

SELECT * FROM temp_customer_orders;
```
**Result Set:***

| order_id | customer_id | pizza_id | exclusions | extras | order_time              |
|----------|-------------|----------|------------|--------|--------------------------|
| 1        | 101         | 1        | null       | null   | 2020-01-01T18:05:02.000Z |
| 2        | 101         | 1        | null       | null   | 2020-01-01T19:00:52.000Z |
| 3        | 102         | 1        | null       | null   | 2020-01-02T23:51:23.000Z |
| 3        | 102         | 2        | null       | null   | 2020-01-02T23:51:23.000Z |
| 4        | 103         | 1        | 4          | null   | 2020-01-04T13:23:46.000Z |
| 4        | 103         | 1        | 4          | null   | 2020-01-04T13:23:46.000Z |
| 4        | 103         | 2        | 4          | null   | 2020-01-04T13:23:46.000Z |
| 5        | 104         | 1        | null       | 1      | 2020-01-08T21:00:29.000Z |
| 6        | 101         | 2        | null       | null   | 2020-01-08T21:03:13.000Z |
| 7        | 105         | 2        | null       | 1      | 2020-01-09T23:54:33.000Z |
| 8        | 102         | 1        | null       | null   | 2020-01-11T12:25:09.000Z |
| 9        | 103         | 1        | 4          | 1,5    | 2020-01-11T12:25:09.000Z |
| 10       | 104         | 1        | null       | null   | 2020-01-11T18:34:49.000Z |
| 10       | 104         | 1        | 2,6        | 1,4    | 2020-01-11T18:34:49.000Z |

---

### 🧾 Table 2: `runner_orders`

The `runner_orders` table contains important delivery data but includes messy values that must be cleaned before analysis.

#### Result set before removing null values:

| order_id | runner_id | pickup_time         | distance   | duration     | cancellation             |
|----------|-----------|---------------------|------------|--------------|--------------------------|
| 1        | 1         | 2020-01-01 18:15:34 | 20km       | 32 minutes   |                          |
| 2        | 1         | 2020-01-01 19:10:54 | 20km       | 27 minutes   |                          |
| 3        | 1         | 2020-01-03 00:12:37 | 13.4km     | 20 mins      | [null]                   |
| 4        | 2         | 2020-01-04 13:53:03 | 23.4       | 40           | [null]                   |
| 5        | 3         | 2020-01-08 21:10:57 | 10         | 15           | [null]                   |
| 6        | 3         | null                | null       | null         | Restaurant Cancellation  |
| 7        | 2         | 2020-01-08 21:30:45 | 25km       | 25mins       | null                     |
| 8        | 2         | 2020-01-10 00:15:02 | 23.4 km    | 15 minute    | null                     |
| 9        | 2         | null                | null       | null         | Customer Cancellation    |
| 10       | 1         | 2020-01-11 18:50:20 | 10km       | 10minutes    | null                     |

Key issues:

- The `pickup_time` column includes `"null"` strings that should be treated as `NULL`.
- The `distance` column includes `"null"` and trailing units like `"km"` that must be removed.
- The `duration` column also includes `"null"` and contains various non-standard formats like `"mins"`, `"minutes"`, and `"minute"`.
- The `cancellation` column has both blank values and `"null"` strings.

---

### Cleaning Strategy

To prepare the data, we will:

- Create a temporary table with cleaned values.
- Normalize nulls and remove unwanted text from `distance`, `duration`, and `cancellation`.

```sql
DROP TABLE IF EXISTS temp_runner_orders;
CREATE TEMPORARY TABLE temp_runner_orders AS

SELECT order_id, runner_id,
	   CASE WHEN pickup_time = 'null' THEN NULL
	   	    ELSE pickup_time END AS pickup_time,
  	    
       CASE	WHEN distance ~ '^[0-9]+(\.[0-9]+)?\s*km$' THEN ROUND(CAST(REGEXP_REPLACE(distance, '\s*km', '', 'gi') AS NUMERIC), 1)::TEXT
    		WHEN distance ~ '^[0-9]+(\.[0-9]+)?$' THEN ROUND(CAST(distance AS NUMERIC), 1)::TEXT
    		ELSE NULL
  			END AS distance_km,
	    
       CASE WHEN duration = 'null' THEN NULL
    	    ELSE REGEXP_REPLACE(duration, '[^0-9]', '', 'g')
  		    END AS duration_mins,

	   CASE WHEN cancellation = '' OR cancellation = 'null' THEN NULL
	   		ELSE cancellation
	   		END AS cancellation
FROM runner_orders;

SELECT * FROM temp_runner_orders;
```

**Result Set:**

| order_id | runner_id |     pickup_time      | distance_km | duration_mins |      cancellation       |
|----------|-----------|----------------------|-------------|----------------|--------------------------|
|    1     |     1     | 2020-01-01 18:15:34  |    20.0     |       32       |          NULL           |
|    2     |     1     | 2020-01-01 19:10:54  |    20.0     |       27       |          NULL           |
|    3     |     1     | 2020-01-03 00:12:37  |    13.4     |       20       |          NULL           |
|    4     |     2     | 2020-01-04 13:53:03  |    23.4     |       40       |          NULL           |
|    5     |     3     | 2020-01-08 21:10:57  |    10.0     |       15       |          NULL           |
|    6     |     3     |         NULL         |    NULL     |      NULL      | Restaurant Cancellation |
|    7     |     2     | 2020-01-08 21:30:45  |    25.0     |       25       |          NULL           |
|    8     |     2     | 2020-01-10 00:15:02  |    23.4     |       15       |          NULL           |
|    9     |     2     |         NULL         |    NULL     |      NULL      | Customer Cancellation   |
|   10     |     1     | 2020-01-11 18:50:20  |    10.0     |       10       |          NULL           |

---
