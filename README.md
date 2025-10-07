# Token Streaming Protocol

A decentralized token streaming protocol built on Stacks blockchain that enables continuous, block-by-block token transfers between parties.

## Overview

This protocol allows users to create streams where tokens are automatically distributed to recipients over time based on block progression. Perfect for salaries, subscriptions, or any time-based payment scenarios.

## Features

- **Stream Creation**: Set up continuous token streams with customizable parameters
- **Time-based Distribution**: Tokens are released proportionally to blocks mined
- **Refueling**: Add more tokens to existing streams
- **Withdrawal**: Recipients can withdraw accumulated tokens anytime
- **Refunds**: Senders can recover unused tokens after stream completion
- **Signature-based Updates**: Modify stream parameters with multi-party consent

## Smart Contract Functions

### Public Functions

- `stream-to(recipient, initial-balance, timeframe, payment-per-block)` - Create new stream
- `refuel(stream-id, amount)` - Add tokens to existing stream
- `withdraw(stream-id)` - Withdraw available tokens (recipient only)
- `refund(stream-id)` - Claim unused tokens after stream ends (sender only)
- `update-details(stream-id, payment-per-block, timeframe, signer, signature)` - Modify stream with consent.

### Read-only Functions

- `balance-of(stream-id, principal)` - Check available balance for a party
- `calculate-block-delta(timeframe)` - Calculate blocks elapsed in timeframe
- `hash-stream(stream-id, payment-per-block, timeframe)` - Generate stream hash for signatures
- `validate-signature(hash, signature, signer)` - Verify cryptographic signatures

## Quick Start

### Prerequisites

- [Clarinet](https://docs.hiro.so/stacks/clarinet)
- Node.js & npm

### Installation

```bash
git clone <repository-url>
cd stacks-token-streaming
npm install
```

### Testing

```bash
# Run contract checks
clarinet check

# Run test suite
npm test
```

### Deployment

```bash
# Deploy to testnet
clarinet integrate

# Deploy to mainnet
clarinet deploy --network mainnet
```

## Usage Example

```typescript
// Create a stream
const streamResult = simnet.callPublicFn(
  "stream",
  "stream-to",
  [
    Cl.principal("ST2CY5V39NHDPWSXMW9QDT3HC3GD6Q6XX4CFRK9AG"), // recipient
    Cl.uint(100),                                              // 100 STX total
    Cl.tuple({ 
      "start-block": Cl.uint(1000), 
      "stop-block": Cl.uint(1100) 
    }),                                                        // 100 blocks duration
    Cl.uint(1),                                               // 1 STX per block
  ],
  sender
);

// Recipient withdraws tokens
const withdrawResult = simnet.callPublicFn(
  "stream",
  "withdraw",
  [Cl.uint(0)], // stream ID
  recipient
);
```

## Stream Mechanics

1. **Creation**: Sender locks tokens in contract and defines stream parameters
2. **Distribution**: Tokens become available to recipient proportional to blocks mined
3. **Withdrawal**: Recipient can withdraw accumulated tokens at any time
4. **Completion**: After end block, sender can reclaim any unused tokens

## Security Features

- **Authorization checks** on all sensitive operations
- **Cryptographic signatures** required for stream modifications
- **Time-based validation** prevents premature refunds
- **Balance validation** ensures sufficient funds

## Error Codes

- `ERR_UNAUTHORIZED (u0)` - Caller not authorized for operation
- `ERR_INVALID_SIGNATURE (u1)` - Invalid cryptographic signature
- `ERR_STREAM_STILL_ACTIVE (u2)` - Stream hasn't ended yet
- `ERR_INVALID_STREAM_ID (u3)` - Stream doesn't exist

## Contributing

1. Fork the repository
2. Create a feature branch
3. Add tests for new functionality
4. Ensure all tests pass
5. Submit a pull request

## License

MIT License - see LICENSE file for details

## Acknowledgments

Built following the [LearnWeb3 Stacks Course](https://learnweb3.io/courses/introduction-to-stacks/project-build-a-token-streaming-protocol/)