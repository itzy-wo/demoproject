# Database Design

## Overview
The application uses MongoDB as the database and Mongoose as the Object Data Modeling (ODM) library. The schema consists of four core models: User, Category, Product, and Order.

*Note: This document defines data concepts and structures. Do not write Mongoose code here.*

## 1. User Model
Stores both customer and admin accounts.

**Fields:**
- `name` (String): Full name of the user.
- `email` (String): Email address.
- `password` (String): Hashed password (using bcrypt).
- `role` (String): Defines permissions. Values: 'customer', 'admin'. Default: 'customer'.

**Validation Constraints:**
- `name`, `email`, and `password` are required.
- `email` must be valid and strictly unique.
- `password` must be at least 6 characters long before hashing.

## 2. Category Model
Stores product categories managed by the admin.

**Fields:**
- `name` (String): The category name (e.g., Electronics, Fashion).
- `description` (String): Short description of the category.

**Validation Constraints:**
- `name` and `description` are required.

**Relationships:**
- Products will reference a Category.

## 3. Product Model
Stores products available in the store.

**Fields:**
- `name` (String): Name of the product.
- `description` (String): Detailed product description.
- `price` (Number): Selling price.
- `image` (String): URL or path to the product image.
- `category` (Reference): Reference to the Category model.
- `stock` (Number): Quantity of items available for sale.

**Validation Constraints:**
- All fields are required.
- `price` must be a positive number.
- `stock` cannot be negative.
- `category` must be a valid reference to an existing Category.

**Relationships:**
- Belongs to one Category.

## 4. Order Model
Stores customer orders placed via the checkout process.

**Fields:**
- `user` (Reference): Reference to the User who placed the order.
- `products` (Array of Objects): The items ordered.
- `totalAmount` (Number): The total cost of the order (calculated on the server).
- `shippingAddress` (Object): The delivery details.
- `status` (String): Current status of the order.
- `createdAt` (Date): Order placement timestamp.

**Order Structure Details:**

*`products` Array item structure:*
- `product` (Reference): The Product ID.
- `quantity` (Number): Quantity ordered.
- `price` (Number): The recorded price at the time of purchase.

*`shippingAddress` Object structure:*
- `name` (String)
- `phone` (String)
- `address` (String)
- `city` (String)
- `pincode` (String)

*Order Statuses:*
- Pending
- Confirmed
- Shipped
- Delivered
- Cancelled

**Validation Constraints:**
- All top-level fields are required.
- `totalAmount` must be positive.
- Status must be one of the allowed enum values.

**Relationships:**
- Belongs to one User.
- Contains references to multiple Products.
