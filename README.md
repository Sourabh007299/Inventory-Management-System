# Inventory Management System - Complete Guide

Welcome to the Inventory Management System (IMS) project! This guide is designed for beginners. We'll start with the big picture: how the files are organized, how they talk to each other, and finally a simple explanation of what each file does.

---

## 1. Project Structure Overview

Our app is divided up into logical folders. Think of it like a restaurant:
- **`components/`**: The kitchen equipment and furniture we reuse everywhere. (Buttons, navbars, layouts).
- **`pages/`**: The individual rooms in our restaurant (The Login room, the Admin Dashboard room, the User Profile room).
- **`services/`**: The waiters and cooks handling the data (Talking to our "database").
- **`context/`**: The manager keeping track of who is currently eating in the restaurant (User login state).
- **`routes/`**: The signs on the doors telling people where they are allowed to go.

Here is the exact folder structure inside `src/`:
```text
src/
 ├── assets/          # Images and icons go here
 ├── components/      # Reusable UI pieces
 │    ├── layout/     # Things like the Dashboard Sidebar and Header
 │    └── ui/         # Buttons, Inputs, Cards
 ├── context/         # Stores global app state (like 'is the user logged in?')
 ├── pages/           # The actual screens of the app
 │    ├── admin/      # Screens only the Admin can see
 │    ├── auth/       # Login and Register screens
 │    └── user/       # Screens standard users see
 ├── routes/          # Logic to switch between pages
 ├── services/        # Fake database and data fetching logic
 ├── App.jsx          # The main Hub connecting routes and context
 ├── main.jsx         # The absolute starting point of the app
 └── index.css        # Global styles (Tailwind CSS)
```

---

## 2. How the Files Connect (The Flow)

If a user visits the app, the code flows in this order:

1. **`main.jsx`**: Bootstraps (starts) the React application and mounts it to the web browser. It grabs `App.jsx`.
2. **`App.jsx`**: Wraps the entire app in tools we need: a Router (for changing URLs without reloading), a Toaster (for showing little notification popups), and importantly: the **`AuthContext`**.
3. **`AuthContext.jsx`**: As soon as the app starts, this file checks: *"Is anyone logged in?"* by looking at the browser's memory (LocalStorage).
4. **`routes/AppRoutes.jsx`**: The app then evaluates what URL the user is on. If they aren't logged in, it forces them to the `Login` page.
5. **`pages/auth/Login.jsx`**: The user types their email and password. This file sends that info to `services/api.js`.
6. **`services/api.js`**: This file checks `mockDb.js` (our fake database) to see if the user exists and the password is correct. If it is, it reports success back to `AuthContext.jsx`.
7. **`components/layout/DashboardLayout.jsx`**: Now the user is logged in, they are sent to the Dashboard. This layout file renders the Sidebar and the Top Header, and leaves an empty space in the middle.
8. **`pages/admin...` or `pages/user...`**: Based on what link the user clicks in the Sidebar, the empty space in the middle is filled with the correct Page file!

---

## 3. Code Explanations (File by File)

Here is what the code inside each important file is doing in simple terms:

### The Starting Point
* **`main.jsx`**: It finds the root `<div id="root">` element in your HTML file and effectively "injects" the entire React application into it. That's all it does!
* **`App.jsx`**: Think of this as the main "wrapper". It doesn't display any visual elements itself, but it sets up the environment so pages can change smoothly and notifications can appear on screen.
* **`index.css` & `App.css`**: These contain basic styling rules using TailwindCSS (a system where we use class names to style things instead of writing raw CSS).

### Managing the User
* **`context/AuthContext.jsx`**: This is a global memory bank. Instead of passing the "user's name" from page to page manually, this file holds the user's data. Any file in the app can ask this file, *"Hey, who is logged in right now?"* It also handles the Login, Logout, and Register actions.

### Routing (Navigation)
* **`routes/AppRoutes.jsx`**: This acts like a traffic cop. It has a big list of URLs (like `/login`, `/admin/products`). When the URL changes, it tells React which component to draw on the screen.
* **`routes/ProtectedRoute.jsx`**: This is a security check. If a user tries to type `/admin/products` into their browser but they aren't an admin, this file stops them and redirects them away.

### The Services (Database)
* **`services/mockDb.js`**: In a real app, this would be a server far away. Here, it's just a file that pre-loads some fake users, products, and orders into the browser's Local Storage so we can test the app.
* **`services/api.js`**: These are the "helper functions" that our pages use. Instead of a page talking directly to the database, it asks `api.js` to do it. For example, it has a function to `getAllProducts()` or `createOrder()`.

### The Layouts
* **`components/layout/DashboardLayout.jsx`**: When you log in, every page has the same Sidebar on the left and Header on the top. Instead of copying and pasting that code into every page, we use this Layout file. It draws the Sidebar and Header, and then puts the specific page content in the main viewing area.

### The Pages (What you actually see)
* **`pages/auth/Login.jsx` & `Register.jsx`**: Simple forms with inputs for email and passwords. When you click "Submit", they call functions from `AuthContext.jsx`.
* **`pages/admin/AdminDashboard.jsx`**: The main screen for the boss. It fetches all products and orders to show summary numbers (like "Total Revenue").
* **`pages/admin/ManageProducts.jsx`**: A screen with a table listing all products. It has buttons that let the admin add new products, edit existing ones, or delete them.
* **`pages/user/ProductsList.jsx`**: A screen for the regular user. It shows the products nicely, often with pictures or cards, so the user can look at what is available to buy.
* **`pages/user/MyOrders.jsx`**: Shows the user a list of only *their* previous purchases.
g