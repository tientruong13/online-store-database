# Integrity Constraints

The following constraints protect the consistency and validity of the online store database.

## Customer

- `customer_id` must be a positive integer and is the primary key.
- `first_name` and `last_name` cannot be NULL.
- `email` cannot be NULL and must be unique.
- Each customer must have a unique `customer_id`.

## Product

- `product_id` must be a positive integer and is the primary key.
- `product_name` cannot be NULL.
- `price` must be greater than or equal to 0.
- `stock_quantity` must be greater than or equal to 0.

## Order

- `order_id` must be a positive integer and is the primary key.
- `customer_id` cannot be NULL and must reference an existing customer.
- `order_date` cannot be NULL.
- `status` cannot be NULL.
- `total_amount` must be greater than or equal to 0.

### Foreign Key: Order.customer_id

`customer_id` references `Customer(customer_id)`.

**ON DELETE: RESTRICT**

A customer cannot be deleted while orders belonging to that customer still exist. This preserves the order history and prevents orders from referencing a customer that no longer exists.

## OrderItem

- `(order_id, product_id)` is the composite primary key.
- `order_id` must reference an existing order.
- `product_id` must reference an existing product.
- `quantity` must be greater than 0.
- `unit_price` must be greater than or equal to 0.

### Foreign Key: OrderItem.order_id

`order_id` references `Order(order_id)`.

**ON DELETE: CASCADE**

When an order is deleted, its order items should also be deleted because those items cannot exist without their parent order.

### Foreign Key: OrderItem.product_id

`product_id` references `Product(product_id)`.

**ON DELETE: RESTRICT**

A product cannot be deleted if it appears in an existing order item. This protects historical order information.

## Payment

- `payment_id` must be a positive integer and is the primary key.
- `order_id` must reference an existing order.
- `payment_date` cannot be NULL.
- `amount` must be greater than 0.
- `payment_method` cannot be NULL.
- `payment_status` cannot be NULL.

### Foreign Key: Payment.order_id

`order_id` references `Order(order_id)`.

**ON DELETE: RESTRICT**

An order cannot be deleted while payment records exist for it. Payment records should be preserved for financial and transaction history.
