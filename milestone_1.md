
# Key Features
### Technical Analysis
The Technical Analysis module is one of the core components of SeerBOT, providing users with real-time analytical insights into tokens on the Cardano ecosystem. This module focuses on computing key technical indicators, displaying them directly on price charts, and generating trend suggestions that help users quickly identify potential uptrend or downtrend signals.

SeerBOT continuously processes on-chain price data to calculate three widely used indicators: Relative Strength Index (RSI), Average Directional Index (ADX), and Parabolic Stop and Reverse (PSAR). These indicators are overlayed on the interactive trading chart, enabling users to visually analyze token movements.

- **PSAR & DCA Strategy Summary:**

  This Parabolic SAR + DCA strategy enters long positions when the Parabolic SAR flips from downtrend (-1) to uptrend (1), executing at the open price of the signal candle. While in a position, it adds $500 when price moves 5% higher or lower than the initial entry (one DCA per direction). It exits at the open price when either the SAR flips back to downtrend or a 5% take-profit is reached. All trades use a fixed $500 per entry, with entries and exits at open prices to simulate realistic execution.
  
  [Strategy backtest result](./resources/backtests/cardano_backtest/Report_ADX_and_DCA_Strategy)
  
- **ADX & DCA Strategy Summary:**

  This strategy enters long positions when ADX > 25, +DI > -DI, and RSI < 70, using a fixed $500 entry at the close. It adds a $500 DCA when price moves 5% above or below the first entry (once per direction). Exits occur at a 5% take profit or when ADX > 25, -DI > +DI, and RSI > 30. Starting capital is $10,000, and all trades execute at close prices. The goal is to capture strong trends with ADX while using DCA to improve average entry prices during volatility.
 
  [Strategy backtest result](./resources/backtests/cardano_backtest/Report_ADX_and_DCA_Strategy/)

- **RSI14 & DCA Strategy:**

  RSI14 & DCA Strategy uses RSI14 ≤ 30 for initial entries and DCA (max 3 times) on red candles with RSI < 30, with $500 fixed positions and $10,000 initial capital. Exits at +5% take profit, -2.5% stop loss, or RSI ≥ 70. Current performance: 3 profitable, 21 loss-making reports. Issues: 1H timeframe losses, early exits (5% TP), no stop loss, unlimited DCA, no trend/volume filters. Proposed improvements: avoid 1H (use 4H+), raise TP to 8–10%, add 4% stop loss, tighten RSI to ≤25/≥75, limit DCA to 3, add EMA20 trend filter (price not >5% below EMA20), require volume ≥1.2x MA, add multi-timeframe confirmation, and implement trailing stops (breakeven at 3% profit, 2% trailing at 5%). Expected: higher average profit, fewer losses, better win rate, reduced drawdown.

  [Strategy backtest result](./resources/backtests/cardano_backtest/Report_RSI14_Signals_and_DCA_Strategy/)

### On-chain Swap
The On-chain Swap module enables users to execute token swaps directly within the SeerBOT trading interface, without leaving the platform. This feature is fully integrated with the Minswap SDK, leveraging the liquidity pools and routing logic provided by the Cardano DeFi ecosystem.\
**Trade Request**: Users specify the input token, output token, and swap amount directly within SeerBOT’s UI.\
**Wallet Interaction*: The transaction is presented to the user’s connected Cardano wallet (Lace, Eternl). Users review and sign the transaction on-chain.\
**Execution & Confirmation**: Once submitted to the blockchain, SeerBOT displays real-time transaction status (pending, confirmed) and final swap results are shown inside the interface.

- Native on-chain execution: All swaps are executed directly on Cardano, ensuring transparency and security.\
Real-time price and slippage estimates: Pulled directly from Minswap liquidity pools.\
- Seamless UX: No need to switch platforms; users can analyze and execute trades in a single environment.\
- Error handling: Clear prompts when liquidity is insufficient, price impact is too high, or the wallet fails to sign.


### AI Chatbot
The AI Chatbot module is an intelligent assistant built specifically to answer user questions related to tokens and market dynamics within the Cardano ecosystem. While powered by the ChatGPT API, it is custom-tuned for Cardano assets, ensuring responses are focused, accurate, and relevant.\

