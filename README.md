# Task Tracker Laravel Backend

A Laravel-based REST API backend for the Task Tracker application with JWT authentication and Google OAuth support.

## Features

- **JWT Authentication**: Secure token-based authentication
- **Google OAuth**: Social login with Google
- **Task Management**: Full CRUD operations for tasks
- **User Management**: User registration and profile management
- **API Endpoints**: RESTful API design

## Requirements

- PHP 8.1 or higher
- Composer
- SQLite (or MySQL/PostgreSQL)
- Google OAuth credentials (for Google login)

## Installation

1. **Clone the repository and navigate to the backend directory:**
   ```bash
   cd task-tracker-backend
   ```

2. **Install dependencies:**
   ```bash
   composer install
   ```

3. **Copy environment file:**
   ```bash
   cp .env.example .env
   ```

4. **Generate application key:**
   ```bash
   php artisan key:generate
   ```

5. **Generate JWT secret:**
   ```bash
   php artisan jwt:secret
   ```

6. **Configure database in `.env`:**
   ```env
   DB_CONNECTION=sqlite
   DB_DATABASE=/absolute/path/to/database.sqlite
   ```

7. **Run migrations:**
   ```bash
   php artisan migrate
   ```

8. **Configure Google OAuth (optional):**
   Add your Google OAuth credentials to `.env`:
   ```env
   GOOGLE_CLIENT_ID=your_google_client_id
   GOOGLE_CLIENT_SECRET=your_google_client_secret
   GOOGLE_REDIRECT_URI=http://localhost:8000/api/auth/google/callback
   ```

9. **Start the development server:**
   ```bash
   php artisan serve
   ```

## API Endpoints

### Authentication Endpoints

#### Register User
```
POST /api/register
Content-Type: application/json

{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "password_confirmation": "password123"
}
```

#### Login
```
POST /api/login
Content-Type: application/json

{
    "email": "john@example.com",
    "password": "password123"
}
```

#### Google OAuth
```
GET /api/auth/google
```
Returns a redirect URL for Google OAuth.

```
GET /api/auth/google/callback
```
Handles Google OAuth callback and returns JWT token.

#### Logout
```
POST /api/logout
Authorization: Bearer {token}
```

#### Refresh Token
```
POST /api/refresh
Authorization: Bearer {token}
```

#### Get User Profile
```
GET /api/user-profile
Authorization: Bearer {token}
```

### Task Endpoints

All task endpoints require authentication with JWT token.

#### Get All Tasks
```
GET /api/tasks
Authorization: Bearer {token}
```

#### Create Task
```
POST /api/tasks
Authorization: Bearer {token}
Content-Type: application/json

{
    "title": "Complete project",
    "description": "Finish the task tracker project",
    "status": "pending",
    "due_date": "2024-12-31 23:59:59"
}
```

#### Get Single Task
```
GET /api/tasks/{id}
Authorization: Bearer {token}
```

#### Update Task
```
PUT /api/tasks/{id}
Authorization: Bearer {token}
Content-Type: application/json

{
    "title": "Updated task title",
    "status": "in_progress"
}
```

#### Delete Task
```
DELETE /api/tasks/{id}
Authorization: Bearer {token}
```

## Task Status Values

- `pending`: Task is not started
- `in_progress`: Task is currently being worked on
- `completed`: Task is finished

## Response Format

All API responses follow this format:

```json
{
    "message": "Success message",
    "data": {
        // Response data
    }
}
```

## Error Handling

The API returns appropriate HTTP status codes:

- `200`: Success
- `201`: Created
- `400`: Bad Request
- `401`: Unauthorized
- `404`: Not Found
- `422`: Validation Error
- `500`: Server Error

## Security Features

- JWT token-based authentication
- Password hashing using bcrypt
- Input validation and sanitization
- CORS protection
- Rate limiting (can be configured)

## Database Schema

### Users Table
- `id` (Primary Key)
- `name` (String)
- `email` (String, Unique)
- `password` (Hashed)
- `email_verified_at` (Timestamp)
- `created_at` (Timestamp)
- `updated_at` (Timestamp)

### Tasks Table
- `id` (Primary Key)
- `title` (String)
- `description` (Text, Nullable)
- `status` (Enum: pending, in_progress, completed)
- `due_date` (DateTime, Nullable)
- `user_id` (Foreign Key to users)
- `created_at` (Timestamp)
- `updated_at` (Timestamp)

## Testing

Run the test suite:
```bash
php artisan test
```

## Deployment

1. Set up your production environment
2. Configure your web server (Apache/Nginx)
3. Set proper environment variables
4. Run migrations
5. Configure Google OAuth redirect URIs for production

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

## License

This project is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
