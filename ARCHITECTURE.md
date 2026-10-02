# System Architecture

## Overall System Architecture

The Mini E-Commerce Demo project is structured as a two-tier architecture separating the frontend client and backend server, communicating over REST APIs. The application utilizes the MERN stack.

```
/client (React.js + Vite)  <--->  /server (Node.js + Express)  <--->  Database (MongoDB)
```

### Simple Architecture Diagram

```
[ BROWSER (User/Admin) ]
          │
          │ HTTP Request (Axios)
          │
          ▼
[ FRONTEND CLIENT (React/Vite) ]
  - UI Components & Pages
  - Client-side Routing
  - State Management
          │
          │ REST API / JSON Data / JWT
          │
          ▼
[ BACKEND SERVER (Node/Express) ]
  - Routes Definition
  - Controllers (Business Logic)
  - Auth/Admin Middleware
          │
          │ Mongoose ODM
          │
          ▼
[ DATABASE (MongoDB) ]
  - User Collection
  - Category Collection
  - Product Collection
  - Order Collection
```

## Client → API → Server → MongoDB Flow
1. **Client Request:** The user performs an action on the React frontend, triggering an Axios HTTP request.
2. **API Endpoint:** The request is sent to a specific backend REST API endpoint.
3. **Server Processing:** Express routes the request to the appropriate controller.
   - Middlewares (like Auth) process the request first if required.
   - The controller executes the core business logic.
4. **Database Interaction:** The controller uses Mongoose to perform CRUD operations on MongoDB.
5. **Response:** Data is retrieved, manipulated, and formatted as a JSON response to be sent back to the client.

## Authentication Flow
1. User submits login credentials (email & password).
2. Server validates credentials against MongoDB using bcrypt.
3. On success, the server generates a JSON Web Token (JWT) and returns it.
4. Client stores the JWT.
5. Client attaches the JWT as a Bearer token in the `Authorization` header for subsequent requests to protected endpoints.
6. Server `auth middleware` validates the token before processing protected requests.

## Admin/User Separation
- **Customer:** Authenticated user with standard access. Can view products, manage their cart, place orders, and view their order history.
- **Admin:** Authenticated user with elevated access. Handled by an `admin middleware` on the server which checks the user's role.
- Only admins can access admin panel UI routes and perform actions like adding products/categories or changing order statuses.

## Order Flow
1. Customer adds items to their Cart on the client-side.
2. Customer validates the cart and goes to Checkout.
3. Customer submits shipping information and clicks "Place Order".
4. The client sends a request to the backend with cart items and shipping info.
5. **Backend validation:** Verify sufficient stock for all items.
6. **Price Verification:** Fetch the current product price from MongoDB (ignore client prices).
7. Create the Order document in MongoDB.
8. Reduce the product stock in MongoDB.
9. Return success to the client.
10. Client clears the cart.
