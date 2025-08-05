# 🔄 Bonk.fun Volume Bot - Raydium LaunchLab

A high-performance automated trading bot designed for Bonk.fun tokens on Raydium LaunchLab. This bot creates multiple wallets, executes buy/sell transactions, and manages token accounts with advanced features for volume generation and SOL retrieval.

## 📋 Table of Contents

- [Features](#-features)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Usage](#-usage)
- [Project Structure](#-project-structure)
- [API Reference](#-api-reference)
- [Examples](#-examples)
- [Contact](#-contact)

## ✨ Features

### Core Functionality
- ✅ **Multi-Wallet Management**: Automatically create and manage multiple Solana wallets
- ✅ **Automated Trading**: Execute buy/sell transactions on Raydium LaunchLab
- ✅ **Volume Generation**: Generate trading volume with configurable parameters
- ✅ **SOL Distribution**: Distribute SOL to multiple wallets for transaction fees
- ✅ **Token Account Management**: Create, monitor, and close token accounts
- ✅ **SOL Retrieval**: Automated SOL withdrawal and account cleanup

### Advanced Features
- 🔥 **Jito MEV Integration**: Bundle transactions for better execution
- 🔥 **Lookup Table Optimization**: Optimize transaction size and gas costs
- 🔥 **Retry Mechanisms**: Robust error handling with automatic retries
- 🔥 **Real-time Monitoring**: Live transaction logging and balance tracking
- 🔥 **Configurable Parameters**: Customizable buy amounts, intervals, and settings
- 🔥 **Transaction Batching**: Efficient batch processing of multiple transactions

### Technical Features
- 🛡️ **TypeScript**: Full TypeScript implementation with strict typing
- 🛡️ **Anchor Framework**: Solana program integration using Anchor
- 🛡️ **Raydium SDK v2**: Latest Raydium SDK for LaunchLab integration
- 🛡️ **Environment Management**: Secure configuration with dotenv
- 🛡️ **CLI Interface**: Interactive command-line interface

## 🏗️ Architecture

The bot is built with a modular architecture:

```
├── main.ts              # Entry point and CLI interface
├── bot.ts               # Core bot logic and transaction execution
├── utils.ts             # Utility functions for trading operations
├── retrieve.ts          # SOL retrieval and account cleanup
├── config.ts            # Configuration and environment setup
├── clients/             # Client implementations
│   ├── jito.ts         # Jito MEV integration
│   ├── constants.ts    # Raydium constants and addresses
│   ├── config.ts       # Client configuration
│   ├── utils.ts        # Client utilities
│   ├── encrypt/        # Encryption utilities
│   └── LookupTableProvider.ts # Lookup table management
└── src/keypairs/       # Generated wallet keypairs
```

## 📋 Prerequisites

- **Node.js** (v18 or higher)
- **Yarn** package manager
- **Solana CLI** tools
- **Solana wallet** with SOL balance
- **RPC endpoint** (Helius, QuickNode, or Alchemy recommended)
- **Jito API key** (for MEV bundle submission)

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/solship/Bonkfun-Bonkdotfun-VolumnBot.git
cd Bonkfun-Bonkdotfun-VolumnBot
```

### 2. Install Dependencies

```bash
yarn install
```

### 3. Environment Configuration

Create a `.env` file in the root directory:

```env
# Solana RPC Endpoint (Required)
RPC=https://your-rpc-endpoint.com

# Wallet Secret Key (Required)
SECRET_KEY=your_base58_encoded_secret_key

# Jito API Key (Required for MEV bundles)
API_KEY=your_jito_api_key

# Debug Mode (Optional)
DEBUG=true
```

### 4. Build the Project

```bash
yarn build
```

## ⚙️ Configuration

### Environment Variables

| Variable | Description | Required | Example |
|----------|-------------|----------|---------|
| `RPC` | Solana RPC endpoint | Yes | `https://api.mainnet-beta.solana.com` |
| `SECRET_KEY` | Base58 encoded wallet secret key | Yes | `4xQy...` |
| `API_KEY` | Jito API key for MEV bundles | Yes | `XXXX-FFFFF` |
| `DEBUG` | Enable debug logging | No | `true` |

### Configuration Parameters

The bot supports various configurable parameters:

- **Buy Amounts**: Random or fixed SOL amounts for purchases
- **Transaction Delays**: Configurable delays between transactions
- **Jito Tips**: MEV bundle tip amounts
- **Wallet Count**: Number of wallets to create and manage
- **Retry Attempts**: Number of retry attempts for failed transactions

## 🎯 Usage

### Starting the Bot

```bash
# Start with interactive menu
yarn start

# Start with configuration file
yarn start -c config.json
```

### Interactive Menu Options

1. **AUTO Random Buyers**: Execute automated buy/sell transactions
2. **Retrieve SOL ALL WALLETS**: Withdraw SOL and close token accounts

### Example Configuration File

```json
{
  "buyAmount": 0.1,
  "delayMs": 300,
  "jitoTip": 0.0001,
  "walletCount": 10,
  "retryAttempts": 3
}
```

## 📁 Project Structure

### Core Files

- **`main.ts`**: Entry point with CLI interface and menu system
- **`bot.ts`**: Main bot logic, wallet creation, and transaction execution
- **`utils.ts`**: Trading utilities, swap instructions, and token operations
- **`retrieve.ts`**: SOL retrieval, account cleanup, and balance management
- **`config.ts`**: Environment configuration and connection setup

### Client Modules

- **`clients/jito.ts`**: Jito MEV bundle submission
- **`clients/constants.ts`**: Raydium LaunchLab constants and addresses
- **`clients/LookupTableProvider.ts`**: Lookup table management for transaction optimization
- **`clients/utils.ts`**: Utility functions for retry logic and delays
- **`clients/encrypt/`**: Encryption and parsing utilities

### Generated Files

- **`src/keypairs/`**: Directory containing generated wallet keypairs
- **`dist/`**: Compiled TypeScript output
- **`yarn.lock`**: Dependency lock file

## 🔧 API Reference

### Core Functions

#### `extender(config?: any)`
Main bot function that executes the volume generation strategy.

#### `closeAcc()`
Retrieves SOL from all wallets and closes token accounts.

#### `buy(mint: string, amount: number, keypair: Keypair)`
Executes a buy transaction for the specified token.

#### `sell(mint: string, amount: number, keypair: Keypair)`
Executes a sell transaction for the specified token.

### Utility Functions

#### `getPoolInfo(mint: string)`
Retrieves pool information for a given token mint.

#### `getSwapQuote(baseAmountIn: number, inputMint: string, tokenMint: string, slippage: number)`
Gets a swap quote for the specified parameters.

#### `burnAccount(wallet: Keypair, keypair: Keypair, connection: Connection, ata: PublicKey, tokenprogram: PublicKey)`
Burns tokens and closes the associated token account.

## 📊 Examples

### Basic Usage

```typescript
import { extender } from './bot';

// Start the bot with default settings
await extender();

// Start with custom configuration
const config = {
  buyAmount: 0.05,
  delayMs: 500,
  jitoTip: 0.0001
};
await extender(config);
```

### SOL Retrieval

```typescript
import { closeAcc } from './retrieve';

// Retrieve SOL from all wallets
await closeAcc();
```

### Custom Trading

```typescript
import { buy, sell } from './utils';

// Buy tokens
await buy('token_mint_address', 0.1, keypair);

// Sell tokens
await sell('token_mint_address', 0.1, keypair);
```

## 🔗 Dependencies

### Core Dependencies
- `@solana/web3.js`: Solana Web3 JavaScript API
- `@raydium-io/raydium-sdk-v2`: Raydium SDK for LaunchLab
- `@coral-xyz/anchor`: Anchor framework for Solana
- `@solana/spl-token`: SPL Token program utilities
- `jito-ts`: Jito MEV bundle submission

### Utility Dependencies
- `axios`: HTTP client for API requests
- `bignumber.js`: Big number arithmetic
- `chalk`: Terminal color output
- `dotenv`: Environment variable management
- `figlet`: ASCII art generation

## 🛠️ Development

### Building

```bash
# Build TypeScript
yarn build

# Development mode with hot reload
yarn dev
```

### Testing

```bash
# Run tests (if available)
yarn test
```

### Code Quality

The project uses TypeScript with strict mode enabled and follows modern JavaScript/TypeScript best practices.

## 📈 Performance

- **Transaction Batching**: Efficient batch processing of multiple transactions
- **Lookup Tables**: Optimized transaction size and gas costs
- **MEV Bundles**: Better transaction execution through Jito
- **Retry Logic**: Robust error handling and automatic retries
- **Memory Management**: Efficient wallet and account management

## 🔒 Security

- **Environment Variables**: Sensitive data stored in `.env` files
- **Secret Key Management**: Secure handling of wallet private keys
- **Transaction Validation**: Comprehensive transaction simulation and validation
- **Error Handling**: Robust error handling to prevent fund loss

## 📞 Contact

**Developer**: Tru3Bliss

- **Telegram**: [@Tru3B1iss](https://t.me/Tru3B1iss)
- **X (Twitter)**: [@XTruebliss](https://x.com/XTruebliss)
- **Discord**: [@trueb1iss](https://discord.com/users/1274339638668038187)

## 📄 License

This project is licensed under the ISC License.

## ⚠️ Disclaimer

This software is for educational and research purposes. Use at your own risk. The developers are not responsible for any financial losses incurred while using this bot. Always test with small amounts first and ensure you understand the risks involved in automated trading.

## 🔄 Recent Updates

- **v2.0.0**: Major update with Raydium SDK v2 integration
- Enhanced LaunchLab support
- Improved transaction batching
- Better error handling and retry logic
- Updated dependencies and security improvements

---

**Note**: This bot is specifically designed for Bonk.fun tokens on Raydium LaunchLab. Ensure you have sufficient SOL for transaction fees and understand the risks involved in automated trading.
