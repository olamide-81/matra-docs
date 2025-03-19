# Security & 2FA
Matra prioritizes the security of your assets with multiple layers of protection.

## Two-Factor Authentication (2FA)
Matra uses email-based two-factor authentication specifically for P2P transactions:

<div class="callout note">
  <p>Matra only collects your email for sending notifications and for 2FA verification during P2P transactions.</p>
</div>

### How 2FA Works for P2P Transactions

1. When you initiate a P2P transaction, a verification code is sent to your email.
2. Enter the verification code to authorize the transaction.
3. The transaction will not proceed until verified with the code.

*Note: Google Authenticator integration is coming soon as an optional additional security feature.*

## Securing Your Wallet

- **Recovery Phrase**: Always keep your 12-word recovery phrase secure and offline.
- **App PIN/Biometrics**: Set up a PIN code or use biometric authentication (fingerprint/Face ID) for app access.
- **Automatic Logout**: The app automatically logs out after a period of inactivity.

## Self-Custodial Security Model

Matra uses a self-custodial security model, which means:

1. Your private keys are stored only on your device.
2. Matra cannot access your funds or private keys.
3. You have complete control over your assets.
4. Recovery is only possible with your recovery phrase.

<div class="callout warning">
  <p>If you lose your recovery phrase, Matra cannot help you recover your wallet. There are no "forgot my recovery phrase" options in self-custodial wallets.</p>
</div>

## Secure Authentication Flow

When you first set up your Matra account:

1. You provide your email (used only for notifications and P2P transaction verification).
2. A verification code is sent to your email to confirm ownership.
3. After verification, your wallet is created.
4. You should immediately secure your recovery phrase.

## Security Best Practices
- Never share your recovery phrase or private keys with anyone.
- Be cautious of phishing attempts - always verify you're using the official Matra app.
- Enable all available security features on your device.
- Regularly update the app to ensure you have the latest security patches.
- Use the app's built-in browser for any crypto-related websites to avoid phishing.

## Device Security
Securing your mobile device is critical for wallet security:

- Enable screen lock on your device (PIN, pattern, biometrics).
- Keep your device's operating system and apps updated.
- Be cautious about which apps you install.
- Consider using a dedicated device for substantial crypto holdings.

## Transaction Security
Matra implements multiple security measures for transactions:

- Double confirmation for all outgoing transactions.
- Email 2FA for P2P transactions.
- Clear transaction details before confirmation.
- Transaction limits to reduce risk.

<div class="callout tip">
  <p>Always double-check the recipient address before confirming any transaction. Even a single incorrect character can result in permanent loss of funds.</p>
</div> 