# Moviereviewplatform-React-js-Node-js-Mongodb

This repository contains a Movie Review Platform built with React (frontend), Node.js/Express (backend), and MongoDB (database). Below you’ll find a comprehensive guide to set up, run, and understand the project, including setup, API documentation, database requirements, environment variables, and design decisions.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Setup & Installation](#setup--installation)
3. [Environment Variables](#environment-variables)
4. [Running the Application](#running-the-application)
5. [Database Setup](#database-setup)
6. [API Documentation](#api-documentation)
7. [Project Structure](#project-structure)
8. [Authentication & Security](#authentication--security)
9. [Development & Build](#development--build)
10. [Testing](#testing)
11. [Design Decisions](#design-decisions)
12. [Notes & Troubleshooting](#notes--troubleshooting)
13. [Contributing](#contributing)
14. [Versioning](#versioning)
15. [License](#license)

---

## Project Overview

- **Frontend**: React (client-side rendering or SPA)
- **Backend**: Node.js with Express
- **Database**: MongoDB (via Mongoose)
- **Authentication**: JWT-based tokens
- **Primary Entities**: User, Movie, Review, (optional) Comment, Genre/Category
- **API Style**: RESTful endpoints with JSON payloads

This README provides a quickstart path as well as a deeper API reference. Replace placeholder values (e.g., domain, ports) with your actual deployment details.

---

## Setup & Installation

### Prerequisites

- Node.js (≥ 14.x or as required by the project)
- npm (or yarn)
- MongoDB (local or cloud, e.g., MongoDB Atlas)
- Git

### Clone the Repository

```bash
git clone https://github.com/your-org/Moviereviewplatform-React-js-Node-js-Mongodb.git
cd Moviereviewplatform-React-js-Node-js-Mongodb
```

### Backend Setup (Node.js / Express)

1. Navigate to the backend directory (adjust if your structure is monorepo)
   ```bash
   cd backend            # or server
   ```
2. Install dependencies
   ```bash
   npm install           # or yarn install
   ```
3. Create and configure environment variables (see next section)
4. Start in development
   ```bash
   npm run dev           # or nodemon src/server.js (depending on your setup)
   ```
5. (Optional) Build for production
   ```bash
   npm run build
   npm start
   ```

### Frontend Setup (React)

1. Navigate to the frontend directory
   ```bash
   cd frontend
   ```
2. Install dependencies
   ```bash
   npm install           # or yarn install
   ```
3. Start development server
   ```bash
   npm start
   ```
4. Open the app in your browser
   - http://localhost:3000 (default, may vary)

> If your repo uses a monorepo or a single repo with separate folders (e.g., `client` and `api`), adapt the paths accordingly.

---

## Environment Variables

Create an environment file for the backend (e.g., `.env`) with the required variables. Below are common variables you may need; adjust names to match your code.

### Required

- `PORT` - Backend server port (default often 5000)
- `MONGODB_URI` - MongoDB connection string, e.g. `mongodb://localhost:27017/moviedb` or a Atlas URI
- `JWT_SECRET` - Secret key for signing JWT tokens
- `JWT_EXPIRES_IN` - Token expiry (e.g., `1h`, `24h`)
- `NODE_ENV` - Environment (development|production)
- `CORS_ORIGIN` - Allowed origin for frontend during development (e.g., `http://localhost:3000`)

### Optional / Common

- `JWT_REFRESH_SECRET` - Secret for refresh tokens (if implementing refresh flow)
- `EMAIL_SERVICE_HOST`, `EMAIL_SERVICE_USER`, `EMAIL_SERVICE_PASS` - For any email verification or notification features
- `S3_BUCKET_NAME` or `CLOUD_STORAGE_URL` - For poster uploads (if using cloud storage)
- `REDIS_URL` - For caching/session management (optional)

### Example `.env` (backend)

```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/moviedb
JWT_SECRET=your_jwt_secret_here
JWT_EXPIRES_IN=1h
NODE_ENV=development
CORS_ORIGIN=http://localhost:3000
```

> If you have a separate config system, place these values accordingly and ensure your app loads them at startup.

---

## Running the Application

- Ensure MongoDB is running and accessible via the `MONGODB_URI`.
- Start the backend and frontend in their respective folders as described above.
- Verify the API is reachable (e.g., `GET http://localhost:5000/api/status` or similar health check endpoint if provided).

---

## Database Setup

### Local MongoDB

- Install MongoDB locally and run the daemon (e.g., `mongod`).
- Create the database name according to your `MONGODB_URI` (e.g., `moviedb`).

### Seed Data (optional)

- If your project includes a seed script, run:
  ```bash
  node scripts/seed.js        # or npm run seed
  ```
- Seed data typically includes:
  - Admin user
  - A few sample movies and genres
  - Sample reviews

### Migrations

- If your project uses migrations, ensure you run them as part of deployment (e.g., `npm run migrate`).

---

## API Documentation

Below is a comprehensive API reference template. Replace placeholder schemas and endpoints with your actual implementation. The structure follows typical RESTful patterns.

Note: All endpoints expect JSON requests and responses. Use `Authorization: Bearer <token>` for protected routes.

### Authentication

- Register
  - POST `/api/auth/register`
  - Request:
    ```json
    {
      "name": "John Doe",
      "email": "john@example.com",
      "password": "P@ssword123",
      "username": "johndoe"
    }
    ```
  - Response (201):
    ```json
    {
      "user": { "id": "...", "name": "...", "email": "...", "username": "..." },
      "token": "...",
      "expiresIn": 3600
    }
    ```
  - Errors: 400, 409

- Login
  - POST `/api/auth/login`
  - Request:
    ```json
    {
      "email": "john@example.com",
      "password": "P@ssword123"
    }
    ```
  - Response (200):
    ```json
    {
      "token": "...",
      "user": { "id": "...", "name": "...", "email": "...", "username": "..." },
      "expiresIn": 3600
    }
    ```
  - Errors: 401

- Refresh Token
  - POST `/api/auth/refresh`
  - Request:
    ```json
    {
      "refreshToken": "..."
    }
    ```
  - Response: new `token` and `expiresIn`

### Movies

- List Movies
  - GET `/api/movies`
  - Query: `page`, `limit`, `search`, `genre`, `sort`
  - Response:
    ```json
    {
      "page": 1,
      "limit": 20,
      "total": 123,
      "movies": [
        {
          "id": "...",
          "title": "...",
          "description": "...",
          "releaseDate": "...",
          "genres": ["Action","Adventure"],
          "rating": 4.5,
          "posterUrl": "...",
          "director": "..."
        }
      ]
    }
    ```

- Get Movie Details
  - GET `/api/movies/{id}`
  - Response:
    ```json
    {
      "id": "...",
      "title": "...",
      "description": "...",
      "releaseDate": "...",
      "genres": ["..."],
      "rating": 4.5,
      "posterUrl": "...",
      "director": "...",
      "cast": ["...", "..."],
      "runtime": 120,
      "reviewsCount": 12
    }
    ```
- Create Movie (Admin)
  - POST `/api/movies`
  - Headers: `Authorization: Bearer <token>`
  - Request:
    ```json
    {
      "title": "...",
      "description": "...",
      "releaseDate": "YYYY-MM-DD",
      "genres": ["Action"],
      "director": "...",
      "cast": ["...", "..."],
      "runtime": 130,
      "posterUrl": "https://..."
    }
    ```
  - Response (201):
    ```json
    { "movie": { /* created movie */ } }
    ```
  - Permissions: admin

- Update Movie
  - PUT `/api/movies/{id}`
  - Request: fields to update
  - Response: 200 with updated movie
  - Permissions: admin

- Delete Movie
  - DELETE `/api/movies/{id}`
  - Response: 204
  - Permissions: admin

### Reviews

- List Reviews for a Movie
  - GET `/api/movies/{id}/reviews`
  - Pagination
  - Response: list of reviews with user info

- Create Review
  - POST `/api/movies/{id}/reviews`
  - Auth required
  - Request:
    ```json
    {
      "rating": 4,
      "text": "Great movie with stunning visuals."
    }
    ```
  - Response: 201 with created review

- Get Review
  - GET `/api/reviews/{id}`

- Update Review
  - PUT `/api/reviews/{id}`
  - Auth required and ownership or admin
  - Request: partial fields (e.g., `rating`, `text`)

- Delete Review
  - DELETE `/api/reviews/{id}`
  - Auth required and ownership or admin
  - Response: 204

### Users

- Get Current User
  - GET `/api/users/me`
  - Auth required

- Update User
  - PUT `/api/users/{id}`
  - Auth required, must be same user or admin
  - Request: profile fields

- Get User by ID
  - GET `/api/users/{id}`

- Favorites / Watchlist (optional)
  - POST `/api/users/me/favorites/{movieId}`
  - DELETE `/api/users/me/favorites/{movieId}`
  - GET `/api/users/me/favorites`

### Admin

- List Users
  - GET `/api/admin/users`
  - Query: `page`, `limit`, `role`

- Update User Role
  - PUT `/api/admin/users/{id}/role`
  - Request: `{ "role": "admin" }`

- Manage Genres
  - GET `/api/admin/genres`
  - POST `/api/admin/genres` with `{ "name": "Sci-Fi" }`
  - PUT `/api/admin/genres/{id}`
  - DELETE `/api/admin/genres/{id}`

---

## Design Decisions

- **API-first approach**: Clean, versioned REST endpoints with JSON payloads.
- **JWT-based authentication**: Stateless authentication suitable for scalable distributed systems.
- **MongoDB with Mongoose**: Flexible schemas for users, movies, reviews; easy to evolve data models.
- **Pagination defaults**: Prevent large payloads; client can override with `page`/`limit`.
- **Role-based access control**: Admin/Moderator protections for sensitive operations (movie creation, updates, deletions, user management).
- **Separation of concerns**: Distinct layers for routes, controllers, services, and models to improve maintainability.
- **Extensibility**: Optional features like comments, watchlists, and genres implemented as separate collections with references.

---

## Notes & Troubleshooting

- If you encounter connection errors to MongoDB:
  - Verify `MONGODB_URI` in your `.env`
  - Ensure MongoDB is running and reachable from the host
- If you see CORS issues during frontend-backend communication:
  - Confirm `CORS_ORIGIN` is set correctly
  - Ensure your backend uses proper CORS middleware configuration
- For token-related errors:
  - Check `JWT_SECRET` and token expiry settings
  - Ensure the client stores and sends the token in `Authorization: Bearer <token>`

---

## Contributing

- Fork the repository
- Create a feature branch: `git checkout -b feature/awesome-feature`
- Implement, test locally
- Open a Pull Request with a clear description of changes
- Ensure test coverage where applicable

---

## Versioning

- This project follows semantic versioning for the API and app. If you implement breaking changes, increment the major/minor version in the API docs and update changelog accordingly.

---

## Contact

- bhavya sree uppaluru
- Email : bhavyasreereddy371@gmail.com
- GitHub : https://github.com/bhavya371/Moviereviewplatform-React-js-Node-js-Mongodb

