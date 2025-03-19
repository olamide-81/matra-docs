# Getting Started with Matra

This internal documentation describes the user onboarding process for the Matra wallet application.

## App Distribution

- **iOS TestFlight**: https://testflight.apple.com/join/matra
- **Android Play Store**: https://play.google.com/store/apps/details?id=io.matra.wallet

## Installation Process

<div class="card-container">
  <div class="card">
    <h3 class="card-title">iOS Installation</h3>
    <ol>
      <li>User opens TestFlight on their iOS device</li>
      <li>User enters our invitation code or taps the invitation link</li>
      <li>User taps "Accept" to join the beta</li>
      <li>User taps "Install" to download the app</li>
    </ol>
  </div>
  <div class="card">
    <h3 class="card-title">Android Installation</h3>
    <ol>
      <li>User opens the Google Play Store</li>
      <li>User searches for "Matra Wallet"</li>
      <li>User taps "Install" to download the app</li>
      <li>Installation completes automatically</li>
    </ol>
  </div>
</div>

## User Onboarding Flow

<div class="steps">
  <div class="step">
    <h3>App Launch</h3>
    <p>User launches the Matra app for the first time.</p>
  </div>
  <div class="step">
    <h3>Email Collection</h3>
    <p>User is prompted to enter their email address.</p>
    <div class="callout note">
      <p>We only collect email for sending notifications and for 2FA verification during P2P transactions.</p>
    </div>
  </div>
  <div class="step">
    <h3>Email Verification</h3>
    <p>A verification code is sent to the user's email. The code expires after 6 minutes.</p>
    <p>User enters this verification code to validate their email ownership.</p>
  </div>
  <div class="step">
    <h3>Profile Setup</h3>
    <p>User enters their name to complete their profile.</p>
  </div>
  <div class="step">
    <h3>Wallet Creation</h3>
    <p>User follows the prompts to create their self-custodial wallet.</p>
    <p>For detailed wallet creation implementation, see <a href="creating-wallet.md">Creating a Wallet</a>.</p>
  </div>
</div>

## Recovery Phrase Security

<div class="callout danger">
  <p><strong>CRITICAL:</strong> Users must understand that their recovery phrase is the only way to recover their wallet if they lose access to their device. Matra cannot recover wallets for users who lose their recovery phrase.</p>
</div>

During wallet creation, users are shown a 12-word recovery phrase. The app instructs them to:

1. Write down the recovery phrase on paper in the correct order
2. Store this paper in a secure, waterproof, and fireproof location
3. Consider creating multiple backup copies stored in different secure locations
4. Never store the recovery phrase digitally (no photos, no digital documents, no cloud storage)
5. Never share the recovery phrase with anyone, including Matra support

## App Navigation Structure

After creating a wallet, users have access to these main interface areas:

<table>
  <thead>
    <tr>
      <th>Tab</th>
      <th>Features</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Home</td>
      <td>Wallet balance, recent transactions, and quick actions</td>
    </tr>
    <tr>
      <td>Wallet</td>
      <td>All tokens, send and receive transaction functions</td>
    </tr>
    <tr>
      <td>P2P</td>
      <td>P2P marketplace to buy and sell tokens</td>
    </tr>
    <tr>
      <td>Swap</td>
      <td>Token exchange through ChangeNOW integration</td>
    </tr>
    <tr>
      <td>Profile</td>
      <td>Account settings and support access</td>
    </tr>
  </tbody>
</table>

## Web Dashboard Authentication

Users can access our web interface through:

<div class="steps">
  <div class="step">
    <h3>Web Access</h3>
    <p>User visits web.matra.io in their browser.</p>
  </div>
  <div class="step">
    <h3>QR Code Authentication</h3>
    <p>User opens their Matra mobile app and scans the QR code displayed on the web dashboard.</p>
  </div>
  <div class="step">
    <h3>Authentication Confirmation</h3>
    <p>User confirms the connection request on their mobile device.</p>
    <div class="callout note">
      <p>This ensures only authorized users can access their wallet through the web interface.</p>
    </div>
  </div>
  <div class="step">
    <h3>Web Interface Access</h3>
    <p>After successful authorization, users can manage their wallet through the web interface.</p>
  </div>
</div>

<div class="callout tip">
  <p>The web dashboard maintains the same security level since private keys never leave the mobile device. The web interface uses secure communication with the mobile app to authorize transactions.</p>
</div>

## Transaction Processes

Users can perform these basic operations:

### Receiving Tokens

1. User navigates to the "Wallet" tab
2. User selects the token they want to receive
3. User taps "Receive"
4. User shares their wallet address or QR code with the sender

### Sending Tokens

1. User navigates to the "Wallet" tab
2. User selects the token they want to send
3. User taps "Send"
4. User enters the recipient's address (or scans their QR code)
5. User enters the amount to send
6. User reviews and confirms the transaction

## Related Documentation

- [Creating a Wallet](creating-wallet.md) - Detailed wallet creation process
- [P2P Transactions](p2p-transactions.md) - P2P marketplace functionality
- [Swapping Tokens](swapping-tokens.md) - Token exchange implementation
- [Security & 2FA](security-2fa.md) - Security features and 2FA implementation 