# Unit 1 Analysis

## Modelling Justification

The database is designed for a simple online store and contains five main relations: Customer, Product, Order, OrderItem, and Payment. I separated these roles into different relations because each one represents a different type of information in the application. This also reduces repeated data and makes the database easier to maintain.

For Customer, I chose customer_id as the primary key instead of email. Although email is unique, a customer may change an email address in the future. A numeric customer_id gives each customer a stable identifier that does not depend on personal information. I used the same approach for Product, Order, and Payment by giving each relation its own numeric primary key.

OrderItem is different because I used a composite primary key consisting of order_id and product_id. An order can contain many products, and a product can appear in many different orders. OrderItem resolves this many-to-many relationship between Order and Product. The composite key also prevents the same product from appearing as multiple separate rows in one order. Instead, the quantity attribute records how many units of that product were ordered.

The foreign keys connect the five relations. Order.customer_id references Customer.customer_id, which means every order must belong to an existing customer. OrderItem.order_id references Order.order_id, and OrderItem.product_id references Product.product_id. Payment.order_id references Order.order_id. These foreign keys prevent records from referring to objects that do not exist.

I chose different ON DELETE behaviors based on the meaning of each relationship. Customer to Order uses RESTRICT because deleting a customer who already has orders could remove an important connection to the order history. Product to OrderItem also uses RESTRICT because products that have already been purchased should remain connected to historical order records. Order to Payment uses RESTRICT because payment information represents transaction history and should not become disconnected from its order.

For Order to OrderItem, I chose ON DELETE CASCADE. Order items depend completely on their parent order and do not have a useful meaning by themselves. Therefore, if an order is deleted, its associated order items should also be removed.

Additional constraints prevent invalid values. Product prices and stock quantities cannot be negative. OrderItem quantity must be greater than zero, and payment amounts must also be greater than zero. Customer email addresses must be unique. Required attributes such as names, order dates, statuses, product names, and payment information cannot be NULL. These rules move important data validation into the database instead of depending only on the application.

Overall, this design keeps customer, product, ordering, and payment information separated while using keys and constraints to maintain the relationships between them. It supports common operations such as finding a customer's orders, viewing the products in an order, checking product inventory, and finding payments associated with an order.

## Reflection

One design decision that another designer could reasonably make differently is the primary key for OrderItem. I chose the combination of order_id and product_id as the composite primary key. With this design, a product can appear only once in a particular order. If a customer orders three units of the same product, the system stores one OrderItem row with a quantity of three instead of three separate rows.

Another reasonable design would add an order_item_id as a separate primary key. This would allow multiple rows containing the same product within one order. That approach could be useful for a more complex store where identical products may need separate discounts, customization options, shipping information, or other item-level details.

For this project, I prefer the composite key because the store model is simple and does not require those features. Most reads need to identify which products belong to an order, and the combination of order_id and product_id directly represents that relationship. It also prevents duplicate product rows without requiring another uniqueness constraint.

The tradeoff is that future requirements could make the composite key less flexible. However, for the current read and write patterns, it keeps the model simple, reduces unnecessary identifiers, and clearly represents the relationship between orders and products.
