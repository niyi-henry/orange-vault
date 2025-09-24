# 🟠 OrangeVault Protocol

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Stacks](https://img.shields.io/badge/Stacks-Blockchain-orange)](https://stacks.org)
[![Clarity](https://img.shields.io/badge/Clarity-Smart%20Contract-purple)](https://clarity-lang.org)
[![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen.svg)](https://github.com/niyi-henry/orange-vault)

> **Next-generation Bitcoin-native lending infrastructure utilizing Stacks Layer 2 technology for permissionless, over-collateralized STX borrowing with institutional-grade risk management and autonomous liquidation systems.**

## 🚀 Overview

OrangeVault transforms Bitcoin DeFi by introducing a fully decentralized lending marketplace where users leverage STX as premium collateral for efficient capital allocation. The protocol implements cutting-edge risk assessment algorithms, multi-tiered collateral ratios, and flash-liquidation capabilities to ensure maximum capital efficiency while preserving system integrity.

### Key Features

- **🔒 Over-Collateralized Lending**: Secure STX borrowing with configurable collateral ratios
- **⚡ Flash Liquidations**: Autonomous liquidation engine protecting protocol solvency
- **📊 Risk Management**: Advanced algorithms for position health monitoring
- **🏛️ Institutional Grade**: Built for seamless institutional integration
- **🔐 Battle-Tested Security**: Leveraging Bitcoin's most secure Layer 2 infrastructure
- **🎯 Capital Efficiency**: Optimized lending parameters for maximum utilization

## 📋 Table of Contents

- [Architecture](#architecture)
- [Quick Start](#quick-start)
- [Protocol Mechanics](#protocol-mechanics)
- [Smart Contract API](#smart-contract-api)
- [Risk Parameters](#risk-parameters)
- [Development](#development)
- [Testing](#testing)
- [Deployment](#deployment)
- [Security](#security)
- [Contributing](#contributing)

## 🏗️ Architecture

The OrangeVault protocol is built on the Stacks blockchain using Clarity smart contracts, providing:

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   User Interface │    │  Core Protocol  │    │ Liquidation Bot │
│                 │    │                 │    │                 │
│ • Web3 Frontend │◄──►│ • Lending Pool  │◄──►│ • Health Monitor│
│ • Mobile App    │    │ • Risk Engine   │    │ • Auto Execute  │
│ • API Gateway   │    │ • Fee Collector │    │ • MEV Protection│
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────────┐
                    │ Stacks Blockchain│
                    │                 │
                    │ • Settlement    │
                    │ • Consensus     │
                    │ • State Storage │
                    └─────────────────┘
```

### Core Components

- **Lending Pool**: Central liquidity management and collateral custody
- **Risk Engine**: Real-time position monitoring and health calculations
- **Liquidation System**: Automated undercollateralized position resolution
- **Fee Management**: Protocol revenue collection and distribution

## 🚀 Quick Start

### Prerequisites

- [Clarinet](https://github.com/hirosystems/clarinet) (v2.0+)
- [Node.js](https://nodejs.org/) (v18+)
- [Git](https://git-scm.com/)

### Installation

```bash
# Clone the repository
git clone https://github.com/niyi-henry/orange-vault.git
cd orange-vault

# Install dependencies
npm install

# Verify Clarinet installation
clarinet --version
```

### Basic Usage

```bash
# Check contracts syntax
clarinet check

# Run unit tests
npm test

# Deploy to testnet
clarinet deploy --testnet

# Interactive console
clarinet console
```

## ⚙️ Protocol Mechanics

### Lending Lifecycle

1. **Deposit Collateral**: Users deposit STX tokens as collateral
2. **Borrow Assets**: Borrow STX against deposited collateral (up to collateral ratio)
3. **Manage Position**: Monitor health ratio and adjust as needed
4. **Repay Debt**: Repay borrowed amounts plus accumulated interest
5. **Withdraw Collateral**: Retrieve deposited collateral after debt settlement

### Collateral Management

```clarity
;; Deposit STX as collateral
(contract-call? .orange-vault deposit)

;; Borrow against collateral
(contract-call? .orange-vault borrow u1000000) ;; 1 STX

;; Repay borrowed amount
(contract-call? .orange-vault repay u1000000)

;; Withdraw collateral
(contract-call? .orange-vault withdraw u500000) ;; 0.5 STX
```

### Health Monitoring

The protocol continuously monitors position health using the collateral ratio:

```
Collateral Ratio = (Total Collateral × 100) / Total Borrowed
```

- **Healthy**: Ratio > 150% (minimum collateral ratio)
- **Warning**: Ratio between 130-150%
- **Liquidation**: Ratio < 130% (liquidation threshold)

## 📖 Smart Contract API

### Public Functions

#### Core Operations

```clarity
;; Deposit STX as collateral
(define-public (deposit))
→ Returns: (response uint uint)

;; Borrow STX against collateral
(define-public (borrow (amount uint)))
→ Returns: (response uint uint)

;; Repay borrowed STX
(define-public (repay (amount uint)))
→ Returns: (response uint uint)

;; Withdraw collateral
(define-public (withdraw (amount uint)))
→ Returns: (response uint uint)

;; Liquidate undercollateralized position
(define-public (liquidate (user principal)))
→ Returns: (response bool uint)
```

#### Administrative Functions

```clarity
;; Update minimum collateral ratio
(define-public (set-minimum-collateral-ratio (new-ratio uint)))

;; Update liquidation threshold  
(define-public (set-liquidation-threshold (new-threshold uint)))

;; Update protocol fee
(define-public (set-protocol-fee (new-fee uint)))
```

### Read-Only Functions

```clarity
;; Get user position data
(define-read-only (get-user-position (user principal)))
→ Returns: {total-collateral: uint, total-borrowed: uint, loan-count: uint}

;; Get protocol statistics
(define-read-only (get-protocol-stats))
→ Returns: {total-deposits: uint, total-borrows: uint, ...}
```

### Error Codes

| Code | Constant | Description |
|------|----------|-------------|
| 100 | `ERR-NOT-AUTHORIZED` | Insufficient permissions |
| 101 | `ERR-INSUFFICIENT-COLLATERAL` | Collateral ratio too low |
| 102 | `ERR-INVALID-AMOUNT` | Invalid amount specified |
| 103 | `ERR-LOAN-NOT-FOUND` | Loan does not exist |
| 104 | `ERR-LOAN-ACTIVE` | Loan is still active |
| 105 | `ERR-INSUFFICIENT-BALANCE` | Insufficient token balance |
| 106 | `ERR-LIQUIDATION-FAILED` | Liquidation requirements not met |
| 107 | `ERR-INVALID-PARAMETER` | Parameter out of valid range |

## 🎯 Risk Parameters

### Default Configuration

| Parameter | Value | Description |
|-----------|-------|-------------|
| **Minimum Collateral Ratio** | 150% | Required overcollateralization |
| **Liquidation Threshold** | 130% | Automatic liquidation trigger |
| **Protocol Fee** | 1% | Protocol revenue percentage |
| **Maximum Collateral Ratio** | 500% | Upper bound for security |
| **Minimum Collateral Ratio** | 110% | Lower bound for security |

### Parameter Bounds

- **Collateral Ratios**: 110% - 500%
- **Protocol Fees**: 0% - 10%
- **Liquidation Threshold**: Must be ≤ Minimum Collateral Ratio

## 🛠️ Development

### Project Structure

```
orange-vault/
├── contracts/
│   └── orange-vault.clar      # Main protocol contract
├── tests/
│   └── orange-vault.test.ts   # Comprehensive test suite
├── settings/
│   ├── Devnet.toml           # Development configuration
│   ├── Testnet.toml          # Testnet configuration
│   └── Mainnet.toml          # Production configuration
├── Clarinet.toml             # Clarinet project config
├── package.json              # Node.js dependencies
└── vitest.config.js          # Test configuration
```

### Local Development

```bash
# Start Clarinet console
clarinet console

# Check contract validity
clarinet check

# Format contracts
clarinet fmt --in-place

# Run integration tests
clarinet integrate

# Generate deployment plan
clarinet deployment generate --testnet
```

### Contract Interaction Examples

```javascript
// JavaScript SDK usage
import { Cl } from '@stacks/transactions';

// Deposit collateral
const depositTx = contractCall({
  contractAddress: 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM',
  contractName: 'orange-vault',
  functionName: 'deposit',
  functionArgs: [],
});

// Borrow against collateral
const borrowTx = contractCall({
  contractAddress: 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM',
  contractName: 'orange-vault',
  functionName: 'borrow',
  functionArgs: [Cl.uint(1000000)], // 1 STX in microSTX
});
```

## 🧪 Testing

### Test Suite Coverage

- **Unit Tests**: Individual function testing with edge cases
- **Integration Tests**: End-to-end workflow validation
- **Security Tests**: Attack vector and edge case protection
- **Gas Optimization**: Cost analysis and optimization

### Running Tests

```bash
# Run all tests
npm test

# Run with coverage report
npm run test:report

# Watch mode for development
npm run test:watch

# Specific test file
npx vitest run tests/orange-vault.test.ts
```

### Test Categories

- ✅ **Deposit Operations**: Collateral management
- ✅ **Borrow Operations**: Lending functionality  
- ✅ **Repay Operations**: Debt settlement
- ✅ **Withdraw Operations**: Collateral retrieval
- ✅ **Liquidation Engine**: Position liquidation
- ✅ **Administrative Functions**: Parameter updates
- ✅ **Error Handling**: Edge cases and failures
- ✅ **Security Vectors**: Attack prevention

## 🚀 Deployment

### Network Configuration

#### Testnet Deployment

```bash
# Generate deployment plan
clarinet deployment generate --testnet

# Apply deployment
clarinet deployment apply --testnet
```

#### Mainnet Deployment

```bash
# Security audit required before mainnet
npm run audit

# Generate mainnet deployment
clarinet deployment generate --mainnet

# Deploy with multisig (recommended)
clarinet deployment apply --mainnet --multisig
```

### Post-Deployment Checklist

- [ ] Contract verification on explorer
- [ ] Initial parameter configuration
- [ ] Admin key management setup
- [ ] Monitoring and alerting configuration
- [ ] Frontend integration testing
- [ ] Security audit completion

## 🔒 Security

### Security Features

- **Access Control**: Role-based permissions with owner restrictions
- **Input Validation**: Comprehensive parameter bounds checking
- **Overflow Protection**: Safe arithmetic operations throughout
- **State Consistency**: Atomic updates preventing race conditions
- **Flash Loan Protection**: MEV and sandwich attack mitigation

### Audit Checklist

- [ ] **Reentrancy**: No external calls in state-changing functions
- [ ] **Integer Overflow**: Safe math operations implemented
- [ ] **Access Control**: Proper authorization checks
- [ ] **Input Validation**: All inputs sanitized and bounded
- [ ] **State Consistency**: Atomic state transitions
- [ ] **Economic Attacks**: Incentive alignment verified

### Reporting Vulnerabilities

Please report security vulnerabilities via:

- **Email**: <security@orangevault.finance>
- **Encrypted**: Use PGP key on our website
- **Bug Bounty**: Up to $50,000 for critical findings

## 🤝 Contributing

We welcome contributions from the community! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

### Development Process

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Test** thoroughly (`npm test`)
5. **Push** to the branch (`git push origin feature/amazing-feature`)
6. **Open** a Pull Request

### Code Standards

- **Clarity Style**: Follow [Clarity best practices](https://docs.stacks.org/clarity)
- **Testing**: Maintain >95% test coverage
- **Documentation**: Update relevant documentation
- **Security**: Consider security implications of all changes

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Stacks Foundation** for blockchain infrastructure
- **Hiro Systems** for development tools
- **Bitcoin Community** for inspiration and security model
- **DeFi Pioneers** for protocol design patterns
