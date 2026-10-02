# Frontend Specification

## Overview
The frontend is a single-page application built with React.js, Vite, and styled with Tailwind CSS. It communicates with the backend via Axios.

*Note: This is a specification document. Do not implement the React code here.*

## Responsibilities
- Manage user authentication state (login/logout) on the client side.
- Handle routing between public pages, customer pages, and the protected admin panel.
- Manage the shopping cart state.
- Provide a responsive, modern, and clean UI using Tailwind CSS.
- Handle loading, empty, and error states gracefully (e.g., using toast messages).

## Required UI Pages

### Public / Customer Pages
- **Home:** Landing page.
- **Products:** Displays a responsive grid of products. Includes search functionality (by text) and category filtering options (e.g., All, Electronics, Fashion).
- **Product Details:** Shows detailed information about a specific product, including image, description, price, category, and an "Add to Cart" button.
- **Cart:** Displays selected items, allows increasing/decreasing quantity (with maximum bounded by available stock), removing items, and shows the total price.
- **Checkout:** Collects shipping details (Name, Phone, Address, City, Pincode) and displays a summary of the order. Hardcoded to Cash on Delivery.
- **Login / Register:** Forms for user authentication.
- **My Orders:** Displays the authenticated customer's order history and statuses.

### Admin Panel Pages
- **Admin Layout:** Includes an Admin Sidebar for navigation and a main content area.
- **Categories:** View list of categories in a table, Add Category, Edit Category, Delete Category (with confirmation dialog).
- **Products:** View list of products in a table, Add Product, Edit Product, Delete Product (with confirmation dialog).
- **Orders:** View a list of all customer orders. View order details. Change the status of an order.

## Global Components
- **Navbar:** Responsive navigation. Shows links to Home, Products, Cart. Displays Login/Register or User/Logout based on auth state. Shows cart item count.
- **Toast Messages:** Used to display success and error notifications across the app.
- **Loading States:** Spinners or skeletons to indicate data fetching.
- **Empty States:** Clear messaging when a list (like cart, products, or orders) is empty.

## Styling Requirements (Tailwind CSS)
- **Modern & Clean:** The design should be simple and professional without over-designing.
- **Cards & Tables:** Use cards for products and forms. Use tables for admin data presentation.
- **Responsive:** The UI must be fully responsive, adapting seamlessly to mobile, tablet, and desktop viewports.

## Axios Usage
- All API communication must be handled using Axios.
- Axios should be configured to automatically attach the JWT Bearer token to the `Authorization` header for requests to protected endpoints.
