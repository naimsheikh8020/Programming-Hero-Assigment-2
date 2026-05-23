# DevPulse API

An issue tracking API built with Express.js and TypeScript for managing bug reports and feature requests with role-based access control.

## 🌐 Live URL

[https://programming-hero-assigment-2.vercel.app/](https://programming-hero-assigment-2.vercel.app/)

## ✨ Features

- **User Authentication**: Secure signup and login with JWT tokens
- **Role-Based Access Control**: Contributor and Maintainer roles for permission management
- **Issue Tracking**: Create, read, update, and delete issues
- **Issue Classification**: Support for bug and feature request issue types
- **Issue Status Management**: Track issues through open, in_progress, and resolved states
- **Input Validation**: Comprehensive validation for all API requests
- **Error Handling**: Global error handling middleware with consistent error responses
- **Database Persistence**: PostgreSQL for reliable data storage

## 🛠️ Tech Stack

- **Runtime**: Node.js
- **Backend Framework**: Express.js 5.2.1
- **Language**: TypeScript 6.0.3
- **Database**: PostgreSQL
- **Authentication**: JWT (JSON Web Tokens)
- **Password Hashing**: bcrypt
- **Development**: tsx (TypeScript executor)
- **Package Manager**: npm

## 📋 Setup Steps

### Prerequisites

- Node.js (v18 or higher)
- PostgreSQL (v12 or higher)
- npm or yarn

### Installation

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd assigment-02
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Create environment file**

   ```bash
   touch .env
   ```

4. **Configure environment variables**

   ```env
   DATABASE_URL=postgresql://username:password@localhost:5432/devpulse_db
   PORT=5000
   JWT_SECRET=your_jwt_secret_key_here
   NODE_ENV=development
   ```

5. **Set up the database**
   - Create a PostgreSQL database named `devpulse_db`
   - The tables will be created automatically when the server starts

6. **Start the development server**
   ```bash
   npm run dev
   ```

The API will be available at `http://localhost:5000`

## 📡 API Endpoints

### Base URL

```
/api
```

### Authentication Endpoints

| Method | Endpoint       | Description                             | Auth Required |
| ------ | -------------- | --------------------------------------- | ------------- |
| POST   | `/auth/signup` | Register a new user                     | No            |
| POST   | `/auth/login`  | Authenticate user and receive JWT token | No            |

**Signup Request Body:**

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securePassword123",
  "role": "contributor"
}
```

**Login Request Body:**

```json
{
  "email": "john@example.com",
  "password": "securePassword123"
}
```

### Issue Management Endpoints

| Method | Endpoint      | Description               | Auth Required |
| ------ | ------------- | ------------------------- | ------------- |
| GET    | `/issues`     | Retrieve all issues       | No            |
| GET    | `/issues/:id` | Retrieve a specific issue | No            |
| POST   | `/issues`     | Create a new issue        | Yes           |
| PATCH  | `/issues/:id` | Update an existing issue  | Yes           |
| DELETE | `/issues/:id` | Delete an issue           | Yes           |

**Create Issue Request Body:**

```json
{
  "title": "Login button not responsive",
  "description": "The login button on the homepage is not responding to clicks on mobile devices.",
  "type": "bug"
}
```

**Update Issue Request Body:**

```json
{
  "title": "Updated title (optional)",
  "description": "Updated description (optional)",
  "type": "bug or feature_request (optional)",
  "status": "open, in_progress, or resolved (optional)"
}
```

**Headers for Authenticated Requests:**

```
Authorization: Bearer <jwt_token>
```

## 🗄️ Database Schema

### Users Table

| Column     | Type         | Constraints                                              |
| ---------- | ------------ | -------------------------------------------------------- |
| id         | SERIAL       | PRIMARY KEY, Auto-increment                              |
| name       | VARCHAR(100) | NOT NULL                                                 |
| email      | VARCHAR(255) | UNIQUE, NOT NULL                                         |
| password   | TEXT         | NOT NULL (bcrypt hashed)                                 |
| role       | VARCHAR(20)  | DEFAULT 'contributor', CHECK (contributor \| maintainer) |
| created_at | TIMESTAMP    | DEFAULT CURRENT_TIMESTAMP                                |
| updated_at | TIMESTAMP    | DEFAULT CURRENT_TIMESTAMP                                |

### Issues Table

| Column      | Type         | Constraints                                             |
| ----------- | ------------ | ------------------------------------------------------- |
| id          | SERIAL       | PRIMARY KEY, Auto-increment                             |
| title       | VARCHAR(150) | NOT NULL                                                |
| description | TEXT         | NOT NULL, MIN LENGTH 20 characters                      |
| type        | VARCHAR(30)  | NOT NULL, CHECK (bug \| feature_request)                |
| status      | VARCHAR(30)  | DEFAULT 'open', CHECK (open \| in_progress \| resolved) |
| reporter_id | INTEGER      | NOT NULL, Foreign Key → users(id)                       |
| created_at  | TIMESTAMP    | DEFAULT CURRENT_TIMESTAMP                               |
| updated_at  | TIMESTAMP    | DEFAULT CURRENT_TIMESTAMP                               |

## 🔐 Authentication

The API uses JWT (JSON Web Tokens) for authentication. After successful login, clients receive a token that must be included in the `Authorization` header as a Bearer token for protected endpoints.

## 📝 Scripts

- `npm run dev` - Start the development server with hot reload
- `npm test` - Run tests (not yet configured)

## 📂 Project Structure

```
src/
├── app.ts                    # Express app configuration
├── server.ts                 # Server entry point
├── api/
│   ├── controller/          # Request handlers
│   ├── routes/              # Route definitions
│   └── service/             # Business logic
├── config/
│   └── db.ts                # Database configuration
├── db/
│   ├── userTable.ts         # Users table schema
│   └── issueTable.ts        # Issues table schema
├── middleware/              # Custom middleware
├── types/                   # TypeScript type definitions
└── utils/                   # Utility functions
```

## 🚀 Deployment

The API is deployed on Vercel. To deploy your own instance:

1. Push your code to a Git repository (GitHub, GitLab, etc.)
2. Connect your repository to Vercel
3. Set up environment variables in Vercel dashboard
4. Deploy with a single click

## 📄 License

ISC

## 👨‍💻 Author

Created as part of Programming Hero Assignment 02

---

For more information, visit the [live API](https://programming-hero-assigment-2.vercel.app/)
