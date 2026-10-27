# Online Food Ordering System on a Budget

A full-stack food ordering application for discovering affordable meals, filtering by a maximum item price, building a cart, and placing orders.

## Stack

- **Frontend:** React, Vite, React Router, Axios, Context API
- **Backend:** Java 17, Spring Boot, Spring Web, Spring Data JPA, Bean Validation
- **Database:** MySQL 8+

## Project structure

```text
online-food-ordering-system/
├── frontend/    # Standalone Vite React application
├── backend/     # Standalone Spring Boot application
└── database/    # MySQL schema and sample data
```

## Run locally

### 1. Create the database

Create a MySQL database and run `database/schema.sql`:

```sql
CREATE DATABASE food_ordering;
```

Then execute the schema file with a MySQL user that can create tables.

### 2. Start the backend

Set database values in the shell or edit `application.properties`:

```bash
cd backend
set DB_USERNAME=root
set DB_PASSWORD=your-password
mvn spring-boot:run
```

On macOS/Linux, use `export DB_USERNAME=root` and `export DB_PASSWORD=your-password`.
The API is available at `http://localhost:8081` (or your configured port).

The default admin key is `change-me-admin-key`. Set `APP_ADMIN_KEY` before starting in any shared environment.

### 3. Start the frontend

```bash
cd frontend
npm install
npm run dev
```

Open the URL shown by Vite, normally `http://localhost:5173`.

The Vite proxy forwards `/api` calls to `http://localhost:8081`.

## Authentication & Admin Access

There is no separate Admin button exposed to regular users on the navbar. Instead:
- Visit **Sign In / Account** (`/login`).
- Entering admin credentials (e.g., `admin@budgetbites.com` / `admin` or password `admin123` / `change-me-admin-key`) will authenticate and automatically open the **Admin Dashboard** (`/admin`).
- Regular customers can register and sign in to keep track of their profile, address, and orders.
- Clicking **Account** when logged in displays the profile card with quick access to the Admin Dashboard (for admins), My Orders, and Sign Out.

## API overview

### Authentication

| Method | Endpoint | Purpose |
| --- | --- | --- |
| POST | `/api/auth/login` | Sign in (customer or admin) |
| POST | `/api/auth/register` | Register customer account |

### Food

| Method | Endpoint | Purpose |
| --- | --- | --- |
| GET | `/api/foods` | List food items |
| GET | `/api/foods/{id}` | Get one food item |
| GET | `/api/foods/search?name=` | Search by name |
| GET | `/api/foods/category/{category}` | Filter by category |
| GET | `/api/foods/budget?maxPrice=` | Filter items at or below a price |
| POST | `/api/foods` | Create food item (admin key) |
| PUT | `/api/foods/{id}` | Update food item (admin key) |
| DELETE | `/api/foods/{id}` | Delete food item (admin key) |

### Orders

`POST /api/orders` accepts customer details and `{ foodId, quantity }` items. Prices are looked up from the database and totals are calculated by the server.

| Method | Endpoint | Purpose |
| --- | --- | --- |
| POST | `/api/orders` | Place an order |
| GET | `/api/orders/{id}` | Get an order |
| GET | `/api/orders/customer/{customerId}` | Get a customer's orders |
| GET | `/api/orders` | List all orders (admin key) |
| PATCH | `/api/orders/{id}/status` | Change status (admin key), body `{ "status": "PREPARING" }` |

## Checks

```bash
cd frontend && npm run build
cd backend && mvn test
```