Users can ask natural-language questions such as:

“What is the price of token X?”/
“How is the supply distribution of project Y?”\
“Are there any recent updates for this project on Cardano?”\
The chatbot responds with concise, domain-specific information curated for ADA-based tokens.

**AI Chatbot Feature:**
- Cardano-specialized responses: Optimized for ADA ecosystem knowledge.
- Integrated with Technical Analysis: Can reference RSI, ADX, PSAR, or trend suggestions.
- User-friendly: Supports natural language, enabling both beginners and experienced traders to interact with market data effortlessly.
- Real-time contextual awareness: Chatbot can fetch and summarize token metrics currently available in SeerBOT.




# System architecture 
![system architecture](resources/images/system_architecture.drawio.png)

## Web UI

### User flow:
 ![User flow](resources/images/SeerBOT_User%20Flow.png)


## Backend
### Data flow:
![Data flow](resources/images/data_flow.drawio.png)

### I. API:
#### 1.1. Objective
Provide a compact, modular backend that exposes focused HTTPS API endpoints to support Cardano Catalyst workflows: market/pricing data, search and lookup utilities, AI-assisted analysis, trading-related helpers, and conversational interfaces. The design favors small, single-responsibility components that are easy to extend and integrate.

#### 1.2. Solutions
The project implements the objective by dividing responsibilities into independent components. Each component offers a clear API path for one area of functionality (data, analysis, search, chatbot). Components are orchestrated together by a minimal application entrypoint so they can run singly or as a combined service. The source code will be organized as a monorepo, making it easier to develop and maintain small projects.

#### 1.3. Features
- Modular component-based design with clear separation of concerns.
- Minimal application to combine components into a single running service.
- The project architecture is designed to be easily extended and add more features.
- Endpoints will cover features: 
  - pricing/market data
  - Technical analysis
  - AI chat bot

#### 1.4. Quality Assurance
- Tests, CI/CD, and linting to ensure code quality and reliability.
- Monitoring and alerts for production health.

### II. Data pipelines:
#### 2.1. Objective
Provide a reliable, maintainable data pipeline that ingests market and reference data, performs deterministic transforms and enrichment, and persists all outputs to the project database as the single source of truth. The pipeline emphasizes repeatability, observability, and safe scheduling so API endpoints can serve ready-to-query data on demand.

#### 2.2. Solutions
The data pipeline implements the following cooperating responsibilities:

- Ingestion and normalization: a scheduled crawler retrieves market/exchange data, normalizes timestamps and numeric fields, validates rows, and writes them to the database.
- Reference synchronization: a periodic sync refreshes currency/token metadata and market quotes so lookups and UIs use current information.
- Transform and materialization: windowed aggregations and enrichment transforms produce materialized tables and summaries used for low-latency queries.

An orchestration layer schedules these jobs, enforces dependency order (for example, refresh reference data before transforms), and handles retries and run metadata for observability.

#### 2.3. Features & Component roles (concise)

- Crawler / Ingest — scheduled, idempotent collection and normalization of market ticks/bars.
- Reference sync — periodic refresh of asset metadata and quotes for reliable lookups.
- Transform / Materialization — windowed aggregations and summaries for low-latency queries.
- Orchestration & reliability — job scheduling, dependency enforcement, retries, and run metadata.
- Data quality & observability — lightweight checks and metrics for dashboards and alerts.
- Persistence & serving — all outputs persisted to the database as the single source of truth.

#### 2.4. Quality Assurance
- Unit tests for transform logic to ensure deterministic correctness.
- Integration tests validating end-to-end ingestion, storage, and query workflows over a representative sample window.
- Logging and metrics: scheduled jobs emit start/stop, processed-record counts, and error signals for alerting.
- Safe deploys: schema changes and data migrations are rolled out in small, reversible steps and validated on staging data before production.



### Docs
[Trade algorithm and AI integration plan](https://docs.google.com/document/d/1MLxm0QokDywJMqHItA40bd2RkJ77MbrSO9cghgu6C-c/edit?usp=sharing)
