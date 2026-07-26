# NextGym 🏋️‍♂️ - "Rent Sports & Outdoor Gear Instantly"

[![Node.js](https://img.shields.io/badge/Node.js-22.x-green.svg)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-6.0-blue.svg)](https://www.typescriptlang.org/)
[![Express](https://img.shields.io/badge/Express-5.2-purple.svg)](https://expressjs.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-8.21-blue.svg)](https://www.postgresql.org/)
[![License](https://img.shields.io/badge/License-ISC-yellow.svg)](LICENSE)

## 📋 Overview

**NextGym** is a robust backend API built for a sports and outdoor equipment rental service, designed and developed as a complete system implementation. Customers can browse available gear, place rental orders, make secure payments, and return equipment. Providers manage their gear inventory and fulfill rental orders, while Admins oversee the platform, manage users, and moderate listings.

🔗 **Live Backend API URL:** https://nextgym.onrender.com/

## 🛠️ Tech Stack

<div align="left" style="background-color: #f4f6f9; padding: 10px; border-radius: 8px;">

| Category | Technology | Purpose |
| :--- | :--- | :--- |
| **Backend** | Node.js + Express  | REST API |
| **Language** | TypeScript  | Type safety |
| **Database & ORM** | PostgreSQL + Prisma  | Database management & ORM |
| **Authentication** | JWT (JSON Web Tokens)  | Secure user authentication |
| **Deployment** | Render | Backend API hosting & deployment|

</div>

## 👥 Roles & Permissions

<div align="left" style="background-color: #f4f6f9; padding: 10px; border-radius: 8px;">

| Role | Description | Key Permissions |
| :--- | :--- | :--- |
| **Customer** | Users who rent sports gear | Browse gear, place rental orders, make payments, track status, leave reviews |
| **Provider** | Gear vendors/rental shops | Manage gear inventory, view incoming orders, update order status |
| **Admin** | Platform moderators | Manage users, oversee all rentals, manage gear categories |

</div>

## ✨ Features

### Customer Features

* User registration and role-based login. 
* Place rental orders specifying items and dates. 
* Secure payments integration via Stripe. 
* Track rental order status and view payment history. 
* Leave ratings and reviews after returning equipment. 

### Provider Features

* Add, update, and remove gear items from personal inventory. 
* Monitor stock levels and availability. 
* View and process incoming customer rental orders (confirm). 

### Admin Features

* Comprehensive user management (view, suspend, or activate users). 
* Oversight of platform-wide gear listings and rental orders. 
* Category administration. 
  

## 🏗️ Architecture

```text
src/
├── controllers/
│   ├── admin.controller.ts
│   ├── auth.controller.ts
│   ├── gear.controller.ts
│   ├── payment.controller.ts
│   ├── provider.controller.ts
│   ├── rental.controller.ts
│   └── review.controller.ts
├── middlewares/
├── routes/
│   ├── admin.routes.ts
│   ├── auth.routes.ts
│   ├── category.routes.ts
│   ├── gear.routes.ts
│   ├── index.ts
│   ├── payment.routes.ts
│   ├── provider.routes.ts
│   ├── rental.routes.ts
│   └── review.routes.ts
├── validations/
│   ├── auth.validation.ts
│   ├── category.validation.ts
│   ├── gear.validation.ts
│   ├── rental.validation.ts
│   └── review.validation.ts
├── app.ts
├── AppError.ts
├── prisma.ts
└── server.ts
```
## 🚀 Getting Started

### Prerequisites

- **Node.js** (v22 or higher)
- **PostgreSQL** (v14 or higher)
- **npm** or **yarn**

## 🌐 API Endpoints Specification

### 🔹 Authentication Module

### 1. User Registration

**Access:** Public

**Description:** Register a new user account with contributor or maintainer role

**Endpoint**

`POST /api/auth/signup`

**Request Body**

```json
{
  "name": "John Doe",
  "email": "john.doe@devpulse.com",
  "password": "securePassword123",
  "role": "contributor"
}
```

**Success Response (201 Created)**

```json
{
  "success": true,
  "message": "User registered successfully",
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@devpulse.com",
    "role": "contributor",
    "created_at": "2026-01-20T09:00:00Z",
    "updated_at": "2026-01-20T09:00:00Z"
  }
}
```

---

### 2. User Login

**Access:** Public

**Description:** Authenticate user and receive JWT token

**Endpoint**

`POST /api/auth/login`

**Request Body**

```json
{
  "email": "john.doe@devpulse.com",
  "password": "securePassword123"
}
```

**Success Response (200 OK)**

```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": 1,
      "name": "John Doe",
      "email": "john.doe@devpulse.com",
      "role": "contributor",
      "created_at": "2026-01-20T09:00:00Z",
      "updated_at": "2026-01-20T09:00:00Z"
    }
  }
}
```
 
---

### 🔹 Issues Module

### 3. Create Issue

**Access:** Authenticated users (`contributor`, `maintainer`)

**Description:** Create a new bug report or feature request

**Endpoint**

`POST /api/issues`

**Headers**

```
Authorization: <JWT_TOKEN>
```

**Request Body**

```json
{
  "title": "Database connection timeout under load",
  "description": "Pool exhausts after 50+ concurrent queries, causing 500 errors",
  "type": "bug"
}
```

**Success Response (201 Created)**

```json
{
  "success": true,
  "message": "Issue created successfully",
  "data": {
    "id": 45,
    "title": "Database connection timeout under load",
    "description": "Pool exhausts after 50+ concurrent queries, causing 500 errors",
    "type": "bug",
    "status": "open",
    "reporter_id": 1,
    "created_at": "2026-01-20T10:30:00Z",
    "updated_at": "2026-01-20T10:30:00Z"
  }
}
```

---

### 4. Get All Issues

**Access:** Public

**Description:** Retrieve all issues with optional sorting and filtering

**Endpoint**

`GET /api/issues?sort=newest`

**Success Response (200 OK)**

```json
{
  "success": true,
  "message": "Issues retrived successfully",
  "data": [
    {
      "id": 45,
      "title": "Database connection timeout under load",
      "description": "Pool exhausts after 50+ concurrent queries, causing 500 errors",
      "type": "bug",
      "status": "open",
      "reporter": {
        "id": 1,
        "name": "John Doe",
        "role": "contributor"
      },
      "created_at": "2026-01-20T10:30:00Z",
      "updated_at": "2026-01-20T14:45:00Z"
    }
  ]
}
```

---

### 5. Get Single Issue

**Access:** Public

**Description:** Retrieve full details of a specific issue

**Endpoint**

`GET /api/issues/:id`

**Success Response (200 OK)**

```json
{
  "success": true,
  "message": "Issue retrived successfully",
  "data": {
    "id": 45,
    "title": "Database connection timeout under load",
    "description": "Pool exhausts after 50+ concurrent queries, causing 500 errors",
    "type": "bug",
    "status": "open",
    "reporter": {
      "id": 1,
      "name": "John Doe",
      "role": "contributor"
    },
    "created_at": "2026-01-20T10:30:00Z",
    "updated_at": "2026-01-20T14:45:00Z"
  }
}
```

---

### 6. Update Issue

**Access:** Maintainer (any issue) OR Contributor (own issue, only if status is `open`)

**Description:** Update issue title, description, or type

**Endpoint**

`PATCH /api/issues/:id`

**Headers**

```
Authorization: <JWT_TOKEN>
```

**Request Body**

```json
{
  "title": "Updated: Database pool exhaustion fix needed",
  "description": "Updated description with reproduction steps...",
  "type": "bug"
}
```

**Success Response (200 OK)**

```json
{
  "success": true,
  "message": "Issue updated successfully",
  "data": {
    "id": 45,
    "title": "Updated: Database pool exhaustion fix needed",
    "description": "Updated description with reproduction steps...",
    "type": "bug",
    "status": "in_progress",
    "reporter_id": 1,
    "created_at": "2026-01-20T10:30:00Z",
    "updated_at": "2026-01-20T14:45:00Z"
  }
}
```

---

### 7. Delete Issue

**Access:** Maintainer only

**Description:** Permanently remove an issue from the system

**Endpoint**

`DELETE /api/issues/:id`

**Headers**

```
Authorization: <JWT_TOKEN>
```

**Success Response (200 OK)**

```json
{
  "success": true,
  "message": "Issue deleted successfully"
}
```

---
## 🚀 Deployment Guide (Render)

Follow these steps to deploy the Express & TypeScript backend to **Render**.

### Step 1: Production Configuration
Ensure your application is configured for a production environment before deploying.

1. **Dynamic Port Binding** (`src/server.ts`):
   
   ```typescript
   const PORT = process.env.PORT || 5000;
   ```
   
2. **Production Scripts** (`package.json`):
   
   ```json
   "scripts": {
      "build": "tsc",
      "start": "node dist/server.js",
      "dev": "tsx watch src/server.ts"
   }
  

### Step 2: Push to GitHub

Make sure your `.gitignore` includes `.env` and `node_modules/`, then push your changes:

```bash
git add .
git commit -m "Final Deploy and Ready to go Live...."
git push origin main
````

### Step 3: Create Render Web Service

1. Log into the [Render Dashboard](https://render.com).
2. Click **New +** > **Web Service**.
3. Connect your GitHub account and select the required repository.

### Step 4: Configure Web Service

Fill out the configuration dashboard with the following settings:

| Field | Configuration Value |
| :--- | :--- |
| **Name** | `Relavent Name` |
| **Region** | Select closest region (e.g., `Oregon (US West)`) |
| **Branch** | `main` |
| **Root Directory** | *Leave blank* |
| **Runtime** | `Node` |
| **Build Command** | `npm install && npm run build` |
| **Start Command** | `npm run start` |
| **Instance Type** | `Free` |

### Step 5: Environment Variables

Navigate to the **Environment** tab (or click **Advanced**) and inject your keys:

* `DATABASE_URL` = `Your required input from .env file`
* `JWT_SECRET` = `Your required input from .env file`

> ⚠️ **Note:** Do not add `PORT` manually. Render handles its own internal port allocation.

### Step 6: Deploy & Verify

1. Click **Deploy Web Service**.
2. Monitor the **Logs** tab. Success is confirmed when you see your initialization logs:
   ```text
   🐘 PostgreSQL pool initialized successfully.
   🚀 DevPulse server is racing hot on port 10000
3. Copy your live public URL from the top left of the dashboard (e.g., https://devpulse-api.onrender.com ) to test in Postman or your browser.

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the ISC License.

## 👨‍💻 Author

Md. Abidur Rahman  
