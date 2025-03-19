# Creating or Importing a Wallet

This internal documentation describes the wallet creation and import processes in the Matra app.

## Creating a New Wallet

When users choose to create a new wallet:

1. User launches the Matra app and completes email verification
2. User taps "Create New Wallet" on the wallet setup screen
3. The app generates a new 12-word recovery phrase
4. User is instructed to write down the recovery phrase in the correct order
5. User must verify the recovery phrase by selecting the words in the correct sequence
6. Once verified, the wallet is created and ready to use

## Importing an Existing Wallet

When users choose to import an existing wallet:

1. User launches the Matra app and completes email verification
2. User taps "Import Existing Wallet" on the wallet setup screen
3. User enters their 12-word recovery phrase in the correct order
4. The app validates the recovery phrase
5. If valid, the wallet is restored with all associated accounts and balances

## Technical Implementation

### Wallet Generation

The wallet creation process:
- Uses BIP-39 standard for mnemonic generation
- Creates a 12-word seed phrase with 128 bits of entropy
- Derives keys using BIP-44 derivation paths
- Supports multiple blockchain networks through a single recovery phrase

### Recovery Phrase Security

<div class="callout danger">
  <p><strong>CRITICAL:</strong> The recovery phrase is the only way to recover a wallet. There is no password recovery system.</p>
</div>

Security measures implemented for recovery phrases:
- Never stored on our servers
- Only briefly held in app memory during creation/verification
- Cleared from memory after wallet creation
- User is responsible for securely storing their recovery phrase

## Security Recommendations

Instruct users to:
- Write down their recovery phrase on paper in the correct order
- Store this paper in a secure, waterproof, and fireproof location
- Consider creating multiple backup copies stored in different secure locations
- Never store recovery phrases digitally (no photos, no digital documents, no cloud storage)
- Never share their recovery phrase with anyone, including Matra support staff 