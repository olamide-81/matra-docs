# P2P System & KYC
This guide explains how the P2P system works and the KYC requirements for vendors.

## P2P System Architecture
The Matra P2P system connects users directly with verified vendors for token exchanges:

1. **Order Matching**: Algorithm matches buy/sell orders based on price and availability
2. **Escrow System**: Smart contract holds funds in escrow during transactions
3. **Dispute Resolution**: Built-in process for resolving transaction disputes
4. **Rating System**: Vendors earn reputation based on successful transactions

## KYC Process for Vendors
Before a user can become a vendor, they must complete KYC verification:

1. **Basic Information**:
   - Full legal name
   - Date of birth
   - Address of residence
   - Phone number

2. **Identity Verification**:
   - Government-issued ID (passport, driver's license)
   - Selfie with ID
   - Proof of address (utility bill, bank statement)

3. **Business Information** (for business vendors):
   - Business registration documents
   - Business address verification
   - Beneficial owner information

## KYC Integration
Matra uses a third-party KYC provider for identity verification. The integration:
- Securely transmits user documents
- Performs face matching
- Conducts sanctions screening
- Verifies document authenticity

## Compliance Requirements
- AML compliance built into the vendor approval process
- Transaction monitoring for suspicious activity
- Regular re-verification for active vendors
- Country-specific compliance rules 