---
name: prediction-market-agents
description: >
  Comprehensive knowledge base and strategic framework for AI agents operating in prediction markets
  (Polymarket, Kalshi, and others). Use this skill whenever the user asks about prediction markets,
  prediction market agents, event contract trading, automated betting strategies, probability markets,
  AgentFi, crypto-AI agents, arbitrage on prediction platforms, or building/deploying agents that trade
  on outcome markets. Also trigger when discussing Polymarket, Kalshi, Omen, resolution arbitrage,
  Dutch Book strategies, cross-platform arbitrage, Kelly Criterion for betting, or business models for
  autonomous trading agents. This skill covers market landscape, agent architecture, strategy selection,
  position management, risk control, business models, tooling ecosystem, and monetization opportunities.
  Even if the user just mentions "prediction markets" casually, consult this skill — it contains
  strategic depth that generic knowledge lacks.
---

# Prediction Market Agents: Strategy & Architecture Guide

This skill provides a complete framework for understanding, building, and monetizing AI agents that operate in prediction markets. It covers the market landscape, technical architecture, strategy selection, risk management, business models, and the current tooling ecosystem.

For deep reference on specific topics (project directory, strategy math, tooling details), see `references/deep-reference.md`.

---

## 1. Prediction Market Landscape (2025–2026)

Prediction markets are financial mechanisms for trading on future event outcomes. Contract prices reflect collective probability judgments. Their effectiveness comes from combining crowd wisdom with economic incentives — real-money bets aggregate dispersed information into price signals, reducing noise.

### Key Platforms

**Polymarket** — Crypto-native, global
- Hybrid CLOB architecture: off-chain matching, on-chain settlement
- Non-custodial, decentralized settlement
- Strongest for: long-tail events, crypto-native users, global access

**Kalshi** — US-regulated, traditional finance
- Integrated with traditional financial system (broker APIs, Wall Street market makers)
- Strongest for: macro/data contracts, institutional participation
- Weakness: regulatory lag on long-tail and sudden events

### The "Global Truth Layer" Thesis
Prediction markets are evolving from betting tools into real-time probability infrastructure. As institutions like CME and Bloomberg connect, event probabilities become decision-making metadata callable by financial and corporate systems.

---

## 2. Agent Architecture (Four-Layer Model)

The value of prediction market agents is NOT "AI predicts more accurately." It is amplifying information processing and execution efficiency. Real-world market inefficiencies stem from information asymmetry, liquidity gaps, and attention constraints. Agents capture value through speed, discipline, and systematic execution.

**Core positioning: Executable Probabilistic Portfolio Management** — converting news, rule texts, and on-chain data into verifiable pricing deviations, executing faster and cheaper than humans.

### Layer 1: Information Layer
Aggregates and normalizes data from multiple sources:
- News feeds and social media (Twitter/X, Reddit, Telegram)
- On-chain data (wallet flows, smart money movements, liquidity shifts)
- Official data sources (government releases, earnings reports, sports APIs)
- Market data (prices, volumes, order books across platforms)

### Layer 2: Analysis Layer
Processes raw data into actionable signals:
- LLM-based event interpretation and sentiment analysis
- ML models for pattern recognition and probability estimation
- Mispricing identification: comparing model probability vs. market implied probability
- Edge calculation: `Edge = Model_Probability - Market_Price`

### Layer 3: Strategy Layer
Converts Edge into executable positions:
- Position sizing via confidence tiers (see Section 4)
- Entry timing: staggered entry to manage slippage
- Risk control: max exposure limits, correlation constraints, drawdown triggers
- Portfolio construction across multiple markets

### Layer 4: Execution Layer
Handles the mechanical trading:
- Multi-market order placement and routing
- Slippage optimization and Gas management
- Cross-platform arbitrage execution
- Settlement monitoring and position reconciliation

An effective agent closes the loop across all four layers autonomously.

---

## 3. Strategy Framework

Not all prediction markets or strategies suit automated execution. The core question: does the scenario have clear rules, codifiability, and structural advantages for an agent?

### Target Selection Criteria
Before entering any market, evaluate five dimensions:
1. **Settlement Clarity** — Are resolution rules unambiguous? Is the data source unique and verifiable?
2. **Liquidity Quality** — Sufficient market depth, tight spreads, adequate volume?
3. **Insider Risk** — How asymmetric is information access? High insider risk = avoid.
4. **Time Structure** — Expiration timeline and event pacing. Shorter = more agent-friendly.
5. **Information Advantage** — Does the agent have a structural edge in processing relevant data?

### Human vs. Agent Advantage Zones

**Human Core Advantage (days/weeks decision windows):**
- Political elections, macro trend forecasting, corporate milestones
- Markets requiring domain expertise, judgment on ambiguous information, qualitative integration

