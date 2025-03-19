# Authentication Endpoints

## Register User
```
POST /api/v1/auth/register
```

Register a new user with email and password.

### Request
```json
{
  "email": "user@example.com",
  "password": "securePassword123!",
  "firstName": "John",
  "lastName": "Doe"
}
```

### Response
```json
{
  "success": true,
  "message": "User registered successfully",
  "userId": "user_123456",
  "verificationEmailSent": true
}
```

## Login
```
POST /api/v1/auth/login
```

Authenticate a user and get access token.

### Request
```json
{
  "email": "user@example.com",
  "password": "securePassword123!"
}
```

### Response
```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiresIn": 86400
}
```

## Verify Email
```
GET /api/v1/auth/verify-email/:token
```

Verify user email with token sent after registration.

### Parameters
- `token`: Verification token sent via email

### Response
```json
{
  "success": true,
  "message": "Email verified successfully"
}
```

## Request Password Reset
```
POST /api/v1/auth/forgot-password
```

Request password reset email.

### Request
```json
{
  "email": "user@example.com"
}
```

### Response
```json
{
  "success": true,
  "message": "Password reset email sent"
}
```

## Reset Password
```
POST /api/v1/auth/reset-password
```

Reset password with token.

### Request
```json
{
  "token": "reset_token_123",
  "newPassword": "newSecurePassword456!"
}
```

### Response
```json
{
  "success": true,
  "message": "Password reset successful"
}
```

## Request 2FA Code
```
POST /api/v1/auth/request-2fa
```

Request a 2FA code via email.

### Request
```json
{
  "email": "user@example.com"
}
```

### Response
```json
{
  "success": true,
  "message": "2FA code sent to email",
  "expiresIn": 300
}
``` 