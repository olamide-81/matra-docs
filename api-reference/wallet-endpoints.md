# Wallet Endpoints

## Create Wallet
```
POST /api/v1/wallet/create
```

Create a new self-custodial wallet for the authenticated user.

### Request
```json
{
  "walletName": "My Main Wallet"
}
```

### Response
```json
{
  "success": true,
  "walletId": "wallet_789012",
  "address": "0x1234567890abcdef1234567890abcdef12345678",
  "recoveryPhrase": "word1 word2 word3 word4 word5 word6 word7 word8 word9 word10 word11 word12",
  "message": "Store your recovery phrase securely. It will not be shown again."
}
```

## Get Wallet Balance
```
GET /api/v1/wallet/balance
```

Get the balance of all tokens in the wallet.

### Response
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

## Get Transaction History
```
GET /api/v1/wallet/transactions
```

Get transaction history for the wallet.

### Query Parameters
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 10)
- `token`: Filter by token (optional)

### Response
```json
{
  "success": true,
  "transactions": [
    {
      "id": "tx_123456",
      "type": "RECEIVE",
      "token": "BTC",
      "amount": "0.01234567",
      "from": "0x9876543210abcdef9876543210abcdef98765432",
      "to": "0x1234567890abcdef1234567890abcdef12345678",
      "timestamp": 1678901234,
      "status": "CONFIRMED",
      "confirmations": 6,
      "txHash": "0xabcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 35,
    "pages": 4
  }
}
```

## Send Tokens
```
POST /api/v1/wallet/send
```

Send tokens to another address.

### Request
```json
{
  "token": "ETH",
  "amount": "0.5",
  "toAddress": "0x9876543210abcdef9876543210abcdef98765432",
  "memo": "Payment for services",
  "gasPrice": "standard"
}
```

### Response
```json
{
  "success": true,
  "transactionId": "tx_234567",
  "status": "PENDING",
  "txHash": "0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
  "estimatedCompletionTime": 30
}
```

## Get Token Information
```
GET /api/v1/wallet/tokens
```

Get information about supported tokens.

### Response
```json
{
  "success": true,
  "tokens": [
    {
      "symbol": "BTC",
      "name": "Bitcoin",
      "decimals": 8,
      "network": "Bitcoin",
      "icon": "https://api.matra.io/assets/icons/btc.png",
      "enabled": true
    },
    {
      "symbol": "ETH",
      "name": "Ethereum",
      "decimals": 18,
      "network": "Ethereum",
      "icon": "https://api.matra.io/assets/icons/eth.png",
      "enabled": true
    }
  ]
}
``` 