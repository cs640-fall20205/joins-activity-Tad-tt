# Postgres JOINS (Customer & Orders Example with PK/FK)

## Learning Objectives

Able to...

1. Understand INNER, LEFT, RIGHT, and FULL JOINS in PostgreSQL

---

## Model 1

**Customers**

| customer_id | customer_name |
| ----------- | ------------- |
| c1          | Alice         |
| c2          | Bob           |
| c3          | Carol         |
| c4          | David         |

**Orders**

| order_id | customer_id |
| -------- | ----------- |
| o1       | c1          |
| o2       | c2          |
| o5       | c5          |
| o6       | c6          |

Questions

1. Which customers have matching orders?

   c(1) and c(2)

2. Which customers do not have any orders?

   c(3) and c(4)

3. Which orders do not match any existing customers?

   o(5) and o(6)


---

## Model 2

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
INNER JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1	        | Alice	        | o1       |
| c2	        | Bob	          | o2       |

Questions

1. Which customers are not included in the result?

   c(3) and c(4)

2. Why do you think they are not in the result?

   No matching orders to connects both table

3. Which orders are not included in the result?

   o(5) and o(6)

4. Why do you think they are not in the result?

   Because c(5) and c(6) dosen't exist in customer table to match it

5. When is a row from Customers or Orders included?

   When c.customer_id = o.customer_id matches

6. What is the meaning of INNER JOIN?

   Between two table count/return only matching rows under conditions


---

## Model 3

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
LEFT OUTER JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1          | Alice	        | o1       |
| c2	        | Bob	          | o2       |
| c3	        | Carol	        |          |
| c4	        | David	        |          |

Questions

1. Which customers are not included in the result?

   All of them are included

2. Which orders are not included in the result?

   o(5) and o(6)

3. When is a row included?

   All row from the left table (customer) are included

4. What is the meaning of LEFT OUTER JOIN?

   Return every row from the left table

---

## Model 4

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
RIGHT OUTER JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1          | Alice	        | o1       |
| c2	        | Bob	          | o2       |
|   	        |      	        | o5       |
|   	        |     	        | o6       |


Questions

1. Which customers are not included in the result?

   c3 and c4

2. Which orders are not included in the result?

   all orders appear

3. When is a row included?

   All row from the right table (orders)

4. What is the meaning of RIGHT OUTER JOIN?

   Left join mirrie

---

## Model 5

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
FULL JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1          | Alice	        | o1       |
| c2	        | Bob	          | o2       |
| c3	        | Carol	        |          |
| c4	        | David	        |          |
|   	        |      	        | o5       |
|   	        |     	        | o6       |

Questions

1. Which customers are not included in the result?

   c3 and c4

2. Which orders are not included in the result?

   All appear

3. When is a row included?

   If that is existed in either table

4. What is the meaning of FULL JOIN?

   Combination of right and left join

---

## Model 6

Confirm the above by creating the tables in Postgres and running the queries. Paste the SQL for creating and populating the tables below.

```sql
DROP TABLE IF EXISTS Orders;
DROP TABLE IF EXISTS Customers;

CREATE TABLE Customers (
  customer_id   TEXT PRIMARY KEY,
  customer_name TEXT NOT NULL
);

CREATE TABLE Orders (
  order_id    TEXT PRIMARY KEY,
  customer_id TEXT
);

INSERT INTO Customers (customer_id, customer_name) VALUES
('c1', 'Alice'),
('c2', 'Bob'),
('c3', 'Carol'),
('c4', 'David');

INSERT INTO Orders (order_id, customer_id) VALUES
('o1', 'c1'),
('o2', 'c2'),
('o5', 'c5'),
('o6', 'c6');

-- INNER
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
INNER JOIN Orders o ON c.customer_id = o.customer_id;

-- LEFT
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
LEFT JOIN Orders o ON c.customer_id = o.customer_id;

-- RIGHT
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
RIGHT JOIN Orders o ON c.customer_id = o.customer_id;

-- FULL
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
FULL JOIN Orders o ON c.customer_id = o.customer_id;

```
