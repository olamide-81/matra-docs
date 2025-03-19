# Security & Two-Factor Authentication

This internal documentation describes the security features and two-factor authentication implementation in our application.

## Two-Factor Authentication (2FA)

Our application uses email-based 2FA specifically for P2P transactions:

- Email verification codes are sent only when users initiate P2P transactions
- Codes have a limited validity period
- Each code can only be used once

### How 2FA Works for P2P Transactions

1. User initiates a P2P transaction
2. System sends a verification code to the user's registered email
3. User enters the verification code in the app
4. System validates the code
5. If valid, the P2P transaction is processed

## Self-Custodial Security Model

Our application follows a self-custodial security model:

- Users have complete control over their private keys
- Private keys never leave the user's device
- All transactions require user authorization
- Users are responsible for securely storing their recovery phrase

<div class="callout warning">
  <p><strong>Warning:</strong> If a user loses their recovery phrase, there is no way to recover their wallet or funds. There is no password reset option or backup access.</p>
</div>

## Security Best Practices

We recommend the following security practices to users:

- Store recovery phrase securely offline
- Enable device-level security (biometrics, PIN)
- Never share recovery phrase with anyone
- Be cautious of phishing attempts
- Verify transaction details before confirming
- Keep the app updated to the latest version

## Device Security

The application security is enhanced with device-level security features:

- Biometric authentication (where available)
- Local data encryption
- Automatic session timeout
- No sensitive data stored in the device clipboard

## Transaction Security

Additional security measures for transactions include:

- Double confirmation for transactions
- Email 2FA for P2P transactions
- Transaction amount limits
- Clearly displayed transaction fees and details 