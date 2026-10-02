# Backend Specification

## Overview
The backend is built with Node.js and Express.js. Its primary responsibilities are serving REST APIs, managing database interactions with MongoDB (via Mongoose), handling authentication and authorization, and executing critical business logic securely.

*Note: This is a specification document. Do not implement the backend code here.*

## Responsibilities

### 1. Authentication
- Manage user authentication using JSON Web Tokens (JWT).
- Securely hash all customer and admin passwords using `bcrypt` before storing them in the database.
- Provide endpoints for user registration and login.

### 2. Middleware
- **Auth Middleware:** Verifies the JWT provided in the `Authorization` header. If valid, attaches the user object to the request. If missing or invalid, returns an unauthorized error.
- **Admin Middleware:** Placed after the Auth Middleware. Checks if the authenticated user has the 'admin' role. If not, returns a forbidden error.

### 3. Category Operations
- Expose public endpoints to list categories.
- Provide protected admin endpoints to Add, Edit, and Delete categories.

### 4. Product Operations
- Expose public endpoints to retrieve products, supporting query parameters for searching by name and filtering by category.
- Provide protected admin endpoints to Add, Edit, and Delete products.

### 5. Order Operations
- Handle customer order placement securely.
- Provide a customer endpoint to view their own order history.
- Provide admin endpoints to view all orders and patch order statuses.

### 6. Security and Core Business Logic
- **Stock Validation:** Before finalizing an order, the server must verify that the requested quantity for each product does not exceed the available stock in the database.
- **Price Verification:** **CRITICAL:** The backend must NEVER trust the product prices sent from the frontend. During order creation, the server must fetch the current price for each product directly from MongoDB to calculate the order total.
- **Stock Reduction:** Upon successful order placement, the server must decrement the stock of the ordered products accordingly.

### 7. Error & Validation Expectations
- The server must validate all incoming request bodies (e.g., required fields, positive prices, valid emails).
- Return appropriate HTTP status codes (e.g., 400 for Bad Request/Validation Error, 401 for Unauthorized, 403 for Forbidden, 404 for Not Found).
- Ensure stock cannot become negative.
- Prevent duplicate emails during registration.
