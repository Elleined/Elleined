# Laravel Social Media API

A robust RESTful API for a social media platform built with Laravel. This API provides the foundational backend services required to run a modern social network, including user management, post creation, commenting, and a reaction system.

## 🚀 Features Overview

*   **Authentication & Authorization:** Secure user registration, login, logout, and password reset functionalities using Laravel Sanctum for token-based authentication.
*   **User Profiles:** Endpoints for users to view, update, and delete their profiles, as well as change their passwords.
*   **Admin Management:** Dedicated administrative endpoints to manage users (CRUD operations including soft deletes).
*   **Post Management:** Create, read, update, and delete posts. Supports soft deleting to preserve data integrity.
*   **Commenting System:** Users can add comments to posts. Includes full CRUD capabilities for comments.
*   **Reactions:** Users can react to both posts and comments (e.g., likes). Includes toggling functionality.
*   **References:** Read-only endpoints providing available provider types and emojis used across the platform.

## 🛠 Tech Stack

*   **Framework:** [Laravel 11.x](https://laravel.com/)
*   **Language:** PHP 8.2+
*   **Database:** SQLite (default for development), compatible with MySQL/PostgreSQL.
*   **Authentication:** [Laravel Sanctum](https://laravel.com/docs/sanctum)
*   **API Documentation:** [Scramble](https://scramble.dedoc.co/) (Auto-generates OpenAPI docs)

---

## 💻 Installation & Setup Instructions

Follow these steps to get the project running on your local machine.

### Prerequisites
*   PHP >= 8.2
*   Composer
*   Git

### 1. Clone the Repository
```bash
git clone https://github.com/Elleined/laravel-social-media-api.git
cd laravel-social-media-api
```

### 2. Install Dependencies
```bash
composer install
```

### 3. Environment Setup
Copy the example environment file and create a new `.env` file.
```bash
cp .env.example .env
```
*(Note: If `.env.example` is missing, you can create a blank `.env` file and manually set the core variables like `APP_KEY`, `DB_CONNECTION`, etc.)*

### 4. Generate Application Key
```bash
php artisan key:generate
```

### 5. Database Configuration
By default, this guide assumes SQLite for quick setup.
Ensure your `.env` contains:
```env
DB_CONNECTION=sqlite
# DB_DATABASE=/absolute/path/to/database/database.sqlite
```
Create the SQLite database file:
```bash
touch database/database.sqlite
```

### 6. Run Migrations
Run the database migrations to create all necessary tables.
```bash
php artisan migrate
```

### 7. Serve the Application
Start the Laravel local development server.
```bash
php artisan serve
```
The API will be accessible at `http://localhost:8000`.

---

## 🔐 Authentication Flow

This API uses **Laravel Sanctum** for issuing API tokens.

1.  **Register/Login:** Make a POST request to `/api/register` or `/api/login` with your credentials.
2.  **Receive Token:** Upon success, the server responds with a bearer token.
3.  **Authenticated Requests:** For all protected endpoints (anything outside of register/login/forgot-password), you must include this token in the `Authorization` header of your HTTP request.

**Header Format:**
```http
Authorization: Bearer <your_access_token_here>
```

---

## 📡 API Endpoints Summary

Below is a high-level summary of the available endpoints. All API routes are prefixed with `/api`.

### Public Routes
*   `GET /api` - Health check (returns 'pong')
*   `POST /api/register` - Register a new user
*   `POST /api/login` - Authenticate a user and receive a token
*   `POST /api/forgot-password` - Request a password reset link/token
*   `GET /api/forgot-password/verify-reset-token` - Verify the provided reset token
*   `PUT /api/forgot-password/reset-password` - Submit the new password

### Protected Routes (Requires Bearer Token)

**Auth & Profile**
*   `POST /api/logout` - Revoke the current access token
*   `GET /api/me/profile` - View authenticated user's profile
*   `PUT /api/me/profile` - Update profile information
*   `DELETE /api/me/profile` - Delete the user account
*   `PATCH /api/me/profile/password` - Change account password

**Admin User Management**
*   `GET /api/admin/users` - List all users
*   `POST /api/admin/users` - Create a new user manually
*   `GET /api/admin/users/{user}` - View specific user details
*   `PUT /api/admin/users/{user}` - Update specific user details
*   `DELETE /api/admin/users/{user}` - Delete a user

**Posts**
*   `GET /api/posts` - List all posts
*   `POST /api/posts` - Create a new post
*   `PUT /api/posts/{post}` - Update a post
*   `DELETE /api/posts/{post}` - Delete a post

**Post Reactions**
*   `GET /api/posts/{post}/reactions` - List reactions for a specific post
*   `POST /api/posts/{post}/reactions` - Toggle a reaction on a post

**Comments**
*   `GET /api/posts/{post}/comments` - List comments on a specific post
*   `POST /api/posts/{post}/comments` - Add a comment to a post
*   `PUT /api/posts/{post}/comments/{comment}` - Update a specific comment
*   `DELETE /api/posts/{post}/comments/{comment}` - Delete a comment

**Comment Reactions**
*   `GET /api/posts/{post}/comments/{comment}/reactions` - List reactions for a comment
*   `POST /api/posts/{post}/comments/{comment}/reactions` - Toggle a reaction on a comment

**References (Dictionaries/Enums)**
*   `GET /api/references/emojis` - Get list of supported emojis
*   `GET /api/references/provider-types` - Get authentication provider types

---

## 📖 Interactive API Documentation

This project uses **Scramble** to automatically generate OpenAPI documentation.

To view the interactive Swagger-like UI, simply visit the following URL in your browser while the server is running:

```
http://localhost:8000/docs/api
```

*(Note: If the application environment is set to `production`, Scramble documentation might be disabled by default for security reasons. Ensure `APP_ENV=local` in your `.env` file during development.)*

You can also export the OpenAPI schema manually by running:
```bash
php artisan scramble:export
```
This will generate an `api.json` file in the project root.