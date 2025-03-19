# Vendor Management

## Apply for Vendor Status
```
POST /api/v1/vendor/apply
```

Apply to become a vendor on the platform.

### Request
```json
{
  "businessName": "Crypto Exchange Pro",
  "businessType": "INDIVIDUAL",
  "country": "US",
  "phoneNumber": "+12025550187",
  "tradingVolume": "MEDIUM",
  "tradingCurrencies": ["USD", "EUR"],
  "supportedTokens": ["BTC", "ETH", "USDT"],
  "preferredPaymentMethods": ["BANK_TRANSFER", "PAYPAL"]
}
```

### Response
```json
{
  "success": true,
  "applicationId": "app_123456",
  "status": "PENDING_DOCUMENTS",
  "message": "Please complete KYC verification to continue",
  "nextSteps": [
    "Upload identity documents",
    "Complete verification form",
    "Wait for approval"
  ]
}
```

## Upload Vendor Documents
```
POST /api/v1/vendor/documents
```

Upload KYC documents for vendor verification.

### Request
```
multipart/form-data
```

Form fields:
- `applicationType`: Type of document being uploaded (`ID_FRONT`, `ID_BACK`, `SELFIE`, `ADDRESS_PROOF`, `BUSINESS_REG`)
- `document`: File upload

### Response
```json
{
  "success": true,
  "documentId": "doc_789012",
  "status": "UPLOADED",
  "message": "Document uploaded successfully",
  "pendingDocuments": ["BUSINESS_REG"],
  "allDocumentsUploaded": false
}
```

## Check Vendor Application Status
```
GET /api/v1/vendor/application
```

Check the status of a vendor application.

### Response
```json
{
  "success": true,
  "application": {
    "id": "app_123456",
    "status": "UNDER_REVIEW",
    "submittedAt": 1678900000,
    "updatedAt": 1678910000,
    "documents": [
      {
        "type": "ID_FRONT",
        "status": "VERIFIED",
        "uploadedAt": 1678900500
      },
      {
        "type": "ID_BACK",
        "status": "VERIFIED",
        "uploadedAt": 1678900600
      },
      {
        "type": "SELFIE",
        "status": "VERIFIED",
        "uploadedAt": 1678900700
      },
      {
        "type": "ADDRESS_PROOF",
        "status": "VERIFIED",
        "uploadedAt": 1678900800
      },
      {
        "type": "BUSINESS_REG",
        "status": "UNDER_REVIEW",
        "uploadedAt": 1678900900
      }
    ],
    "estimatedCompletionTime": "1-2 business days"
  }
}
```

## Update Vendor Settings
```
PUT /api/v1/vendor/settings
```

Update vendor settings, including rates and availability.

### Request
```json
{
  "isActive": true,
  "dailyLimit": 25000,
  "minTransactionAmount": 100,
  "maxTransactionAmount": 5000,
  "supportedTokens": ["BTC", "ETH", "USDT", "XRP"],
  "paymentMethods": ["BANK_TRANSFER", "PAYPAL", "VENMO"],
  "rates": {
    "BTC": {
      "buy": 38500,
      "sell": 38300
    },
    "ETH": {
      "buy": 1850,
      "sell": 1830
    }
  },
  "autoReply": "Thank you for your order. Please complete payment within 30 minutes."
}
```

### Response
```json
{
  "success": true,
  "message": "Vendor settings updated successfully",
  "settings": {
    "isActive": true,
    "dailyLimit": 25000,
    "minTransactionAmount": 100,
    "maxTransactionAmount": 5000,
    "supportedTokens": ["BTC", "ETH", "USDT", "XRP"],
    "paymentMethods": ["BANK_TRANSFER", "PAYPAL", "VENMO"],
    "rates": {
      "BTC": {
        "buy": 38500,
        "sell": 38300
      },
      "ETH": {
        "buy": 1850,
        "sell": 1830
      }
    },
    "autoReply": "Thank you for your order. Please complete payment within 30 minutes."
  }
}
```

## Get Vendor Dashboard Stats
```
GET /api/v1/vendor/stats
```

Get statistics for the vendor dashboard.

### Query Parameters
- `period`: Time period for stats (`today`, `week`, `month`, `all`)

### Response
```json
{
  "success": true,
  "stats": {
    "transactionsCompleted": 156,
    "transactionVolume": {
      "BTC": "3.45",
      "ETH": "25.67",
      "USDT": "15000.00"
    },
    "totalVolumeUSD": 187450.23,
    "averageCompletionTime": 25,
    "rating": 4.87,
    "disputeRate": 0.5,
    "activeOrders": 3,
    "chartData": [
      {
        "date": "2023-01-01",
        "volume": 5678.45
      },
      {
        "date": "2023-01-02",
        "volume": 6789.12
      }
    ]
  }
}
``` 