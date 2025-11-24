# deCHAT.me & deCASH.me Whitepaper

**Version:** 1.0
**Date:** November 24, 2025
**Status:** Draft

---

## Abstract

deCHAT.me & deCASH.me represents a unified ecosystem that combines privacy-focused instant messaging with permissionless cryptocurrency payments. Built on the proven chatmail infrastructure and powered by the MobileCoin blockchain, this platform enables fast, secure, and truly decentralized communication and financial transactions.

**Core Proposition:** Every message becomes a potential payment channel. Every conversation becomes a commerce opportunity.

---

## Table of Contents

1. [Vision & Mission](#vision--mission)
2. [The Problem](#the-problem)
3. [The Solution](#the-solution)
4. [Technical Foundation](#technical-foundation)
5. [MobileCoin Integration Architecture](#mobilecoin-integration-architecture)
6. [Core Features](#core-features)
7. [Use Cases](#use-cases)
8. [Implementation Roadmap](#implementation-roadmap)
9. [Security & Privacy](#security--privacy)
10. [Economic Model](#economic-model)
11. [Ecosystem & Partnerships](#ecosystem--partnerships)
12. [Technical Specifications](#technical-specifications)
13. [Future Vision](#future-vision)

---

## Vision & Mission

### Vision

**deCentral 💬 deCASH.me ☆ natürlich dezentral ☆ open source ☆ erlaubnisfrei im p2p flow**

To create a world where private communication and permissionless payments are unified, accessible, and as simple as sending a text message.

### Mission

Build an open-source, decentralized infrastructure that:

1. **Democratizes Financial Access:** Enable anyone, anywhere to send and receive payments without gatekeepers
2. **Preserves Privacy:** Protect user communications and transactions through end-to-end encryption
3. **Eliminates Friction:** Make cross-border payments as fast and easy as sending a chat message
4. **Empowers Users:** Give people full control over their data, conversations, and money
5. **Fosters Innovation:** Provide open APIs and protocols for developers to build new financial services

### Core Principles

- **Privacy First:** No tracking, no data harvesting, no surveillance capitalism
- **Permissionless:** No accounts, no KYC, no gatekeepers
- **Decentralized:** No single point of failure or control
- **Open Source:** Transparent, auditable, community-driven
- **Interoperable:** Works with existing Delta Chat ecosystem and standard email
- **Fast:** 5-second settlement times, real-time messaging
- **Accessible:** Easy onboarding, intuitive UX

---

## The Problem

### Fragmented Communication & Payment Systems

**Current State:**
- Messaging apps and payment systems are separate silos
- Cross-border payments are slow (3-5 days), expensive (5-10% fees), and require intermediaries
- Traditional payment systems require extensive KYC, exclude billions of unbanked people
- Privacy is non-existent: financial surveillance is ubiquitous
- Centralized platforms control user data, can censor, freeze accounts, or shut down
- High barriers to entry for merchants (payment processors, banking relationships, compliance costs)

### The Cryptocurrency Gap

**Existing Solutions Fall Short:**
- **Bitcoin/Ethereum:** Slow confirmation times, high fees, poor privacy, complex UX
- **Custodial Wallets:** Reintroduce centralization, counterparty risk, KYC requirements
- **DeFi Platforms:** Complex, risky, high slippage, not suitable for everyday payments
- **Existing Crypto Messengers:** Limited adoption, poor UX, no stablecoin support, or centralized

### What's Missing

**A unified platform that:**
- Combines messaging and payments seamlessly
- Provides privacy-preserving instant payments
- Uses stablecoins for price stability
- Requires zero KYC or personal information
- Works on mobile devices with simple UX
- Is fully open source and auditable
- Integrates with existing ecosystems

---

## The Solution

### deCHAT.me 💬 deCASH.me

A unified messaging and payment platform built on proven technologies:

**Messaging Layer (deCHAT.me):**
- Based on chatmail infrastructure (battle-tested Postfix/Dovecot)
- End-to-end encrypted messaging via OpenPGP
- Instant onboarding (create-on-login)
- Compatible with Delta Chat ecosystem
- Push notifications, multi-device support
- No phone number or email required

**Payment Layer (deCASH.me):**
- Powered by MobileCoin blockchain
- Supports MOB (native token) and eUSD (USD-backed stablecoin)
- 5-second settlement times
- End-to-end encrypted transactions
- 0.0026 USD transaction fees
- Zero-knowledge proof privacy
- No blockchain analysis possible

### Inspired by Success Stories

**Signal Messenger + MobileCoin:**
- Signal integrated MobileCoin payments in 2021
- Enables users in supported regions to send/receive payments
- Demonstrates viability of messaging + payments integration
- Proves user demand for privacy-preserving payments

**Mixin Messenger:**
- Signal fork with multi-blockchain support
- Supports 100+ cryptocurrencies
- Active ecosystem with DeFi integrations
- Shows path to expanding beyond single blockchain

**Sentz.com:**
- User-friendly MobileCoin wallet
- Supports MOB, eUSD stablecoin
- Fiat on/off ramps
- Conversion to USDT, USDC
- Demonstrates practical usability of MobileCoin

### Key Differentiators

1. **Open Infrastructure:** Unlike Signal (centralized server) or Mixin (controlled network), deCHAT.me runs on decentralized chatmail relays
2. **Payment Gateway Functionality:** Server-to-server payments, merchant integrations, QR codes
3. **Interoperability:** Works with existing Delta Chat ecosystem (millions of potential users)
4. **Self-Hostable:** Anyone can run a relay, true decentralization
5. **Focus on Stablecoins:** eUSD provides price stability for real-world commerce

---

## Technical Foundation

### Current Architecture (chatmail/arcanechat)

The platform builds on the chatmail relay infrastructure, a production-ready system that powers arcanechat.me and other Delta Chat relays.

#### Core Components

**Mail Transport Layer:**
- **Postfix SMTP:** Handles email routing and delivery
- **Dovecot IMAP:** Manages mailbox storage and access
- **OpenDKIM:** DKIM signing and verification
- **Nginx:** Web server and reverse proxy
- **acmetool:** Automated TLS certificate management

**chatmaild Services (Python):**
- **doveauth:** Create-on-login authentication
- **filtermail:** Enforces OpenPGP encryption
- **chatmail-metadata:** Device tokens, push notifications
- **notifier:** Delivers push notifications via Apple/Google/Huawei
- **expire:** Automatic message and user cleanup
- **echobot:** Test bot for validation

**Infrastructure Features:**
- **Instant Onboarding:** No registration, create account on first login
- **Privacy-Preserving:** No personal data collection
- **End-to-End Encrypted:** Only OpenPGP messages allowed
- **Push Notifications:** Real-time delivery via platform providers
- **Automatic Cleanup:** Messages deleted after 20 days, users after 90 days of inactivity
- **Multi-Device Support:** IMAP enables multiple device access
- **Peer-to-Peer:** Iroh relay for P2P connections, TURN for WebRTC calls

#### Proven Scalability

- **Low Resource Requirements:** 1GB RAM, 1 CPU, 10GB storage for thousands of users
- **Battle-Tested Components:** Postfix and Dovecot have decades of production use
- **Efficient Design:** Messages stored only in transit, minimal disk usage
- **Rate Limiting:** Built-in protection against abuse

### Why This Foundation is Ideal

1. **Proven Reliability:** Postfix/Dovecot power millions of mail servers worldwide
2. **Standardized Protocols:** IMAP/SMTP ensure interoperability
3. **Modular Architecture:** Easy to add new services (like payment processing)
4. **Security First:** Multiple layers of encryption and authentication
5. **Open Source:** Fully auditable, community-driven development
6. **Existing Ecosystem:** Compatible with Delta Chat (1M+ users)

---

## MobileCoin Integration Architecture

### Why MobileCoin?

MobileCoin is uniquely suited for messaging-integrated payments:

**Technical Advantages:**
- **Fast:** ~5-second block times, near-instant finality
- **Private:** Uses CryptoNote protocol with Ring Signatures, Ring CT, and Bulletproofs
- **Mobile-First:** Designed for smartphone use with light client architecture
- **Encrypted Fog:** Allows users to receive payments even when offline
- **Small Blockchain:** Pruned design keeps blockchain size manageable
- **Low Fees:** ~0.0026 USD per transaction

**Ecosystem Advantages:**
- **Signal Integration:** Proven integration with major messenger
- **eUSD Stablecoin:** USD-backed stablecoin for price stability
- **Active Development:** Well-funded, professional team
- **Regulatory Clarity:** Working within regulatory frameworks
- **Exchange Support:** Listed on major exchanges
- **Fiat On/Off Ramps:** Via partners like Sentz

### MobileCoin Technical Overview

**Consensus Mechanism:**
- **Byzantine Fault Tolerant:** Based on Stellar Consensus Protocol (SCP)
- **Network of Validators:** Decentralized validator set
- **Fast Finality:** Blocks confirmed in ~5 seconds

**Privacy Technology:**
- **Ring Signatures:** Hides transaction sender among group
- **Ring Confidential Transactions (RingCT):** Hides transaction amounts
- **Stealth Addresses:** One-time addresses for each transaction
- **Bulletproofs:** Efficient zero-knowledge proofs for amount hiding

**Client Architecture:**
- **Light Clients:** Don't need full blockchain, can run on mobile
- **Fog Service:** Encrypted untrusted service for receiving payments
- **Consensus Validators:** Full nodes that maintain network

**Supported Assets:**
- **MOB:** Native token, used for fees and value transfer
- **eUSD:** USD-backed stablecoin, 1:1 USD peg
- **Future Tokens:** Architecture supports additional assets

### Integration Architecture

#### Component Overview

```
┌─────────────────────────────────────────────────────────────┐
│                      User Devices                           │
│           (deCASH.me App / Delta Chat Client)               │
└─────────────────┬───────────────────┬───────────────────────┘
                  │                   │
         ┌────────▼────────┐  ┌───────▼────────┐
         │  Messaging      │  │  Wallet        │
         │  (IMAP/SMTP)    │  │  (MobileCoin)  │
         └────────┬────────┘  └───────┬────────┘
                  │                   │
         ┌────────▼───────────────────▼────────┐
         │      deCHAT.me / deCASH.me Server   │
         ├─────────────────────────────────────┤
         │  Messaging Services (chatmaild)     │
         │  • doveauth                         │
         │  • filtermail                       │
         │  • chatmail-metadata                │
         │  • notifier                         │
         ├─────────────────────────────────────┤
         │  Payment Services (NEW)             │
         │  • cashmail-wallet                  │
         │  • cashmail-gateway                 │
         │  • cashmail-notifier                │
         │  • cashmail-merchant                │
         └────────┬────────────────────────────┘
                  │
         ┌────────▼────────┐
         │  MobileCoin     │
         │  Network        │
         │  (Validators)   │
         └─────────────────┘
```

#### New Services for Payment Integration

**1. cashmail-wallet**
- **Purpose:** Wallet management service
- **Responsibilities:**
  - Generate and manage MobileCoin accounts for users
  - Secure key storage (encrypted with user password)
  - Query account balances
  - Build and submit transactions
  - Track transaction history
- **Technology:**
  - MobileCoin SDK (Rust)
  - Python bindings via PyO3 or gRPC
  - Encrypted filesystem storage
- **API:** Unix socket (similar to doveauth pattern)

**2. cashmail-gateway**
- **Purpose:** Payment request and settlement service
- **Responsibilities:**
  - Generate payment QR codes
  - Create payment links
  - Monitor incoming transactions
  - Notify merchant of successful payments
  - Handle payment confirmations
- **Technology:**
  - REST API (Flask/FastAPI)
  - PostgreSQL/SQLite for payment tracking
  - MobileCoin SDK for transaction monitoring
- **Endpoints:**
  - `POST /api/payment/request` - Create payment request
  - `GET /api/payment/status/:id` - Check payment status
  - `POST /api/payment/webhook` - Merchant webhooks

**3. cashmail-notifier**
- **Purpose:** Push notifications for payment events
- **Responsibilities:**
  - Notify users of incoming payments
  - Alert merchants of successful transactions
  - Send transaction confirmations
- **Integration:**
  - Extends existing chatmail-metadata/notifier
  - Uses same push notification infrastructure

**4. cashmail-merchant**
- **Purpose:** Merchant tools and integrations
- **Responsibilities:**
  - Merchant dashboard (web interface)
  - Sales reporting and analytics
  - Fiat conversion tracking
  - Integration with external services (Sentz, exchanges)
- **Technology:**
  - Web dashboard (Vue.js/React)
  - API backend (Python/FastAPI)
  - Integration with cashmail-gateway

#### Data Flow: User-to-User Payment

1. **Sender initiates payment in chat**
   - User types: `/pay @user 10.00 eUSD` or uses payment button
   - Client app parses payment command

2. **Client requests transaction from server**
   - App sends signed request to cashmail-wallet service
   - Request includes: recipient address, amount, asset type

3. **Server builds transaction**
   - cashmail-wallet retrieves sender's keys (encrypted)
   - Builds MobileCoin transaction with Ring Signatures
   - Submits to MobileCoin network

4. **Transaction confirmed**
   - MobileCoin network validates and includes in block (~5 seconds)
   - cashmail-wallet monitors for confirmation

5. **Notification sent**
   - cashmail-notifier sends push notification to recipient
   - In-chat message confirms payment: "✓ Paid 10.00 eUSD"

6. **Recipient sees balance update**
   - App queries cashmail-wallet for new balance
   - Transaction appears in chat history

#### Data Flow: Merchant Payment (QR Code)

1. **Merchant creates payment request**
   - Merchant app calls `POST /api/payment/request`
   - Parameters: amount, currency, description, webhook URL

2. **Server generates payment request**
   - cashmail-gateway creates payment record (unique ID)
   - Generates MobileCoin payment request
   - Returns QR code data and payment link

3. **Customer scans QR code**
   - deCASH.me app decodes payment request
   - Shows payment details: amount, recipient, description
   - User confirms payment

4. **Payment submitted**
   - App builds transaction via cashmail-wallet
   - Transaction references payment request ID
   - Submits to MobileCoin network

5. **Gateway monitors for payment**
   - cashmail-gateway watches for incoming transactions
   - Matches transaction to payment request
   - Updates payment status to "confirmed"

6. **Merchant notified**
   - Webhook sent to merchant URL
   - Push notification to merchant app
   - Payment marked complete

7. **Settlement (optional)**
   - Merchant uses Sentz integration to convert eUSD → USDT/USDC
   - Or cashout via fiat off-ramp

### Security Architecture

**Key Management:**
- User keys encrypted with password-derived key (Argon2)
- Server-side encrypted storage (filesystem)
- Option for client-side key storage (advanced users)
- Mnemonic phrase backup (BIP39 compatible)

**Authentication:**
- Payment requests signed with user's MobileCoin private key
- Server validates signatures before processing
- Rate limiting to prevent abuse

**Privacy:**
- Server cannot determine transaction amounts (RingCT)
- Server cannot identify sender (Ring Signatures)
- Metadata minimization: only store necessary payment request data
- Option to use Tor for additional network privacy

**Audit Trail:**
- All payment requests logged (without amounts)
- Transaction IDs stored for dispute resolution
- User can export full transaction history
- Server logs rotated and encrypted

---

## Core Features

### Phase 1: User-to-User Payments

**In-Chat Payments:**
- Send MOB or eUSD directly in any chat conversation
- Simple commands: `/pay @user 10 eUSD` or payment button
- Transaction confirmation within 5 seconds
- Transaction history visible in chat
- Payment requests: "Request 20 eUSD for dinner"

**Wallet Management:**
- Built-in wallet (non-custodial)
- Display balance (MOB and eUSD)
- Receive address (QR code)
- Transaction history
- Backup/restore via mnemonic phrase

**Address Book Integration:**
- Each chat contact automatically has a payment address
- No need to copy/paste addresses
- Identity verification via chatmail address

### Phase 2: Merchant Solutions

**Payment Gateway API:**
- REST API for payment requests
- Generate QR codes for in-person payments
- Payment links for online sales
- Webhook notifications for payment confirmations
- SDKs for popular languages (Python, JavaScript, PHP)

**Point-of-Sale (POS):**
- Merchant app (mobile/tablet)
- Generate QR codes at checkout
- Monitor incoming payments in real-time
- Daily sales reports
- Multi-location support

**E-Commerce Integration:**
- Plugins for popular platforms (WooCommerce, Shopify, etc.)
- Shopping cart integration
- Automatic order confirmation on payment
- Refund capability

**Merchant Dashboard:**
- View all transactions
- Export reports (CSV, PDF)
- Analytics (sales trends, popular products)
- Fiat conversion tracking
- Integration with accounting software

### Phase 3: Advanced Features

**Recurring Payments:**
- Subscription support
- Automatic monthly/weekly payments
- Cancel anytime
- Use cases: memberships, donations, utility bills

**Multi-Signature Wallets:**
- Shared accounts (2-of-3, 3-of-5, etc.)
- Business accounts with multiple signers
- Enhanced security for large balances

**Escrow Service:**
- Smart escrow via trusted third party
- Use cases: freelance work, online marketplaces
- Dispute resolution

**Atomic Swaps:**
- Exchange MOB ↔ eUSD without intermediary
- Potential integration with other blockchains (via Mixin ecosystem)

**DeFi Integration (via Mixin):**
- Access to Mixin's DeFi ecosystem
- Support for 100+ cryptocurrencies
- Liquidity pools, yield farming
- Cross-chain bridges

**Chatbot Payments:**
- Bots can receive and send payments
- Automated services (tipping bots, games, productivity tools)
- Monetization for bot developers

---

## Use Cases

### Personal Use Cases

**1. Remittances:**
- **Scenario:** Maria works in the US, sends money to family in Mexico
- **Traditional:** $50 fee + 3-day wait via Western Union
- **deCASH.me:** $0.0026 fee + 5 seconds via eUSD transfer
- **Benefits:** Instant, nearly free, no intermediaries, no banks needed

**2. Splitting Bills:**
- **Scenario:** Friends at restaurant split dinner bill
- **Traditional:** Venmo (US-only), requires bank account, 1-3 days settlement
- **deCASH.me:** Instant payment in group chat, works internationally
- **Benefits:** Fast, private, no bank account needed

**3. Family Allowances:**
- **Scenario:** Parent sends weekly allowance to child
- **Traditional:** Cash or bank transfer (needs bank account)
- **deCASH.me:** Recurring payment in family chat
- **Benefits:** Teach financial literacy, no bank account needed for child, transparent

**4. Travel:**
- **Scenario:** Tourist in foreign country needs local currency
- **Traditional:** Exchange cash at poor rates, use credit card with foreign transaction fees
- **deCASH.me:** Pay directly with eUSD, merchant converts if needed
- **Benefits:** No exchange fees, works everywhere with internet

### Business Use Cases

**5. Freelance Work:**
- **Scenario:** Designer in Philippines works for client in Europe
- **Traditional:** PayPal (5% fee) or bank wire ($50 fee + 5 days)
- **deCASH.me:** Invoice via payment request link, instant settlement
- **Benefits:** Low fees, instant, no chargebacks, global access

**6. E-Commerce:**
- **Scenario:** Online store selling digital goods
- **Traditional:** Stripe (2.9% + $0.30 per transaction), chargebacks, KYC required
- **deCASH.me:** Accept eUSD payments via plugin
- **Benefits:** Lower fees, no chargebacks, privacy for customers, no KYC

**7. Street Vendor:**
- **Scenario:** Food truck at festival
- **Traditional:** Cash (insecure, needs change) or credit card reader (fees, connectivity)
- **deCASH.me:** Display QR code, customers scan and pay
- **Benefits:** No equipment needed, instant confirmation, low fees

**8. Content Creators:**
- **Scenario:** YouTuber receives tips from fans
- **Traditional:** Patreon (5-12% fee), requires bank account, monthly payouts
- **deCASH.me:** Tipping bot in community chat, instant payouts
- **Benefits:** Direct connection with fans, low fees, instant access to funds

### Emerging Market Use Cases

**9. Unbanked Populations:**
- **Scenario:** Small business owner in developing country without bank access
- **Traditional:** Cash only, limited to local customers
- **deCASH.me:** Accept global payments via smartphone
- **Benefits:** Access to global economy, no bank account needed, secure storage

**10. Inflation Hedge:**
- **Scenario:** Citizen in country with high inflation
- **Traditional:** Local currency loses 50% value per year
- **deCASH.me:** Store savings in eUSD (USD-pegged)
- **Benefits:** Preserve value, easily convertible, private

**11. Cross-Border Trade:**
- **Scenario:** Small importer buying from supplier in another country
- **Traditional:** International wire ($50 fee + 5 days + 3% forex fee)
- **deCASH.me:** Pay supplier directly in eUSD
- **Benefits:** Fast settlement, low fees, transparent exchange rates

### Developer Use Cases

**12. Payment Integration:**
- **Scenario:** Developer building new app with payments
- **Traditional:** Integrate Stripe (complex, KYC, monthly fees)
- **deCASH.me:** Use open API, no KYC, no monthly fees
- **Benefits:** Simple integration, lower barriers, privacy-preserving

**13. Chatbot Monetization:**
- **Scenario:** Developer creates useful bot (translation, reminders, etc.)
- **Traditional:** Hard to monetize small utilities
- **deCASH.me:** Bot accepts micro-payments (e.g., $0.01 per translation)
- **Benefits:** Enable micro-transaction business models

---

## Implementation Roadmap

### Phase 1: Foundation (Months 1-3)

**Milestone 1.1: Rebranding**
- Rename arcanechat.me → deCHAT.me
- Update all branding, website, documentation
- Maintain compatibility with Delta Chat ecosystem
- Deploy rebranded servers

**Milestone 1.2: MobileCoin SDK Integration**
- Research and select MobileCoin SDK (Rust core + Python bindings)
- Set up development environment
- Create test wallet functionality
- Integration testing with MobileCoin testnet

**Milestone 1.3: Wallet Service (cashmail-wallet)**
- Implement wallet creation and key management
- Encrypted key storage
- Balance queries
- Transaction building and submission
- Basic CLI for testing

**Milestone 1.4: Client App Updates**
- Fork Delta Chat Android client (or extend ArcaneChat)
- Add wallet UI (balance, receive address)
- Integrate with cashmail-wallet API
- Testnet release for early testing

### Phase 2: User-to-User Payments (Months 4-6)

**Milestone 2.1: In-Chat Payment Commands**
- Implement `/pay` command parsing
- Payment confirmation UI
- Transaction history display
- Error handling and user feedback

**Milestone 2.2: Payment Notifications**
- Extend chatmail-notifier for payment events
- Push notifications for incoming payments
- In-chat payment confirmations
- Transaction status updates

**Milestone 2.3: Security Hardening**
- Implement key encryption (Argon2)
- Secure key derivation from user password
- Rate limiting for transaction requests
- Security audit (internal)

**Milestone 2.4: Mainnet Beta**
- Deploy to MobileCoin mainnet
- Limited beta release (invite-only)
- Bug bounty program
- Community testing

### Phase 3: Merchant Solutions (Months 7-9)

**Milestone 3.1: Payment Gateway (cashmail-gateway)**
- REST API for payment requests
- QR code generation
- Payment status monitoring
- Webhook system

**Milestone 3.2: Merchant App**
- Mobile POS app (Android)
- Generate payment QR codes
- View incoming payments
- Basic reporting

**Milestone 3.3: E-Commerce Plugins**
- WooCommerce plugin
- Shopify app (if approved)
- Open-source payment gateway module
- Documentation and examples

**Milestone 3.4: Merchant Dashboard**
- Web-based dashboard
- Transaction history and export
- Analytics and reporting
- Settings and configuration

### Phase 4: Ecosystem Expansion (Months 10-12)

**Milestone 4.1: Fiat Integration (Sentz)**
- Partnership with Sentz
- eUSD ↔ USDT/USDC conversion
- Fiat off-ramp integration
- KYC for cash-out (optional)

**Milestone 4.2: Mixin Integration**
- Explore Mixin SDK integration
- Cross-chain support (BTC, ETH, etc.)
- Access to Mixin DeFi ecosystem
- Liquidity pools

**Milestone 4.3: Developer SDK**
- Python SDK for payment gateway
- JavaScript/TypeScript SDK
- PHP SDK
- Comprehensive documentation
- Code examples and tutorials

**Milestone 4.4: Public Launch**
- Full public release (mainnet)
- Marketing campaign
- Onboarding documentation
- Community support channels

### Phase 5: Advanced Features (Months 13+)

**Long-Term Features:**
- Recurring payments
- Multi-signature wallets
- Escrow service
- Atomic swaps
- Advanced DeFi integrations
- iOS app
- Desktop apps (Windows, macOS, Linux)
- Additional blockchain support

---

## Security & Privacy

### Threat Model

**Assumptions:**
- User devices may be compromised
- Network traffic may be monitored (ISP, government)
- Server may be honest-but-curious
- MobileCoin blockchain is secure
- Validators may collude but not majority

**Protection Goals:**
1. **Message Privacy:** Adversary cannot read message content
2. **Payment Privacy:** Adversary cannot determine payment amounts or sender
3. **Metadata Privacy:** Minimize leakage of communication patterns
4. **Censorship Resistance:** System cannot be easily blocked or shut down
5. **User Anonymity:** No requirement for real identity

### Security Mechanisms

#### Messaging Layer

**End-to-End Encryption (OpenPGP):**
- All messages encrypted client-to-client
- Server cannot read message content
- Forward secrecy via ephemeral keys (in Delta Chat implementation)
- Authenticity via digital signatures

**Authentication:**
- Password-based authentication (SHA512-CRYPT)
- No phone number or email verification required
- Create-on-login semantics

**Transport Security:**
- TLS 1.3 for all connections
- DKIM signing for email authentication
- Certificate pinning (optional in client)

#### Payment Layer

**Transaction Privacy (MobileCoin):**
- **Ring Signatures:** Transaction inputs grouped with decoys, sender hidden among group
- **RingCT:** Transaction amounts encrypted, only sender and receiver know amounts
- **Stealth Addresses:** One-time addresses for each transaction, receiver identity hidden
- **Bulletproofs:** Prove transaction validity without revealing amounts

**Key Management:**
- Private keys encrypted with user password (Argon2id)
- Keys never leave device in plaintext
- Optional client-side key storage (full non-custodial)
- Mnemonic phrase backup (BIP39)
- Secure key deletion on account removal

**Network Privacy:**
- **Fog Service:** Encrypted untrusted service for receiving payments, user's view key kept private
- **Tor Support:** Route traffic through Tor for additional anonymity
- **No IP Logging:** Server does not log IP addresses

**Transaction Authorization:**
- All payment requests signed with private key
- Signature verification before processing
- Replay protection (nonce/timestamp)

### Operational Security

**Server Security:**
- Minimal attack surface (only necessary services exposed)
- Regular security updates
- Encrypted storage for sensitive data
- Access control and audit logs
- DDoS protection

**Rate Limiting:**
- Transaction rate limits per user
- API rate limiting for payment gateway
- Protection against spam and abuse

**Monitoring:**
- Prometheus metrics for service health
- Anomaly detection for unusual activity
- Alerting for security events

**Incident Response:**
- Security vulnerability disclosure policy
- Bug bounty program
- Regular security audits
- Transparent communication on security issues

### Privacy Considerations

**What the Server Knows:**
- User's chatmail address (e.g., user@dechat.me)
- User's encrypted private keys
- User's device tokens (for push notifications)
- User's IP address (during connection, not logged)
- Payment request metadata (amounts, timestamps)
- When user last logged in

**What the Server Does NOT Know:**
- Message content (end-to-end encrypted)
- Transaction amounts (RingCT)
- Transaction sender (Ring Signatures)
- Transaction receiver's identity (Stealth Addresses)
- User's real identity (no KYC)

**What MobileCoin Network Knows:**
- Encrypted transaction data
- Transaction graph (obfuscated by Ring Signatures)
- Transaction timing

**What MobileCoin Network Does NOT Know:**
- Sender identity
- Receiver identity
- Transaction amounts
- What the transaction is for

**Comparison with Competitors:**

| Feature | deCASH.me | Signal | Mixin | Venmo |
|---------|-----------|--------|-------|-------|
| Message Encryption | ✅ E2EE | ✅ E2EE | ✅ E2EE | ❌ None |
| Payment Privacy | ✅ Zero-knowledge | ✅ Zero-knowledge | ❌ Transparent | ❌ Public by default |
| No Phone Number | ✅ Optional | ❌ Required | ❌ Required | ❌ Required |
| Decentralized | ✅ Yes | ❌ Centralized | ⚠️ Federated | ❌ Centralized |
| Open Source | ✅ Yes | ✅ Yes | ❌ Closed | ❌ Closed |
| Self-Hostable | ✅ Yes | ❌ No | ❌ No | ❌ No |
| Censorship Resistant | ✅ High | ⚠️ Medium | ⚠️ Low | ❌ Low |

---

## Economic Model

### Fee Structure

**Transaction Fees:**
- **MobileCoin Network Fee:** ~0.0026 USD (paid in MOB)
- **deCASH.me Service Fee:** 0% initially (may introduce optional premium features)
- **Comparison:**
  - Credit cards: 2.9% + $0.30
  - PayPal: 2.9% + $0.30
  - Bank wire: $25-50
  - Western Union: 5-10%

**Value Proposition:**
- **For Users:** Nearly free payments, instant settlement, privacy
- **For Merchants:** 99% lower fees than traditional payment processors

### Revenue Model (Sustainability)

**Phase 1: Open Source / Non-Profit**
- Donations from users and merchants
- Grants from crypto foundations (MobileCoin, Ethereum Foundation, etc.)
- Sponsorships from privacy-focused organizations

**Phase 2: Value-Added Services**
- **Premium Merchant Features:**
  - Advanced analytics dashboard
  - Priority support
  - Custom branding
  - Higher transaction limits
- **Hosting Services:**
  - Managed relay hosting for organizations
  - White-label solutions
  - Enterprise support contracts

**Phase 3: Ecosystem Services**
- **Fiat Conversion Fee:** Small percentage on fiat off-ramp (via Sentz partnership)
- **DeFi Integration Fee:** Revenue share from DeFi integrations
- **Developer Platform:** Paid API tiers for high-volume integrations

**No Value Extraction from Core Payments:**
- Core user-to-user payments remain free (only network fee)
- No rent-seeking behavior
- Sustainability through value-added services

### Token Economics (eUSD)

**eUSD Stablecoin:**
- **Peg:** 1 eUSD = 1 USD
- **Backing:** Fully reserved, audited USD deposits
- **Issuer:** MobileCoin Foundation
- **Redemption:** Via authorized partners (Sentz, exchanges)
- **Use Case:** Primary payment token (stable value)

**MOB Token:**
- **Purpose:** Pay network fees, store of value
- **Volatility:** Subject to market fluctuations
- **Use Case:** Secondary payment token, speculation

**Recommendation for Users:**
- Use eUSD for everyday payments (stable value)
- Use MOB for fees and long-term holding

---

## Ecosystem & Partnerships

### MobileCoin Ecosystem

**MobileCoin Foundation:**
- Core blockchain development
- Grant program for ecosystem projects
- Marketing and adoption initiatives

**Signal Messenger:**
- Established integration (proof of concept)
- Large user base (40M+ users)
- Potential for cross-promotion

**Sentz.com:**
- User-friendly MobileCoin wallet
- Fiat on/off ramps
- Conversion to USDT, USDC
- Potential integration partner for cash-out

**MobileCoin Exchanges:**
- FTX (now defunct, seek alternatives)
- Coinbase (pending)
- Regional exchanges
- DEX integrations

### Delta Chat Ecosystem

**Delta Chat Core:**
- Messaging client (1M+ users)
- Multi-platform (Android, iOS, Desktop)
- Interoperable with any email server
- Potential to integrate deCASH.me as default payment option

**Other Chatmail Relays:**
- Dozens of public chatmail servers
- Opportunity for federated payment network
- Cross-relay payments

**Bot Ecosystem:**
- Existing bots for games, utilities, etc.
- Opportunity for payment-enabled bots
- Developer community

### Mixin Ecosystem

**Mixin Messenger:**
- Signal fork with crypto payments
- Supports 100+ blockchains
- Active DeFi ecosystem
- Potential bridge for multi-chain support

**Mixin Network:**
- Decentralized gateway to blockchains
- Fast cross-chain transactions
- Smart contract platform (Mixin Virtual Machine)
- Liquidity pools and DeFi protocols

**Integration Opportunities:**
- Use Mixin as bridge for BTC, ETH, USDT, etc.
- Access to Mixin's DeFi ecosystem
- Cross-chain atomic swaps
- Shared user base

### Strategic Partnerships

**Target Partners:**
- **E-Commerce Platforms:** WooCommerce, Shopify, Magento
- **Payment Processors:** Integrate as alternative to Stripe
- **NGOs:** Use for international aid and remittances
- **Developer Tools:** Integration with GitHub, GitLab for sponsorships
- **Content Platforms:** Tips for creators (similar to Patreon)

---

## Technical Specifications

### System Requirements

**Server (deCHAT.me / deCASH.me Relay):**
- **OS:** Debian 12 or Ubuntu 22.04 LTS
- **CPU:** 2 cores minimum (4 cores recommended for production)
- **RAM:** 2GB minimum (4GB recommended)
- **Storage:** 20GB SSD minimum (depends on user count)
- **Network:** Static IP, ports 25, 80, 443, 465, 587, 993 open
- **DNS:** Full control over domain

**Client (Mobile App):**
- **Android:** Android 8.0+ (API level 26+)
- **iOS:** iOS 14+ (future)
- **Storage:** 100MB+ free space
- **Network:** Internet connection (WiFi or cellular)

**Developer Environment:**
- **Python:** 3.11+
- **Rust:** 1.70+ (for MobileCoin SDK)
- **Node.js:** 18+ (for web development)
- **Git:** For version control

### API Specifications

#### cashmail-wallet API (Internal)

**Unix Socket:** `/run/cashmail/wallet.socket`

**Protocol:** JSON-RPC over Unix socket (similar to doveauth)

**Methods:**
```json
// Create new wallet
{
  "method": "create_wallet",
  "params": {
    "user": "user@dechat.me",
    "password": "user_password"
  }
}

// Get balance
{
  "method": "get_balance",
  "params": {
    "user": "user@dechat.me",
    "token": "auth_token"
  }
}

// Send payment
{
  "method": "send_payment",
  "params": {
    "user": "user@dechat.me",
    "token": "auth_token",
    "recipient": "recipient_public_address",
    "amount": "10.50",
    "asset": "eUSD"
  }
}

// Get transaction history
{
  "method": "get_transactions",
  "params": {
    "user": "user@dechat.me",
    "token": "auth_token",
    "limit": 50,
    "offset": 0
  }
}
```

#### cashmail-gateway API (Public)

**Base URL:** `https://decash.me/api/v1`

**Authentication:** API key (for merchants) or user token

**Endpoints:**

```http
POST /payment/request
# Create payment request
# Request:
{
  "amount": "25.00",
  "currency": "eUSD",
  "description": "Coffee and pastry",
  "webhook_url": "https://merchant.com/webhook"
}
# Response:
{
  "payment_id": "req_abc123",
  "amount": "25.00",
  "currency": "eUSD",
  "status": "pending",
  "qr_code": "data:image/png;base64,...",
  "payment_uri": "mobilecoin:?address=...&amount=25.00",
  "expires_at": "2025-11-24T12:00:00Z"
}

GET /payment/status/:payment_id
# Check payment status
# Response:
{
  "payment_id": "req_abc123",
  "status": "confirmed",
  "amount": "25.00",
  "currency": "eUSD",
  "confirmed_at": "2025-11-24T10:30:15Z",
  "tx_hash": "abc123def456..."
}

POST /payment/webhook
# Webhook sent to merchant
{
  "event": "payment.confirmed",
  "payment_id": "req_abc123",
  "amount": "25.00",
  "currency": "eUSD",
  "confirmed_at": "2025-11-24T10:30:15Z",
  "tx_hash": "abc123def456..."
}
```

### Data Storage

**Filesystem Layout:**
```
/home/vmail/mail/user@dechat.me/
├── password                    # SHA512-CRYPT password hash
├── enforceE2EEincoming         # Encryption enforcement flag
├── wallet/                     # MobileCoin wallet data
│   ├── keys.enc                # Encrypted private keys
│   ├── view_key.enc            # Encrypted view key
│   └── account_data.json       # Account metadata
├── cur/, new/, tmp/            # Maildir folders
└── metadata.json               # Push notification tokens, etc.
```

**Database (for cashmail-gateway):**
- **Type:** PostgreSQL or SQLite
- **Tables:**
  - `payment_requests` (payment_id, amount, currency, status, created_at, expires_at)
  - `merchants` (merchant_id, api_key, webhook_url, settings)
  - `transactions` (tx_id, payment_id, tx_hash, confirmed_at)

### Performance Benchmarks (Target)

**Messaging:**
- **Account Creation:** < 100ms
- **Message Delivery:** < 500ms
- **Push Notification Latency:** < 2 seconds

**Payments:**
- **Transaction Building:** < 200ms
- **Transaction Submission:** < 1 second
- **Transaction Confirmation:** < 5 seconds (MobileCoin block time)
- **Balance Query:** < 100ms

**Scalability:**
- **Target:** 10,000 active users per relay
- **Payment Volume:** 1,000 transactions per day per relay
- **Storage Growth:** ~1GB per 1,000 users per month

---

## Future Vision

### Short-Term (1 year)

- Stable user-to-user payments
- Basic merchant integrations
- Active community of early adopters
- Integration with Sentz for fiat off-ramps
- Security audits and bug bounties
- iOS app release

### Medium-Term (2-3 years)

- Widespread adoption by freelancers and small businesses
- E-commerce plugin ecosystem
- Multi-chain support (via Mixin)
- DeFi integrations (lending, staking, etc.)
- Recurring payments and subscriptions
- Developer platform with rich SDK

### Long-Term (5+ years)

**Vision: The WhatsApp of Payments**

- Hundreds of thousands of users
- Default payment method for online gig economy
- Integration with major e-commerce platforms
- Bank account replacement for unbanked populations
- Cross-border remittances use case (competitor to Western Union)
- Regulatory acceptance and compliance frameworks
- Foundation/DAO governance model

**Technical Evolution:**

- Layer-2 scaling solutions for higher throughput
- Smart contracts for complex financial instruments
- Integration with CBDCs (Central Bank Digital Currencies)
- Quantum-resistant cryptography
- Advanced privacy features (Zcash Sapling integration?)

**Ecosystem Evolution:**

- Federated payment network (multiple relay operators)
- Cross-relay settlement protocols
- Interoperability with other privacy-focused payment systems
- Open standards for messaging + payment integration

**Social Impact:**

- Financial inclusion for billions of unbanked
- Reduce remittance costs globally (save billions in fees)
- Enable new business models (micro-transactions, micro-work)
- Preserve privacy in age of financial surveillance
- Resist censorship and authoritarian control

---

## Conclusion

deCHAT.me & deCASH.me represents a paradigm shift in how we think about communication and payments. By combining the privacy and decentralization of chatmail with the speed and security of MobileCoin, we create a platform that empowers individuals and businesses to transact freely, privately, and efficiently.

**This is not just a payment app. It's a financial freedom movement.**

### Why Now?

- **Technology is Ready:** MobileCoin has proven itself in Signal, infrastructure is mature
- **Market is Ready:** Crypto adoption accelerating, stablecoins gaining traction
- **World Needs It:** Financial surveillance increasing, privacy under attack, billions unbanked
- **We Can Do It:** Open-source community, proven chatmail infrastructure, passionate team

### Call to Action

**For Users:**
- Join the beta program
- Spread the word about privacy-preserving payments
- Provide feedback and report bugs

**For Merchants:**
- Integrate deCASH.me as a payment option
- Save on fees, get faster settlement
- Support the open-source movement

**For Developers:**
- Contribute to the codebase (GitHub: deCASHme/server)
- Build integrations and plugins
- Create payment-enabled bots and services

**For Investors/Donors:**
- Support the project through donations
- Fund development grants
- Help us achieve financial sustainability

**For Partners:**
- Explore integration opportunities
- Join the ecosystem
- Let's build the future of payments together

---

## Contact & Resources

**Website:** https://decash.me (forthcoming)
**GitHub:** https://github.com/deCASHme/server
**Documentation:** https://docs.decash.me (forthcoming)
**Forum:** TBD
**Email:** contact@decash.me

**Based on:**
- **Chatmail Project:** https://github.com/deltachat/chatmail
- **Delta Chat:** https://delta.chat
- **MobileCoin:** https://mobilecoin.com
- **Sentz:** https://sentz.com
- **Mixin:** https://mixin.one

---

## Appendix A: Glossary

- **Chatmail:** Privacy-focused email relay optimized for instant messaging
- **Delta Chat:** Messaging app that uses email as transport
- **MobileCoin (MOB):** Privacy-preserving cryptocurrency optimized for mobile
- **eUSD:** USD-backed stablecoin on MobileCoin blockchain
- **Ring Signature:** Cryptographic technique that hides sender among group
- **RingCT:** Ring Confidential Transactions, hides transaction amounts
- **Stealth Address:** One-time address for each transaction
- **Bulletproof:** Efficient zero-knowledge proof
- **Fog Service:** Encrypted service for receiving payments while offline
- **E2EE:** End-to-End Encryption
- **IMAP:** Internet Message Access Protocol
- **SMTP:** Simple Mail Transfer Protocol
- **OpenPGP:** Open standard for email encryption
- **DKIM:** DomainKeys Identified Mail
- **KYC:** Know Your Customer (identity verification)
- **Fiat:** Traditional government-issued currency (USD, EUR, etc.)
- **Off-ramp:** Converting cryptocurrency to fiat currency

---

## Appendix B: Comparison Matrix

| Feature | deCASH.me | Signal + MOB | Mixin | PayPal | Venmo | Wise |
|---------|-----------|--------------|-------|--------|-------|------|
| **Messaging** | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| **Payments** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **E2E Encrypted Messages** | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| **Private Payments** | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Decentralized** | ✅ | ❌ | ⚠️ | ❌ | ❌ | ❌ |
| **Open Source** | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Self-Hostable** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **No Phone Number** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **No KYC** | ✅ | ✅* | ⚠️ | ❌ | ❌ | ❌ |
| **Stablecoins** | ✅ eUSD | ✅ eUSD | ✅ Multi | ❌ | ❌ | ❌ |
| **Transaction Fee** | $0.0026 | $0.0026 | Varies | 2.9% | 3% | 0.5% |
| **Settlement Time** | 5 sec | 5 sec | <1 min | Instant | Instant | 1-2 days |
| **Merchant Tools** | ✅ | ❌ | ⚠️ | ✅ | ❌ | ✅ |
| **Global Access** | ✅ | ⚠️ | ✅ | ⚠️ | ❌ US-only | ✅ |
| **Fiat Off-ramp** | ✅ Sentz | ✅ Varies | ✅ Built-in | ✅ | ✅ | ✅ |
| **Censorship Resistant** | ✅ High | ⚠️ Medium | ⚠️ Low | ❌ | ❌ | ❌ |

*Signal KYC: No KYC for payments, but phone number required for account

---

## Appendix C: Technical References

**MobileCoin Documentation:**
- Main Site: https://mobilecoin.com
- GitHub: https://github.com/mobilecoinofficial/mobilecoin
- Developer Docs: https://developers.mobilecoin.com
- Whitepaper: https://mobilecoin.com/whitepaper.pdf

**Chatmail Documentation:**
- GitHub: https://github.com/deltachat/chatmail
- Specification: https://github.com/deltachat/chatmail/blob/main/ARCHITECTURE.md

**Delta Chat:**
- Main Site: https://delta.chat
- Core Repo: https://github.com/deltachat/deltachat-core-rust
- Specifications: https://github.com/deltachat/deltachat-core-rust/tree/main/standards

**Cryptography:**
- CryptoNote: https://cryptonote.org/whitepaper.pdf
- Ring Signatures: https://en.wikipedia.org/wiki/Ring_signature
- Bulletproofs: https://crypto.stanford.edu/bulletproofs/

**Related Projects:**
- Sentz: https://sentz.com
- Mixin: https://mixin.one
- Signal Payments: https://signal.org/blog/signal-payments-beta-mobilecoin/

---

**END OF WHITEPAPER**

---

*This whitepaper is a living document and will be updated as the project evolves. Last updated: November 24, 2025*
