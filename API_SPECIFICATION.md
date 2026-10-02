# API Specification

## Overview
This document outlines the REST APIs for the Mini E-Commerce Demo project.

## Authentication Endpoints

### 1. Register Customer
- **Method:** POST
- **Endpoint:** `/api/auth/register`
- **Purpose:** Create a new customer account.
- **Authentication:** None
- **Request Data:**
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "confirmPassword": "password123"
  }
  ```
- **Expected Behavior:** Validates inputs, hashes password, saves user, returns JWT and user details.
- **Validation:** All fields required, valid email, unique email, password minimum 6 characters, password and confirm password must match.

### 2. Login User
- **Method:** POST
- **Endpoint:** `/api/auth/login`
- **Purpose:** Authenticate a customer or admin.
- **Authentication:** None
- **Request Data:**
  ```json
  {
    "email": "john@example.com",
    "password": "password123"
  }
  ```
- **Expected Behavior:** Verifies credentials, returns JWT and user details (including role).
- **Validation:** Both fields required.

## Category Endpoints

### 3. Get Categories
- **Method:** GET
- **Endpoint:** `/api/categories`
- **Purpose:** Retrieve all product categories.
- **Authentication:** None
- **Request Data:** None
- **Expected Behavior:** Returns an array of category objects.

### 4. Add Category
- **Method:** POST
- **Endpoint:** `/api/categories`
- **Purpose:** Create a new category.
- **Authentication:** Admin Token
- **Request Data:** `name`, `description`
- **Expected Behavior:** Creates category, returns created object.
- **Validation:** Required fields.

### 5. Edit Category
- **Method:** PUT
- **Endpoint:** `/api/categories/:id`
- **Purpose:** Update an existing category.
- **Authentication:** Admin Token
- **Request Data:** `name`, `description`
- **Expected Behavior:** Updates category, returns updated object.
- **Validation:** Required fields, valid ID.

### 6. Delete Category
- **Method:** DELETE
- **Endpoint:** `/api/categories/:id`
- **Purpose:** Delete a category.
- **Authentication:** Admin Token
- **Request Data:** None
- **Expected Behavior:** Deletes category, returns success message.

## Product Endpoints

### 7. Get Products
- **Method:** GET
- **Endpoint:** `/api/products`
- **Purpose:** Retrieve products. Supports filtering and searching.
- **Authentication:** None
- **Query Parameters:**
  - `category` (optional): Filter by category name/slug.
  - `search` (optional): Text search by product name.
  - Example: `GET /api/products?category=electronics&search=phone`
- **Expected Behavior:** Returns an array of product objects matching the queries.

### 8. Get Product Details
- **Method:** GET
- **Endpoint:** `/api/products/:id`
- **Purpose:** Retrieve a single product by ID.
- **Authentication:** None
- **Request Data:** None
- **Expected Behavior:** Returns single product object.

### 9. Add Product
- **Method:** POST
- **Endpoint:** `/api/products`
- **Purpose:** Create a new product.
- **Authentication:** Admin Token
- **Request Data:** `name`, `description`, `price`, `image`, `category`, `stock`
- **Expected Behavior:** Creates product, returns created object.
- **Validation:** Required fields, positive price, non-negative stock, valid category ID.

### 10. Edit Product
- **Method:** PUT
- **Endpoint:** `/api/products/:id`
- **Purpose:** Update a product.
- **Authentication:** Admin Token
- **Request Data:** `name`, `description`, `price`, `image`, `category`, `stock`
- **Expected Behavior:** Updates product, returns updated object.
- **Validation:** Required fields, positive price, non-negative stock, valid category ID.

### 11. Delete Product
- **Method:** DELETE
- **Endpoint:** `/api/products/:id`
- **Purpose:** Delete a product.
- **Authentication:** Admin Token
- **Request Data:** None
- **Expected Behavior:** Deletes product, returns success message.

## Order Endpoints

### 12. Create Order
- **Method:** POST
- **Endpoint:** `/api/orders`
- **Purpose:** Place a new order from the checkout process.
- **Authentication:** Customer Token
- **Request Data:** `products` (array of ID and quantity), `shippingAddress`
- **Expected Behavior:** Validates stock, calculates total price from database values, creates order, reduces product stock, returns order object.
- **Validation:** Products array not empty, valid product IDs, sufficient stock, required address fields.

### 13. Get My Orders
- **Method:** GET
- **Endpoint:** `/api/orders/my-orders`
- **Purpose:** Retrieve orders for the currently authenticated customer.
- **Authentication:** Customer Token
- **Request Data:** None
- **Expected Behavior:** Returns array of user's orders.

### 14. Admin Get All Orders
- **Method:** GET
- **Endpoint:** `/api/admin/orders`
- **Purpose:** Retrieve all orders from all customers.
- **Authentication:** Admin Token
- **Request Data:** None
- **Expected Behavior:** Returns array of all orders.

### 15. Admin Update Order Status
- **Method:** PATCH
- **Endpoint:** `/api/admin/orders/:id/status`
- **Purpose:** Change the status of a specific order.
- **Authentication:** Admin Token
- **Request Data:** `status`
- **Expected Behavior:** Updates the status, returns the updated order.
- **Validation:** Status must be one of: Pending, Confirmed, Shipped, Delivered, Cancelled.