**Agent Core Advantage (seconds/minutes decision windows):**
- High-frequency crypto price markets
- Cross-market arbitrage
- Automated market making
- Resolution arbitrage (event decided but price hasn't caught up)

**Avoid Entirely:**
- Markets dominated by insider information
- Purely random or heavily manipulated markets
- No participant has an edge — negative expected value

### Strategy Categories

#### A. Deterministic Arbitrage (Core revenue — highest agent suitability)

These are the bread and butter. Clear rules, codifiable, minimal subjective judgment.

1. **Resolution Arbitrage** — Event outcome is effectively decided but market hasn't fully priced it. Agent monitors news, detects resolution, executes before price corrects. Low risk, fully codifiable, fastest path to consistent returns.

2. **Dutch Book Arbitrage (Probability Conservation)** — When prices across all outcomes of a mutually exclusive event don't sum to 1.0, build a portfolio that guarantees profit regardless of outcome. Pure math, zero directional risk.

3. **Cross-Platform Arbitrage** — Same event priced differently on Polymarket vs. Kalshi vs. others. Agent scans both simultaneously, captures spread. Low risk but requires low latency and parallel monitoring. Competition intensifying, margins shrinking.

4. **Bundle Arbitrage** — Pricing inconsistencies between related contracts. Clear logic but limited opportunities. Medium agent suitability.

#### B. Speculative Directional (Supplementary only)

5. **Structured Information Trading** — Trading around scheduled data releases, announcements, ruling windows. Agent advantage in speed and discipline when trigger conditions are definable. Human intervention still needed for semantic judgment.

6. **Signal Following** — Copying high-performing wallets or accounts. Simple to automate but core risk is signal decay and being front-run. Use as auxiliary strategy with strict filtering.

7. **Unstructured/Noise-Driven** — Sentiment-based, random, or crowd-behavior driven. No stable edge, not suitable for systematic agent execution. Exclude from agent strategies.

#### C. Infrastructure Strategies (Specialized)

8. **High-Frequency/Market Making** — Continuous quoting, liquidity provision. Extreme latency and capital requirements. Only viable for teams with significant infrastructure advantages.

9. **Risk Control & Hedging** — Not profit-seeking; reduces overall portfolio risk exposure. Runs as a background module.

**Summary rule:** Deterministic arbitrage = core. Structured information + signal following = supplement. Everything else = exclude or specialized.

---

## 4. Position Management

The goal is not maximizing single-trade returns but maximizing long-term compound growth rate.

### Kelly Criterion (Theoretical Foundation)

Classic formula: `f* = (bp - q) / b`
- f* = optimal bet fraction, b = net odds, p = win rate, q = 1-p

Simplified for prediction markets: `f* = (p - market_price) / (1 - market_price)`
- p = your estimated true probability
- market_price = market implied probability

The Kelly formula is theoretically optimal but highly dependent on accurate probability estimation — which is hard in practice.

### Practical Position Management Methods

**For agents, prioritize executability and stability over theoretical optimality.**

1. **Confidence Tiers with Fixed Caps (RECOMMENDED for agents)**
   - Divide opportunities into 3-4 tiers based on signal strength
   - Preset position sizes per tier with absolute caps
   - Example: Low confidence = 0.5% of capital, Medium = 1%, High = 2%, Maximum = 3% (hard cap)
   - Does not require precise probability estimation
   - Controls risk even in high-conviction scenarios

2. **Unit System** — Split capital into fixed units (e.g., 1%), invest different unit counts by confidence. Common in professional gambling.

3. **Flat Betting** — Fixed percentage per bet regardless of conviction. Maximum discipline, suitable for low-conviction environments.

4. **Inverted Risk Approach** — Start from maximum tolerable loss, calculate position backwards. Risk-first rather than profit-first.

**Key principle:** Simple parameters, clear rules, tolerance for judgment errors. The Confidence Tiers method is the most suitable general scheme for prediction market agents.

---

## 5. Business Models & Monetization

### Revenue Layers

**Infrastructure Layer (B2B — most stable)**
- Multi-source real-time data aggregation APIs
- Smart Money address libraries and whale tracking
- Unified prediction market execution engines
- Backtesting and simulation tools
- Revenue: subscription fees, API calls, SaaS pricing
- Why it works: revenue is independent of prediction accuracy

**Strategy Ecosystem Layer (Platform — scalable)**
- Third-party strategy marketplace
- Community-contributed strategies with evaluation/ranking
- Revenue: strategy calls, execution profit-sharing, licensing fees
- Why it works: reduces dependence on single Alpha; builds network effects

**Agent/Vault Layer (Asset management — highest ceiling)**
- Agents trade with pooled capital via entrusted management
- On-chain transparent records + strict risk control
- Revenue: management fees (% of AUM) + performance fees (% of profits)
- Why it works: scale effects and execution efficiency
- Risk: requires licenses, track record, institutional trust. Not a starting point.

### Product Forms

**Entertainment/Gamification Mode**
- Tinder-like swipe UX for market participation
- Strongest user acquisition and market education capability
- Ideal top-of-funnel for breaking out of niche
- Must funnel users to subscription/execution products for monetization

**Strategy Subscription/Signal Mode (MOST FEASIBLE NOW)**
- No capital custody = lighter regulatory burden
- Clear rights and responsibilities
- SaaS revenue structure
- Limitation: strategies are copyable, execution suffers slippage
- Enhancement: "Signal + One-Click Execution" semi-automated form improves retention

**Vault Custody Mode (ENDGAME)**
- Resembles asset management products with scale effects
- Structural constraints: asset management licenses, trust thresholds, centralized tech risks
- Highly dependent on market environment and sustained profitability
- Only viable with long-term track record + institutional endorsement

### Sustainable Model
The optimal approach combines all three: "Infrastructure Monetization + Strategy Ecosystem + Performance Participation." Even if Alpha converges as markets mature, underlying capabilities (execution, risk control, settlement) retain long-term value.

---

## 6. Current Ecosystem & Tooling

The ecosystem is early-stage. No mature, standardized product has closed the full loop on strategy generation → execution → risk control.

### Development Frameworks
- **Polymarket Agents Framework** — Official SDK. Handles data retrieval, order construction, basic LLM interfaces. Access standard only — strategy, calibration, and risk management left to developer.
- **Gnosis Prediction Market Tools** — Full read/write for Gnosis ecosystem (Omen/Manifold). Read-only for Polymarket. Strong for Gnosis-native agents, limited cross-platform.
- **Kalshi** — API and Python SDK only. No official agent framework. Developers must build strategy, risk control, monitoring from scratch.

### Autonomous Agents (Early Stage)
- **Olas Predict / Polystrat** — Most productized. Natural language strategy definition, auto-identifies probability deviations in markets settling within 4 days. Self-custodied Safe accounts, hardcoded limits. First consumer-grade autonomous trading agent for Polymarket.
- **UnifAI Network** — Tail risk strategy: buys contracts near settlement with >95% implied probability. ~95% win rate but returns vary by category.
- **NOYA.ai** — Attempting full Research→Judgment→Execution loop. Omnichain Vaults delivered but PM agent still under development.

### Analysis & Signal Tools
- **Oddpool** — "Bloomberg Terminal for Prediction Markets." Aggregates Polymarket, Kalshi, CME.
- **Hashdive** — Smart Score for identifying Smart Money.
- **Predly** — AI mispricing detection, claims 89% alert accuracy.
- **ArbBets** — Cross-platform arbitrage identification.
- **TradeFox** — Professional aggregation + Prime Brokerage. Advanced order types, multi-platform routing.
- **Verso** — Institutional terminal (YC Fall 2024), 15,000+ contracts, AI news intelligence.

For the full project directory with links, see `references/deep-reference.md`.

---

## 7. Key Opportunities & Strategic Gaps

When advising on prediction market agent opportunities, highlight these structural gaps:

1. **No one has closed the full loop** — Strategy generation → execution → risk control in one productized system doesn't exist yet. First mover advantage is available.

2. **Cross-platform arbitrage infrastructure is underdeveloped** — Unified execution across Polymarket + Kalshi + others with smart routing is a clear gap.

3. **Strategy marketplace/ecosystem doesn't exist** — A platform where third-party developers build, evaluate, and monetize strategies (taking a cut on execution) has strong network effects potential.

4. **Entertainment/gamification entry point is wide open** — Character-driven AI agents that make prediction market participation feel like a game rather than a Bloomberg terminal. Strongest user acquisition funnel, especially for gaming-native audiences.

5. **Agent launchpad model** — A platform enabling creators/developers to deploy prediction market agents with built-in monetization maps directly to platform-as-a-service business models.

6. **Regulatory arbitrage** — Building infrastructure that works across both crypto-native (Polymarket) and regulated (Kalshi) rails gives structural advantage as the regulatory landscape evolves.

---

## Decision Framework: When a User Asks About Prediction Market Agents

1. **"How do prediction markets work?"** → Start with Section 1 (landscape) and the Global Truth Layer thesis
2. **"How would I build a prediction market agent?"** → Walk through the four-layer architecture (Section 2), then strategy selection (Section 3)
3. **"What strategies should an agent use?"** → Deterministic arbitrage first, structured information as supplement. Reference the target selection criteria.
4. **"How do I size positions?"** → Confidence Tiers with Fixed Caps. Explain why Kelly is theoretically nice but practically fragile.
5. **"How do I make money with this?"** → Business models (Section 5). Infrastructure B2B is most stable. Signal subscription is most feasible now. Vault is endgame.
6. **"What tools/frameworks exist?"** → Section 6 ecosystem overview. Emphasize everything is early-stage.
7. **"Where are the opportunities?"** → Section 7 strategic gaps. Tailor to the user's specific strengths and context.
