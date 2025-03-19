# Authentication Endpoints

## Verify Email
```
POST /api/v1/auth/verify-email
```

Sends a verification code to the user's email address.

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
  "message": "Verification code sent to email",
  "expiresIn": 360
}
```

## Register User
```
POST /api/v1/auth/register
```

Register a new user with verified email.

### Request
```json
{
  "email": "user@example.com",
  "firstName": "John",
  "lastName": "Doe",
  "verificationCode": "123456"
}
```

### Response
```json
{
  "success": true,
  "message": "User registered successfully",
  "userId": "user_123456",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

## Login with Email Verification
```
POST /api/v1/auth/login
```

Authenticate a user using email verification.

### Request (Step 1)
```json
{
  "email": "user@example.com"
}
```

### Response (Step 1)
```json
{
  "success": true,
  "message": "Verification code sent to email",
  "expiresIn": 360
}
```

### Request (Step 2)
```json
{
  "email": "user@example.com",
  "verificationCode": "123456"
}
```

### Response (Step 2)
```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiresIn": 86400
}
```

## Request 2FA Code for P2P Transaction
```
POST /api/v1/auth/request-2fa
```

Request a 2FA code via email for verifying a P2P transaction.

### Request
```json
{
  "transactionId": "tx_123456"
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

## Verify 2FA Code
```
POST /api/v1/auth/verify-2fa
```

Verify a 2FA code for a P2P transaction.

### Request
```json
{
  "transactionId": "tx_123456",
  "verificationCode": "123456"
}
```

### Response
```json
{
  "success": true,
  "message": "2FA verification successful",
  "transaction": {
    "id": "tx_123456",
    "status": "VERIFIED"
  }
}
```

## Resend Verification Code
```
POST /api/v1/auth/resend-verification
```

Resend a verification code to the user's email.

### Request
```json
{
  "email": "user@example.com",
  "purpose": "LOGIN" // Or "REGISTRATION" or "P2P_TRANSACTION"
}
```

### Response
```json
{
  "success": true,
  "message": "Verification code resent",
  "expiresIn": 360
}
```

## Verify Email Change
```
POST /api/v1/auth/verify-email-change
```

Verify a change of email address.

### Request (Step 1 - Request verification for new email)
```json
{
  "currentEmail": "user@example.com",
  "newEmail": "newemail@example.com"
}
```

### Response (Step 1)
```json
{
  "success": true,
  "message": "Verification code sent to new email",
  "expiresIn": 360
}
```

### Request (Step 2 - Submit verification code)
```json
{
  "currentEmail": "user@example.com",
  "newEmail": "newemail@example.com",
  "verificationCode": "123456"
}
```

### Response (Step 2)
```json
{
  "success": true,
  "message": "Email updated successfully"
}
``` 