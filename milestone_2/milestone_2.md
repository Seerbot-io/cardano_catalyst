# Key Features

### Backend Infrastructure Development
The Backend Infrastructure for Seerbot has been significantly enhanced in Milestone 2. The focus was on building a secure, scalable, and modular system capable of supporting real-time data ingestion, AI-driven analysis, and complex transaction processing for the Cardano ecosystem.

- **Objective**: Develop a secure and scalable backend for authentication, transaction processing, AI/TA queries, and data collection.
- **Achievements**:
    - Validated and secure API endpoints.
    - Public API documentation available for integration.
    - Successful connection and interaction with Lace and Eternl wallets.
    
**Evidence**:
- [Public API Documentation](https://api.seerbot.io/docs)
- [API Example (JSON)](https://drive.google.com/file/d/1abbz4bX2aKq0eSBdLbCUGLq7A15SAa18/view?usp=sharing)
- [Video Demo (Endpoints)](https://drive.google.com/file/d/1QFf5ifdsLjatP3m-Hv0UlUNLW-GPmtF9/view?usp=sharing)

### Product MVP Implementation
The Minimum Viable Product (MVP) has been successfully implemented, bringing together the core functionalities of Seerbot into a cohesive user experience. This includes an integrated AI Assistant, live technical analysis tools, and on-chain swap capabilities.

- **Objective**: Complete the MVP including AI Assistant, Minswap integration, wallet connection, and user interface.
- **Achievements**:
    - **Integrated AI Assistant & TA Tools**: Natural language interaction combined with indicator analysis (RSI, ADX, PSAR).
    - **Wallet Connectivity**: Seamless integration with popular Cardano wallets (Lace, Eternl).
    - **Mainnet Swap**: Fully functional swap feature using Minswap SDK, supporting multiple trading pairs.

**Evidence**:
- [MVP Product Link](https://seerbot.io/analysis)
- [Product Demo Video](https://drive.google.com/file/d/1bvNqrO4iTwuIY6kR1gLE60o8TCN9PWBt/view?usp=sharing)
- [GitHub: Minswap API Usage](https://github.com/Seerbot-io/catalyst-submit/blob/catalyst-submit/services/swapApi.ts)

---

# System Architecture
The Seerbot architecture utilizes a modular, component-based design to ensure high availability and ease of maintenance.

### I. API Layer
The API is designed as a set of focused HTTPS endpoints that serve as the interface for price data, AI sentiment analysis, and trading utilities. It favors a micro-component approach for scalability.

### II. Data Pipelines
Reliable ingestion and normalization of on-chain data form the foundation of our signal generation.
- **Ingestion**: Scheduled crawlers for market ticks/bars.
- **Enrichment**: Real-time technical indicator calculations (RSI, ADX, PSAR).
- **Materialization**: Aggregated tables for low-latency frontend queries.

---

# Proof of Achievement (PoA)
### Milestone Status: Approved
- **Project Completion**: 75%
- **Approved Date**: January 26, 2026

### Verified Transactions
Experimental transactions to verify the end-to-end swap flow on Cardano mainnet:
- [Order Transaction](https://cardanoscan.io/transaction/37dd0261100c05c4b8f0040cee1929f6058b35ce9f68b3e00c61eb22d5a95771?tab=summary)
- [Execution Transaction](https://cardanoscan.io/transaction/e227722dae7e548274238820fb6ac3f6a13868a0e9dc2325048bb8b583fe98b0?tab=utxo)

### Docs
- [Project Catalyst Milestone 2 Page](https://milestones.projectcatalyst.io/projects/1400111/milestones/2/poa-2)
