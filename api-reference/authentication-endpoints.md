# Authentication Endpoints

## Verify Email
```
POST /auth/verify-email
```

Sends a verification code to the user's email address.

### Request
```json
{
  "email": "[user email]"
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
POST /auth/register
```

Register a new user with verified email.

### Request
```json
{
  "email": "[user email]",
  "firstName": "[first name]",
  "lastName": "[last name]",
  "verificationCode": "[code]"
}
```

### Response
```json
{
  "success": true,
  "message": "User registered successfully",
  "userId": "[user ID]",
  "token": "[authentication token]"
}
```

## Login with Email Verification
```
POST /auth/login
```

Authenticate a user using email verification.

### Request (Step 1)
```json
{
  "email": "[user email]"
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
  "email": "[user email]",
  "verificationCode": "[code]"
}
```

### Response (Step 2)
```json
{
  "success": true,
  "token": "[authentication token]",
  "refreshToken": "[refresh token]",
  "expiresIn": 86400
}
```

## Request 2FA Code for P2P Transaction
```
POST /auth/request-2fa
```

Request a 2FA code via email for verifying a P2P transaction.

### Request
```json
{
  "transactionId": "[transaction ID]"
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
POST /auth/verify-2fa
```

Verify a 2FA code for a P2P transaction.

### Request
```json
{
  "transactionId": "[transaction ID]",
  "verificationCode": "[code]"
}
```

### Response
```json
{
  "success": true,
  "message": "2FA verification successful",
  "transaction": {
    "id": "[transaction ID]",
    "status": "VERIFIED"
  }
}
```

## Resend Verification Code
```
POST /auth/resend-verification
```

Resend a verification code to the user's email.

### Request
```json
{
  "email": "[user email]",
  "purpose": "[verification purpose]"
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
POST /auth/verify-email-change
```

Verify a change of email address.

### Request (Step 1 - Request verification for new email)
```json
{
  "currentEmail": "[current email]",
  "newEmail": "[new email]"
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
  "currentEmail": "[current email]",
  "newEmail": "[new email]",
  "verificationCode": "[code]"
}
```

### Response (Step 2)
```json
{
  "success": true,
  "message": "Email updated successfully"
}
``` 