# ASX-TOKEN

# ASX Reward Token 🌿💎

**Automatic cryptocurrency reward system for cannabis seed purchases on Base L2**

ASX (ASX Reward Token) is a non-transferable ERC-20 loyalty token that automatically distributes to customers when they purchase cannabis seeds and other retail goods online but we have a strong hold on that market right now. Customers can vote on business decisions, redeem tokens for merchandise, and participate in tournaments - but cannot sell the tokens on exchanges.

---

## 🎯 Live Contracts on Base Mainnet

| Contract | Address | BaseScan |
|----------|---------|----------|
| **ASX Token** | `0x0c49d436f0c669Be4690CCB98c7cC6937E8EB8ae` | [View](https://basescan.org/address/0x0c49d436f0c669Be4690CCB98c7cC6937E8EB8ae) |
| **Purchase Processor** | `0xebbC50A761AF9A3A49D3f6Aa1AA038b9B2b70F40` | [View](https://basescan.org/address/0xebbC50A761AF9A3A49D3f6Aa1AA038b9B2b70F40) |
| **USDC (Base)** | `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` | [View](https://basescan.org/address/0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913) |

**Business Wallet:** `0xB14772bf2F491F55845bfa16ba0b5C30285d6907`  
**Treasury Wallet:** `0xc9c3c145fa09C2591d2492E6849B937Ea86B05Fe`

**Deployed:** October 23, 2025  
**Network:** Base (Chain ID: 8453)

---

## 📊 Token Details

- **Name:** ASX Reward Token
- **Symbol:** ASX
- **Decimals:** 18
- **Max Supply:** 100,000,000 ASX
- **Treasury Supply:** 10,000,000 ASX
- **Type:** Non-transferable loyalty token

### Token Features

✅ **Non-Transferable** - Holders cannot sell or transfer tokens (enforced at contract level)  
✅ **Voting Power** - Each token = 1 vote for governance  
✅ **Burnable** - Holders can burn tokens to redeem for rewards  
✅ **Auto-Distribution** - Automatically sent to customers on every purchase  
✅ **Transparent** - All transactions on-chain and publicly visible

---

## 🏗️ How It Works

### Customer Purchase Flow

```
1. Customer adds seeds to cart ($100)
   ↓
2. Pays with Coinbase Commerce (crypto)
   ↓
3. Webhook triggers Purchase Processor
   ↓
4. Contract splits payment:
   - 90% → Business Wallet (USDC)
   - 10% worth → ASX tokens to customer
   ↓
5. Customer receives:
   - Seeds (shipped)
   - 1,000 ASX tokens (instant)
```

### Reward Calculation

- **Reward Rate:** 10% of purchase value
- **Conversion Rate:** 100 ASX per 1 USDC
- **Minimum Purchase:** $10

**Example:**
- Purchase: $50 in seeds
- Reward Value: $5 (10%)
- ASX Received: 500 tokens (5 × 100)

---

## 🔧 Integration Guide

### Prerequisites

- Node.js v18+
- Hardhat development environment
- MetaMask or similar Web3 wallet
- Coinbase Commerce account
- Base network ETH for gas

### 1. Clone & Install

```bash
git clone https://github.com/yourusername/asx-token
cd asx-token
npm install
```

### 2. Configure Environment

Create `.env` file:

```env
PRIVATE_KEY=0xYourPrivateKeyHere
BUSINESS_WALLET=0xYourBusinessWallet
TREASURY_WALLET=0xYourTreasuryWallet
BASE_RPC_URL=https://mainnet.base.org
BASESCAN_API_KEY=YourBasescanAPIKey
```

### 3. Approve Treasury

The Purchase Processor needs permission to spend treasury tokens:

**Option A: Via BaseScan (Recommended)**

1. Go to [ASX Token on BaseScan](https://basescan.org/address/0x0c49d436f0c669Be4690CCB98c7cC6937E8EB8ae)
2. Navigate to "Contract" → "Write Contract"
3. Connect treasury wallet
4. Call `approve` function:
   - `spender`: `0xebbC50A761AF9A3A49D3f6Aa1AA038b9B2b70F40`
   - `amount`: `10000000000000000000000000` (10M tokens)
5. Confirm transaction

**Option B: Via Script**

```bash
npx hardhat run scripts/approve-processor.js --network base
```

### 4. Set Up Webhook Server

Deploy the webhook handler to process Coinbase Commerce payments:

```bash
cd webhook-server
npm install
npm start
```

**Deploy to production:**
- [Vercel](https://vercel.com) (recommended)
- [Railway](https://railway.app)
- Your own VPS

### 5. Configure Coinbase Commerce

1. Go to [Coinbase Commerce Dashboard](https://commerce.coinbase.com)
2. Settings → Webhooks
3. Add webhook URL: `https://your-server.com/coinbase-webhook`
4. Select event: `charge:confirmed`
5. Copy webhook secret to your `.env`

### 6. Update Checkout Form

Collect customer wallet address during checkout:

```html
<form>
  <label>Your Base Wallet Address (for ASX rewards):</label>
  <input 
    type="text" 
    name="wallet_address" 
    required 
    pattern="^0x[a-fA-F0-9]{40}$"
    placeholder="0x..."
  >
  <!-- Rest of checkout -->
</form>
```

---

## 🎮 Token Utility

### Current Use Cases

- **Voting Rights** - Vote on new strains to stock
- **Governance** - Vote on business decisions
- **Leaderboards** - Top holders get recognition

### Planned Features

- **Merch Redemption** - Burn tokens for branded merchandise
- **Tournament Entry** - Pay entry fees with ASX tokens
- **VIP Tiers** - 10k+ tokens = premium perks
- **Exclusive Drops** - Early access to limited strains
- **Discounts** - Token-holder exclusive deals

---

## 📁 Repository Structure

```
asx-token/
├── contracts/
│   ├── ASXRewardToken.sol          # Main token contract
│   └── SeedPurchaseProcessor.sol   # Auto-distribution system
├── scripts/
│   ├── deploy.js                   # Deployment script
│   ├── finish-transfer.js          # Complete token transfer
│   └── approve-processor.js        # Approve spending
├── webhook-server/
│   └── server.js                   # Coinbase Commerce webhook
├── test/
│   └── ASXToken.test.js           # Contract tests
├── hardhat.config.js              # Network configuration
├── .env.example                   # Environment template
└── README.md                      # This file
```

---

## 🔒 Security Features

- **OpenZeppelin Audited Contracts** - Built on battle-tested libraries
- **Non-Transferable Enforcement** - Prevents token dumping
- **ReentrancyGuard** - Protects against reentrancy attacks
- **Ownable Pattern** - Admin functions restricted to owner
- **Max Supply Cap** - Cannot mint beyond 100M tokens
- **Emergency Withdrawal** - Owner can recover stuck funds

---

## 🧪 Testing

Run the test suite:

```bash
# Compile contracts
npx hardhat compile

# Run tests
npx hardhat test

# Run tests with coverage
npx hardhat coverage

# Deploy to testnet
npx hardhat run scripts/deploy.js --network baseSepolia
```

---

## 📈 Token Economics

### Distribution

- **Treasury (Distribution Pool):** 10,000,000 ASX (10%)
- **Liquidity Pool (Future):** TBD
- **Team & Development:** TBD
- **Reserved for Growth:** 90,000,000 ASX (90%)

### Reward Mechanics

- Tokens are minted on-demand from treasury
- Distribution is automatic via smart contract
- No manual airdrops required
- Treasury can be refilled by minting (within max supply)

---

## 🛠️ Development

### Local Development

```bash
# Install dependencies
npm install

# Start local Hardhat node
npx hardhat node

# Deploy to local network (new terminal)
npx hardhat run scripts/deploy.js --network localhost

# Run webhook server locally
cd webhook-server
npm start
```

### Deploy to Mainnet

```bash
# Deploy contracts
npm run deploy:mainnet

# Verify contracts
npx hardhat verify --network base <TOKEN_ADDRESS>
npx hardhat verify --network base <PROCESSOR_ADDRESS> <CONSTRUCTOR_ARGS>
```

---

## 📞 Support & Community

- **Website:** [cannabis-seed.us](https://cannabis-seed.us)
- **Twitter:** [@seedusa](https://twitter.com/seedusa)
- **Discord:** [Join our community](#)
- **Issues:** [GitHub Issues](https://github.com/yourusername/asx-token/issues)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Built on [Base](https://base.org) - Coinbase's Layer 2
- Powered by [OpenZeppelin](https://openzeppelin.com) contracts
- Deployed with [Hardhat](https://hardhat.org)
- Integrated with [Coinbase Commerce](https://commerce.coinbase.com)

---

## ⚠️ Disclaimer

This token is a loyalty reward system and NOT an investment vehicle. ASX tokens:
- Cannot be sold or transferred
- Have no monetary value outside the ecosystem
- Are intended for community engagement only
- Are not securities or financial instruments

Always consult with legal and financial advisors before implementing token systems.

---

## 🚀 Quick Links

- [View ASX Token Contract](https://basescan.org/address/0x0c49d436f0c669Be4690CCB98c7cC6937E8EB8ae)
- [View Purchase Processor](https://basescan.org/address/0xebbC50A761AF9A3A49D3f6Aa1AA038b9B2b70F40)
- [Coinbase Commerce](https://commerce.coinbase.com)
- [Base Documentation](https://docs.base.org)
- [OpenZeppelin Docs](https://docs.openzeppelin.com)

---

**Built with 💚 for the cannabis community**

*Last Updated: October 23, 2025*
