# P2P Endpoints

## Get Available Vendors
```
GET /api/v1/p2p/vendors
```

Get a list of available vendors for P2P transactions.

### Query Parameters
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 10)
- `sortBy`: Sort field (options: `rating`, `transactions`, `created_at`)
- `sortDir`: Sort direction (`asc` or `desc`)
- `token`: Filter by token (e.g., `BTC`, `ETH`)

### Response
```json
{
  "success": true,
  "vendors": [
    {
      "id": "vendor_123456",
      "name": "CryptoTrader",
      "verified": true,
      "rating": 4.8,
      "transactionsCompleted": 382,
      "successRate": 99.2,
      "tokens": ["BTC", "ETH", "USDT"],
      "minAmount": 50,
      "maxAmount": 10000,
      "rates": {
        "BTC": {
          "buy": 38500,
          "sell": 38300
        },
        "ETH": {
          "buy": 1850,
          "sell": 1830
        }
      }
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 25,
    "pages": 3
  }
}
```

## Create P2P Order
```
POST /api/v1/p2p/order
```

Create a new P2P order.

### Request
```json
{
  "vendorId": "vendor_123456",
  "type": "BUY",
  "token": "BTC",
  "amount": "0.05",
  "fiatCurrency": "USD",
  "fiatAmount": "1925.00",
  "paymentMethod": "BANK_TRANSFER"
}
```

### Response
```json
{
  "success": true,
  "orderId": "order_789012",
  "status": "CREATED",
  "expiresAt": 1678956789,
  "vendorId": "vendor_123456",
  "vendorName": "CryptoTrader",
  "type": "BUY",
  "token": "BTC",
  "amount": "0.05",
  "fiatCurrency": "USD",
  "fiatAmount": "1925.00",
  "rate": 38500,
  "paymentMethod": "BANK_TRANSFER",
  "paymentDetails": {
    "accountName": "Crypto Trader LLC",
    "accountNumber": "XXXX-XXXX-XX56",
    "bankName": "Global Bank",
    "reference": "BTC-789012"
  }
}
```

## Get Order Status
```
GET /api/v1/p2p/order/:orderId
```

Get the status of a P2P order.

### Parameters
- `orderId`: ID of the order

### Response
```json
{
  "success": true,
  "order": {
    "id": "order_789012",
    "status": "PENDING_PAYMENT",
    "createdAt": 1678945678,
    "expiresAt": 1678956789,
    "vendorId": "vendor_123456",
    "vendorName": "CryptoTrader",
    "type": "BUY",
    "token": "BTC",
    "amount": "0.05",
    "fiatCurrency": "USD",
    "fiatAmount": "1925.00",
    "rate": 38500,
    "paymentMethod": "BANK_TRANSFER",
    "paymentDetails": {
      "accountName": "Crypto Trader LLC",
      "accountNumber": "XXXX-XXXX-XX56",
      "bankName": "Global Bank",
      "reference": "BTC-789012"
    },
    "timeline": [
      {
        "status": "CREATED",
        "timestamp": 1678945678
      },
      {
        "status": "PENDING_PAYMENT",
        "timestamp": 1678946000
      }
    ]
  }
}
```

## Confirm Payment
```
POST /api/v1/p2p/order/:orderId/confirm-payment
```

Confirm that payment has been sent for a P2P order.

### Parameters
- `orderId`: ID of the order

### Request
```json
{
  "paymentReference": "TX123456789",
  "paymentProofUrl": "https://example.com/payment-proof.jpg"
}
```

### Response
```json
{
  "success": true,
  "message": "Payment confirmation received",
  "order": {
    "id": "order_789012",
    "status": "PAYMENT_CONFIRMED",
    "nextStep": "Waiting for vendor to release tokens"
  }
}
```

## Rate Vendor
```
POST /api/v1/p2p/order/:orderId/rate
```

Rate a vendor after completing a transaction.

### Parameters
- `orderId`: ID of the completed order

### Request
```json
{
  "rating": 5,
  "comment": "Great trader, fast and reliable!"
}
```

### Response
```json
{
  "success": true,
  "message": "Rating submitted successfully",
  "newVendorRating": 4.85
}
``` 