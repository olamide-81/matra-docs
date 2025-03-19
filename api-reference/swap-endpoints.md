# Swap Endpoints

## Get Exchange Rate
```
GET /api/v1/swap/rate
```

Get the current exchange rate between two tokens.

### Query Parameters
- `fromToken`: Source token symbol (e.g., `BTC`)
- `toToken`: Destination token symbol (e.g., `ETH`)
- `amount`: Amount of source token to exchange

### Response
```json
{
  "success": true,
  "fromToken": "BTC",
  "toToken": "ETH",
  "rate": 16.25,
  "fromAmount": "0.1",
  "toAmount": "1.625",
  "networkFee": "0.0005",
  "estimatedTime": "10-20 minutes",
  "validUntil": 1678901234
}
```

## Create Swap Transaction
```
POST /api/v1/swap/create
```

Create a new swap transaction.

### Request
```json
{
  "fromToken": "BTC",
  "toToken": "ETH",
  "fromAmount": "0.1",
  "toAddress": "0x1234567890abcdef1234567890abcdef12345678",
  "refundAddress": "bc1q1234567890abcdef1234567890abcdef12345678"
}
```

### Response
```json
{
  "success": true,
  "swapId": "swap_123456",
  "depositAddress": "bc1q9876543210abcdef9876543210abcdef9876543210",
  "fromToken": "BTC",
  "toToken": "ETH",
  "fromAmount": "0.1",
  "expectedToAmount": "1.625",
  "toAddress": "0x1234567890abcdef1234567890abcdef12345678",
  "refundAddress": "bc1q1234567890abcdef1234567890abcdef12345678",
  "status": "WAITING_FOR_DEPOSIT",
  "expiresAt": 1678901534
}
```

## Get Swap Status
```
GET /api/v1/swap/:swapId
```

Get the status of a swap transaction.

### Parameters
- `swapId`: ID of the swap transaction

### Response
```json
{
  "success": true,
  "swap": {
    "id": "swap_123456",
    "depositAddress": "bc1q9876543210abcdef9876543210abcdef9876543210",
    "fromToken": "BTC",
    "toToken": "ETH",
    "fromAmount": "0.1",
    "expectedToAmount": "1.625",
    "actualToAmount": "1.623",
    "toAddress": "0x1234567890abcdef1234567890abcdef12345678",
    "refundAddress": "bc1q1234567890abcdef1234567890abcdef12345678",
    "status": "EXCHANGING",
    "createdAt": 1678900000,
    "expiresAt": 1678901534,
    "depositTxId": "0xabcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890",
    "payoutTxId": null,
    "timeline": [
      {
        "status": "WAITING_FOR_DEPOSIT",
        "timestamp": 1678900000
      },
      {
        "status": "CONFIRMING_DEPOSIT",
        "timestamp": 1678900300
      },
      {
        "status": "EXCHANGING",
        "timestamp": 1678900600
      }
    ]
  }
}
```

## Get Supported Tokens
```
GET /api/v1/swap/tokens
```

Get a list of tokens supported for swapping.

### Response
```json
{
  "success": true,
  "tokens": [
    {
      "symbol": "BTC",
      "name": "Bitcoin",
      "minAmount": "0.001",
      "maxAmount": "10",
      "isAvailable": true
    },
    {
      "symbol": "ETH",
      "name": "Ethereum",
      "minAmount": "0.01",
      "maxAmount": "500",
      "isAvailable": true
    },
    {
      "symbol": "USDT",
      "name": "Tether USD",
      "minAmount": "50",
      "maxAmount": "100000",
      "isAvailable": true
    }
  ]
}
```

## Get Swap History
```
GET /api/v1/swap/history
```

Get the swap transaction history for the authenticated user.

### Query Parameters
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 10)

### Response
```json
{
  "success": true,
  "swaps": [
    {
      "id": "swap_123456",
      "fromToken": "BTC",
      "toToken": "ETH",
      "fromAmount": "0.1",
      "toAmount": "1.623",
      "status": "COMPLETED",
      "createdAt": 1678900000,
      "completedAt": 1678901800
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 5,
    "pages": 1
  }
}
``` 