# Prediction Market Agents: Deep Reference

## Table of Contents
1. [Kelly Criterion Deep Dive](#kelly-criterion)
2. [Strategy Suitability Matrix](#strategy-matrix)
3. [Complete Project Directory](#project-directory)
4. [Platform Comparison](#platform-comparison)
5. [Regulatory Landscape](#regulatory-landscape)

---

## 1. Kelly Criterion Deep Dive <a name="kelly-criterion"></a>

### Full Formula
```
f* = (bp - q) / b
```
- f* = fraction of capital to bet
- b = net odds (payout on a $1 bet, excluding the $1 stake)
- p = probability of winning
- q = 1 - p = probability of losing

### Prediction Market Simplified Form
```
f* = (p - market_price) / (1 - market_price)
```
- p = estimated true probability of the event
- market_price = current market implied probability (contract price)

### Example Calculation
If you believe an event has a 70% true probability (p = 0.70) and the market prices it at 55% (market_price = 0.55):
```
f* = (0.70 - 0.55) / (1 - 0.55) = 0.15 / 0.45 = 0.333
```
Kelly suggests betting 33.3% of capital. In practice, most professionals use fractional Kelly (1/4 to 1/2 Kelly) to account for estimation error.

### Why Kelly Breaks Down in Practice
1. Requires accurate probability estimation (hardest part)
2. Assumes infinite repeated games (prediction markets have finite opportunities)
3. Ignores correlation between simultaneous bets
4. Can suggest dangerously large positions on overconfident estimates
5. Doesn't account for liquidity constraints or slippage

### Recommended Alternative: Confidence Tier System
```
Tier 1 (Low Signal):     0.5% of capital, max 3 concurrent
Tier 2 (Medium Signal):  1.0% of capital, max 5 concurrent
Tier 3 (Strong Signal):  2.0% of capital, max 3 concurrent
Tier 4 (Very Strong):    3.0% of capital, max 2 concurrent (HARD CAP)
Total portfolio exposure: never exceed 15-20% of capital
```

---

## 2. Strategy Suitability Matrix <a name="strategy-matrix"></a>

| Strategy | Agent Suitability | Risk Level | Codifiability | Expected Edge | Competition |
|----------|------------------|------------|---------------|---------------|-------------|
| Resolution Arbitrage | Very High | Low | Full | Medium-High | Medium |
| Dutch Book Arbitrage | Very High | Very Low | Full | Low-Medium | Low |
| Cross-Platform Arbitrage | High | Low | Full | Low (declining) | High |
| Bundle Arbitrage | Medium | Low | High | Low | Low |
| Structured Info Trading | Medium-High | Medium | Partial | Medium | Medium |
| Signal Following | Medium | Medium | High | Low-Medium | High |
| Noise/Sentiment Trading | Very Low | High | Low | Negative/Zero | N/A |
| HFT/Market Making | High (specialized) | Medium | Full | Low per trade | Very High |
| Risk Control/Hedging | High | N/A | Full | N/A (defensive) | N/A |

### Strategy Priority for New Agent Builders
1. Start with Resolution Arbitrage (easiest to implement, clearest edge)
2. Add Dutch Book scanning (pure math, zero directional risk)
3. Layer in Cross-Platform Arbitrage (requires multi-platform infrastructure)
4. Supplement with Structured Information Trading (requires LLM integration)
5. Consider Signal Following as auxiliary (requires filtering mechanisms)

---

## 3. Complete Project Directory <a name="project-directory"></a>

### Infrastructure / Frameworks

**Polymarket Agents Framework**
- GitHub: github.com/Polymarket/agents
- What it does: Official SDK for Polymarket. Data retrieval, order construction, basic LLM interfaces.
- Limitation: Access standard only. Strategy, probability calibration, risk management are DIY.
- Twitter: @Polymarket

**Gnosis Prediction Market Tools**
- GitHub: github.com/gnosis/prediction-market-agent-tooling
- What it does: Full read/write for Gnosis ecosystem (Omen, Manifold). Read-only for Polymarket.
- Limitation: Ecosystem-locked. Limited cross-platform utility.
- Twitter: @gnosis_

### Autonomous Agents

**Olas Predict (Omenstrat / Polystrat)**
- URL: olas.network/agent-economies/predict
- What it does: Most productized PM agent ecosystem. Omenstrat on Omen (Gnosis), Polystrat extends to Polymarket (launched Feb 2026).
- Key features: Natural language strategy definition, auto-identifies probability deviations in markets settling within 4 days, Pearl local execution, self-custodied Safe accounts, hardcoded limits.
- Limitation: LLM-based prediction lacks real-time data integration. Historical win rates vary significantly across categories.
- Twitter: @autnolas

**UnifAI Network**
- URL: chat.unifai.network/strategies/topic/polymarket-banner
- What it does: Automated Polymarket trading agent. Core strategy: tail risk (buys contracts near settlement with >95% implied probability).
- Performance: ~95% win rate, but returns diverge across categories. Highly dependent on execution frequency and category selection.
- Twitter: @UnifaiNetwork

**NOYA.ai**
- URL: noya.ai
- What it does: Attempting full Research→Judgment→Execution closed loop. Intelligence Layer for signal aggregation, Abstraction Layer using Intents for cross-chain.
- Status: Omnichain Vaults delivered. PM Agent still under development. Vision validation stage.
- Twitter: @NetworkNoya

### Market Analysis Tools

**Polyseer** — polyseer.xyz
Multi-Agent architecture (Planner/Researcher/Critic/Analyst/Reporter). Bayesian aggregation for structured research reports. Open-source.

**Oddpool** — oddpool.com
"Bloomberg Terminal for Prediction Markets." Aggregates Polymarket, Kalshi, CME. Includes arbitrage scanning.

**Polymarket Analytics** — polymarketanalytics.com
Global data analysis: trader activity, market positions, volume data.

**Hashdive** — hashdive.com (@hash_dive)
Smart Score system to identify "Smart Money" traders.

**Polyfactual** — polyfactual.com (@polyfactual)
AI market intelligence via Chrome extension. Sentiment and risk analysis.

**Predly** — predly.ai
AI mispricing detection. Compares market prices with AI-calculated probabilities on Polymarket and Kalshi. Claims 89% alert accuracy.

**Polysights** — app.polysights.xyz
Covers 30+ markets. Insider Finder tracks new wallets and large unidirectional bets.

**PolyRadar** — polyradar.io
Multi-model parallel analysis. Real-time interpretation, timeline evolution, confidence scoring.

**Alphascope** — alphascope.app
AI-driven intelligence engine for real-time signals and research summaries (early stage).

### Alerts & Whale Tracking

**Stand** — stand.trade
Whale copy-trading and high-conviction trade alerts.

**Whale Tracker Livid** — whale-tracker-livid.vercel.app
Tracks and productizes whale position changes.

### Arbitrage Discovery

**ArbBets** — getarbitragebets.com (@arbbets)
AI-driven cross-platform arbitrage identification (Polymarket, Kalshi, Sportsbooks).

**PolyScalping** — polyscalping.org (@PolyScalping)
Real-time arbitrage and scalping analysis for Polymarket (1-minute scan cycles).

**Eventarb** — eventarb.com (@eventarbitrage)
Lightweight cross-platform arbitrage calculator (Polymarket, Kalshi, Robinhood).

**Prediction Hunt** — predictionhunt.com
Cross-exchange aggregator comparing prices for arbitrage across Polymarket, Kalshi, PredictIt.

### Trading Terminals & Aggregated Execution

**Verso** (YC Fall 2024)
Institutional-grade terminal. Bloomberg-style interface covering 15,000+ contracts across Polymarket and Kalshi. Includes AI news intelligence.

**Matchr** — matchr.xyz (@matchrxyz)
Cross-platform aggregator. 1,500+ markets, smart routing for optimal price matching. Planned automated yield strategies.

**TradeFox** — thetradefox.com
Professional aggregation and Prime Brokerage. Backed by Alliance DAO and CMT Digital. Advanced order execution (limit, stop-loss, TWAP), self-custody, multi-platform smart routing. Expanding to Kalshi, Limitless, SxBet.

---

## 4. Platform Comparison <a name="platform-comparison"></a>

| Feature | Polymarket | Kalshi |
|---------|-----------|--------|
| Architecture | Hybrid CLOB (off-chain match, on-chain settle) | Traditional exchange |
| Custody | Non-custodial | Custodial |
| Settlement | Decentralized, on-chain | Centralized |
| Regulation | Dual-track (onshore US + offshore) | CFTC-regulated DCM |
| Market Types | Long-tail, crypto, politics, global events | Macro, data, sports, US-focused |
| Agent Support | Official framework (github.com/Polymarket/agents) | API + Python SDK only |
| Liquidity Profile | Deep on popular markets, thin on long-tail | Deep on macro/sports, limited long-tail |
| Access | Global (with US restrictions for some features) | US-focused, broker API integrations |
| Agent Developer Experience | Best — official framework, on-chain data | Basic — raw API, no agent framework |

### Other Platforms Worth Monitoring
- **Omen** (Gnosis) — Decentralized, FPMM-based. Small markets, good for testing.
- **Manifold** — Play money + real money. Lower stakes, easier experimentation.
- **PredictIt** — Legacy platform, declining relevance.
- **ForecastEx** (via Interactive Brokers) — Compliant distribution play, early stage.
- **Limitless** — Crypto-native, points mining model.
- **SxBet** — Sports-focused prediction market.

---

## 5. Regulatory Considerations for Agent Builders <a name="regulatory-landscape"></a>

Regulatory frameworks vary widely by jurisdiction. Key takeaways for building agents:

1. US-based agents have the clearest legal path via Kalshi integration (CFTC-regulated DCM)
2. Crypto-native agents (Polymarket) operate in regulatory gray area for US users
3. Vault/custody models face additional licensing requirements (asset management)
4. Signal/subscription models have lightest regulatory burden (no capital custody)
5. Cross-jurisdictional agents must handle compliance per-market
6. Europe, UK, Australia, Singapore generally classify prediction markets as gambling
7. China and India have complete bans — do not target these markets

---

## 6. Crypto-AI Integration Context

This prediction market agent landscape sits within a broader Crypto-AI convergence:

**Short-term focus: AgentFi** — Automating yield strategies on mature DeFi protocols. Agents as execution layers for DeFi.

**Medium-to-long-term: Agent Payment** — Autonomous stablecoin settlement via emerging standards:
- **ACP** (Agent Communication Protocol) — Agent-to-agent coordination
- **x402** — HTTP-based micropayment protocol for agent commerce
- **ERC-8004** — On-chain standard for agent payment authorization

Prediction market agents sit at the intersection: they need DeFi execution capabilities (for on-chain markets like Polymarket) and payment rails (for cross-platform settlement and fee collection). Building prediction market agents develops infrastructure that transfers to broader AgentFi and Agent Payment use cases.
