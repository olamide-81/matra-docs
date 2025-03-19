# API Endpoints (Postman Collection)

This documentation covers the endpoints available in the Tradematra V1 Postman collection.

## Verification Endpoints

<div class="api-endpoint">
  <div class="api-endpoint-header">
    <span class="api-endpoint-method post-method">POST</span>
    <span class="api-endpoint-path">/user/verify</span>
  </div>
  <div class="api-endpoint-body">
    <h3>Send Verification Email</h3>
    <p>Sends a verification code to the user's email that expires after 6 minutes. You can request another token after 1 minute of requesting the previous code.</p>
    
    <h4>Request Body</h4>
    
```json
{
  "email": "user@example.com",
  "referral": "2930291"  // Optional
}
```

  </div>
</div>

<div class="api-endpoint">
  <div class="api-endpoint-header">
    <span class="api-endpoint-method post-method">POST</span>
    <span class="api-endpoint-path">/user/verify/resend</span>
  </div>
  <div class="api-endpoint-body">
    <h3>Resend Verification Code</h3>
    <p>Resends the verification code to the user's email.</p>
  </div>
</div>

## Authentication Endpoints

<div class="api-endpoint">
  <div class="api-endpoint-header">
    <span class="api-endpoint-method post-method">POST</span>
    <span class="api-endpoint-path">/user/register</span>
  </div>
  <div class="api-endpoint-body">
    <h3>User Registration</h3>
    <p>Registers a new user account.</p>
    
    <h4>Request Body</h4>
    
```json
{
  "email": "user@example.com",
  "password": "securePassword123!",
  "firstName": "John",
  "lastName": "Doe",
  "verificationCode": "123456"
}
```

    <h4>Response</h4>
    
```json
{
  "success": true,
  "message": "User registered successfully",
  "userId": "user_123456"
}
```
  </div>
</div>

<div class="api-endpoint">
  <div class="api-endpoint-header">
    <span class="api-endpoint-method post-method">POST</span>
    <span class="api-endpoint-path">/user/login</span>
  </div>
  <div class="api-endpoint-body">
    <h3>User Login</h3>
    <p>Authenticates a user and returns an access token.</p>
    
    <h4>Request Body</h4>
    
```json
{
  "email": "user@example.com",
  "password": "securePassword123!"
}
```

    <h4>Response</h4>
    
```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "user_123456",
    "email": "user@example.com",
    "firstName": "John",
    "lastName": "Doe"
  }
}
```
  </div>
</div>

## Wallet Endpoints

<div class="api-endpoint">
  <div class="api-endpoint-header">
    <span class="api-endpoint-method post-method">POST</span>
    <span class="api-endpoint-path">/wallet/create</span>
  </div>
  <div class="api-endpoint-body">
    <h3>Create Wallet</h3>
    <p>Creates a new self-custodial wallet.</p>
    
    <h4>Request Body</h4>
    
```json
{
  "walletName": "My Main Wallet"
}
```

    <h4>Response</h4>
    
```json
{
  "success": true,
  "walletId": "wallet_789012",
  "address": "0x1234567890abcdef1234567890abcdef12345678",
  "recoveryPhrase": "word1 word2 word3 word4 word5 word6 word7 word8 word9 word10 word11 word12",
  "message": "Store your recovery phrase securely. It will not be shown again."
}
```

    <div class="callout warning">
      <p>Always store the recovery phrase in a secure, offline location. Matra cannot recover your wallet if you lose your recovery phrase.</p>
    </div>
  </div>
</div>

<div class="api-endpoint">
  <div class="api-endpoint-header">
    <span class="api-endpoint-method get-method">GET</span>
    <span class="api-endpoint-path">/wallet/balance</span>
  </div>
  <div class="api-endpoint-body">
    <h3>Get Wallet Balance</h3>
    <p>Retrieves the balance of all tokens in the wallet.</p>
    
    <h4>Headers</h4>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Value</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Authorization</td>
          <td>Bearer {token}</td>
          <td>JWT token received after login</td>
        </tr>
      </tbody>
    </table>
    
    <h4>Response</h4>
    
