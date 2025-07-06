# Google Auth API

A .NET 9 Web API project with Google OAuth login, JWT authentication, and ASP.NET Core Identity.

## Features

- ✅ .NET 9 Web API
- ✅ ASP.NET Core Identity for user management
- ✅ JWT Authentication
- ✅ Google OAuth Login
- ✅ SQLite Database (Entity Framework Core)
- ✅ Protected API endpoints
- ✅ Swagger/OpenAPI documentation

## Prerequisites

- .NET 9 SDK
- Google Cloud Console account (for OAuth credentials)

## Setup Instructions

### 1. Google OAuth Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing one
3. Enable Google+ API
4. Go to "Credentials" → "Create Credentials" → "OAuth 2.0 Client IDs"
5. Set Application Type to "Web application"
6. Add authorized redirect URIs:
   - `https://localhost:7001/signin-google` (for HTTPS)
   - `http://localhost:5001/signin-google` (for HTTP)
7. Copy Client ID and Client Secret

### 2. Update Configuration

Edit `appsettings.json` and replace:
- `YOUR_GOOGLE_CLIENT_ID` with your actual Google Client ID
- `YOUR_GOOGLE_CLIENT_SECRET` with your actual Google Client Secret
- Update the JWT key with a secure random string

### 3. Run the Application

```bash
dotnet run
```

The API will be available at:
- Swagger UI: `https://localhost:7001/swagger`
- API Base: `https://localhost:7001/api`

## API Endpoints

### Authentication Endpoints

#### Register User
```
POST /api/auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "Password123!",
  "confirmPassword": "Password123!",
  "firstName": "John",
  "lastName": "Doe"
}
```

#### Login
```
POST /api/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "Password123!"
}
```

#### Google Login
```
GET /api/auth/google-login
```
This will redirect to Google OAuth consent screen.

#### Google Callback
```
GET /api/auth/google-callback
```
This endpoint handles the OAuth callback from Google.

#### Get Profile
```
GET /api/auth/profile
Authorization: Bearer {jwt_token}
```

#### Logout
```
POST /api/auth/logout
Authorization: Bearer {jwt_token}
```

### Protected Endpoints

#### Get Current User
```
GET /api/user/me
Authorization: Bearer {jwt_token}
```

#### Get Protected Data
```
GET /api/user/protected-data
Authorization: Bearer {jwt_token}
```

## Authentication Flow

### 1. Regular Login Flow
1. User registers with email/password
2. User logs in with credentials
3. API returns JWT token
4. Use JWT token in Authorization header for protected endpoints

### 2. Google OAuth Flow
1. User clicks "Login with Google"
2. Redirected to Google OAuth consent screen
3. User authorizes the application
4. Google redirects back to callback endpoint
5. API creates/updates user and returns JWT token
6. Use JWT token in Authorization header for protected endpoints

## Database

The application uses SQLite with Entity Framework Core. The database file (`app.db`) will be created automatically when you first run the application.

### User Table Structure
- Id (Primary Key)
- UserName
- Email
- FirstName
- LastName
- ProfilePicture
- CreatedAt
- LastLoginAt
- EmailConfirmed
- PhoneNumber
- PhoneNumberConfirmed
- TwoFactorEnabled
- LockoutEnd
- LockoutEnabled
- AccessFailedCount

## JWT Token Structure

The JWT token contains the following claims:
- `nameid`: User ID
- `email`: User email
- `name`: Full name
- `firstName`: First name
- `lastName`: Last name
- `profilePicture`: Profile picture URL
- `role`: User roles (if any)

## Testing with Swagger

1. Open `https://localhost:7001/swagger`
2. Test the `/api/auth/register` endpoint to create a user
3. Test the `/api/auth/login` endpoint to get a JWT token
4. Click the "Authorize" button in Swagger
5. Enter your JWT token in the format: `Bearer {your_token}`
6. Test protected endpoints

## Security Notes

- Change the JWT key in production
- Use HTTPS in production
- Implement proper password policies
- Add rate limiting
- Consider adding refresh tokens
- Implement proper error handling
- Add logging and monitoring

## Troubleshooting

### Common Issues

1. **Google OAuth Error**: Make sure redirect URIs are correctly configured in Google Cloud Console
2. **Database Error**: Ensure the application has write permissions in the project directory
3. **JWT Token Error**: Verify the JWT key in appsettings.json is at least 32 characters long

### Development Tips

- Use Postman or similar tool to test API endpoints
- Check browser developer tools for OAuth redirect issues
- Monitor application logs for detailed error messages 