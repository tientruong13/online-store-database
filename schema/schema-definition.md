# Relation Schema Definitions

This database is designed for an online store. It manages customers, products, orders, order items, and payments.

## 1. Customer

Customer(customer_id, first_name, last_name, email, phone)

- customer_id: INTEGER, positive integer
- first_name: VARCHAR(50)
- last_name: VARCHAR(50)
- email: VARCHAR(255)
- phone: VARCHAR(20)

**Primary Key:** customer_id

## 2. Product

Product(product_id, product_name, description, price, stock_quantity)

- product_id: INTEGER, positive integer
- product_name: VARCHAR(150)
- description: TEXT
- price: DECIMAL(10,2), non-negative
- stock_quantity: INTEGER, non-negative

**Primary Key:** product_id

## 3. Order

Order(order_id, customer_id, order_date, status, total_amount)

- order_id: INTEGER, positive integer
- customer_id: INTEGER, positive integer
- order_date: TIMESTAMP
- status: VARCHAR(20)
- total_amount: DECIMAL(10,2), non-negative

**Primary Key:** order_id

**Foreign Key:** customer_id references Customer(customer_id)

## 4. OrderItem

OrderItem(order_id, product_id, quantity, unit_price)

- order_id: INTEGER, positive integer
- product_id: INTEGER, positive integer
- quantity: INTEGER, positive integer
- unit_price: DECIMAL(10,2), non-negative

**Primary Key:** (order_id, product_id)

**Foreign Keys:**
- order_id references Order(order_id)
- product_id references Product(product_id)

## 5. Payment

Payment(payment_id, order_id, payment_date, amount, payment_method, payment_status)

- payment_id: INTEGER, positive integer
- order_id: INTEGER, positive integer
- payment_date: TIMESTAMP
- amount: DECIMAL(10,2), positive
- payment_method: VARCHAR(30)
- payment_status: VARCHAR(20)

**Primary Key:** payment_id

**Foreign Key:** order_id references Order(order_id)