```json
{
  "success": true,
  "balances": [
    {
      "token": "BTC",
      "balance": "0.01234567",
      "usdValue": 458.32
    },
    {
      "token": "ETH",
      "balance": "1.23456789",
      "usdValue": 1892.67
    },
    {
      "token": "USDT",
      "balance": "1000.00",
      "usdValue": 1000.00
    }
  ],
  "totalUsdValue": 3350.99
}
```
  </div>
</div>

<div class="api-endpoint">
  <div class="api-endpoint-header">
    <span class="api-endpoint-method post-method">POST</span>
    <span class="api-endpoint-path">/wallet/send</span>
  </div>
  <div class="api-endpoint-body">
    <h3>Send Transaction</h3>
    <p>Initiates a transaction to send tokens to another address.</p>
    
    <h4>Request Body</h4>
    
```json
{
  "token": "ETH",
  "amount": "0.5",
  "toAddress": "0x9876543210abcdef9876543210abcdef98765432",
  "memo": "Payment for services",
  "gasPrice": "standard"
}
```

    <h4>Response</h4>
    
```json
{
  "success": true,
  "transactionId": "tx_234567",
  "status": "PENDING",
  "txHash": "0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
  "estimatedCompletionTime": 30
}
```

    <div class="callout note">
      <p>Transaction times may vary based on network congestion.</p>
    </div>
  </div>
</div>

## P2P Trading Endpoints

<div class="api-endpoint">
  <div class="api-endpoint-header">
    <span class="api-endpoint-method get-method">GET</span>
    <span class="api-endpoint-path">/p2p/orders</span>
  </div>
  <div class="api-endpoint-body">
    <h3>List P2P Orders</h3>
    <p>Lists available P2P orders.</p>
    
    <h4>Query Parameters</h4>
    <table>
      <thead>
        <tr>
          <th>Parameter</th>
          <th>Type</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>page</td>
          <td>Integer</td>
          <td>Page number (default: 1)</td>
        </tr>
        <tr>
          <td>limit</td>
          <td>Integer</td>
          <td>Items per page (default: 10)</td>
        </tr>
        <tr>
          <td>token</td>
          <td>String</td>
          <td>Filter by token (e.g., BTC, ETH)</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<div class="api-endpoint">
  <div class="api-endpoint-header">
    <span class="api-endpoint-method post-method">POST</span>
    <span class="api-endpoint-path">/p2p/order</span>
  </div>
  <div class="api-endpoint-body">
    <h3>Create P2P Order</h3>
    <p>Creates a new P2P order.</p>
  </div>
</div>

## Bank Integration

<div class="api-endpoint">
  <div class="api-endpoint-header">
    <span class="api-endpoint-method post-method">POST</span>
    <span class="api-endpoint-path">/user/bank</span>
  </div>
  <div class="api-endpoint-body">
    <h3>Add Bank Account</h3>
    <p>Adds a bank account which users can use to receive transfers. If no bank is selected as default, the first bank would be selected as default or the bank that matches the other users (in P2P) bank.</p>
    
    <h4>Request Body</h4>
    
```json
{
  "accountName": "John Doe",
  "accountNumber": "1234567890",
  "bankName": "Example Bank",
  "routingNumber": "123456789",
  "isDefault": true
}
```

    <h4>Response</h4>
    
```json
{
  "success": true,
  "message": "Bank account added successfully",
  "bankId": "bank_123456"
}
```
  </div>
</div>

## Additional Notes

<div class="callout warning">
  <h4>Test Mode Limitations</h4>
  <p>The API is currently in test mode with the following limitations:</p>
  <ul>
    <li>Some endpoints may not work (usdc_trc20, usdt_erc20, usdt_bep20)</li>
    <li>BSC transactions will fail for accounts older than May 20, 2024</li>
    <li>Test tokens should be used cautiously</li>
    <li>Transaction fee estimation endpoints should not be called frequently</li>
  </ul>
</div>

For complete endpoint details including request/response examples, please refer to the Postman collection. 