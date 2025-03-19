# Swap Integration (ChangeNOW)
This guide explains how Matra integrates with ChangeNOW for token swapping functionality.

## Integration Overview
Matra uses the ChangeNOW API to enable seamless token swaps within the platform. ChangeNOW is a non-custodial exchange service that handles the complexity of token exchanges.

## API Integration
The integration involves several key API endpoints:

1. **Exchange Rate Calculation**
   ```
   GET /api/v1/exchange-rate
   ```
   Returns the current exchange rate between two tokens.

2. **Create Swap Transaction**
   ```
   POST /api/v1/create-transaction
   ```
   Initiates a new swap between tokens.

3. **Transaction Status**
   ```
   GET /api/v1/transaction-status/:id
   ```
   Checks the status of an ongoing transaction.

## Implementation Steps
To integrate ChangeNOW in your application:

1. Obtain API keys from ChangeNOW dashboard
2. Implement exchange rate calculation
3. Create a swap transaction flow
4. Set up webhook for status updates
5. Handle transaction completion

## Error Handling
Common errors and resolution strategies:
- Insufficient liquidity
- Network congestion
- Minimum amount requirements

## Testing
- Use ChangeNOW testnet for development
- Test with small amounts on mainnet
- Verify transaction times under various conditions

## Best Practices
- Cache exchange rates (but refresh frequently)
- Implement proper error handling
- Provide clear status updates to users
- Enforce minimum transaction amounts 