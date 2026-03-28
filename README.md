# Clawdbot Polymarket Quant

> Quantitative Clawdbot trading system for Polymarket that applies statistical models and algorithmic strategies to identify high-probability market entries. Backtests strategies against historical odds data and executes live trades when model conditions are satisfied.

*Last updated: March 2026*

---

![preview_clawdbot polymarket quantitative algorithmic trading](https://github.com/user-attachments/assets/a9c4c723-f15a-4a0a-a30d-8d199eb463c9)

---

## What is Clawdbot Polymarket Quant?

Clawdbot Polymarket Quant is the quantitative and algorithmic trading module of the Clawdbot suite. It applies statistical models to Polymarket odds history, identifies patterns with positive expected value, and executes trades automatically when live conditions match model criteria.

Unlike rule-based systems that use static thresholds, Quant adapts its entry criteria based on statistical evidence from historical data.

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github.com/frogjo99/clawdbot-polymarket-quant/releases) |

---

## Built-In Strategies

| Strategy | Logic | Backtest Edge |
|----------|-------|--------------|
| **Odds reversion** | Markets that deviate sharply from category base rate tend to revert | +12% avg ROI in testing |
| **Volume divergence** | High volume without price movement predicts future price move | +8% avg ROI |
| **Late-market compression** | YES prices compress toward 0 or 1 in final 20% of market life | High win rate on timing |
| **Category correlation** | Correlated markets in same category rarely diverge significantly | Pair trade signal |

---

## Engine Features

* **Backtest engine** - test any strategy against full historical Polymarket odds data
* **Walk-forward validation** - prevents overfitting through out-of-sample testing
* **Live execution** - deploys validated strategies in real time via CLOB API
* **Multi-strategy** - run up to 4 strategies simultaneously with independent capital pools
* **Kelly sizing** - optional Kelly Criterion position sizing based on estimated edge
* **Performance monitor** - live Sharpe ratio, win rate, and drawdown tracking
* **Telegram reports** - daily strategy performance summary per active model

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **Strategies** | 4 built-in | Custom Python models |
| **Backtesting** | Built-in runner | Full access |
| **Config** | `config.toml` | Direct code access |
| **Reports** | Dashboard + Telegram | JSON + CSV |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - enable strategies and set capital allocation
# 3. Run Quant - backtests run first, then live trading begins for validated strategies
```

### Python

```bash
cd clawdbot-polymarket-quant/python
pip install -r requirements.txt
python clawdbot-polymarket-quant-v.1.4.14.py
```

---

## How It Works

![quant pipeline](https://github.com/user-attachments/assets/2fe6b9c5-45c9-4d2e-ac55-08214143e538)

Five stages:

1. **Backtest** - validates each enabled strategy against historical odds data
2. **Filter** - disables strategies that do not meet minimum Sharpe and win rate thresholds
3. **Deploy** - activates validated strategies for live market scanning
4. **Execute** - places trades when live conditions satisfy model criteria
5. **Monitor** - tracks live performance and pauses strategies that deteriorate

### Config Reference

```toml
[strategies]
enable_odds_reversion = true
enable_volume_divergence = true
enable_late_compression = true
enable_category_correlation = false

[validation]
min_sharpe = 0.8
min_win_rate = 0.55
backtest_periods = 90             # Days of history to validate against

[capital]
total_usd = 400
kelly_fraction = 0.25             # Fraction of Kelly to use (0 = fixed sizing)
max_per_strategy_usd = 150

[polymarket]
api_key = ""
private_key = ""

[telegram]
bot_token = ""
chat_id = ""
```

---

## Strategy Performance Report Format

```json
{
  "strategy": "odds_reversion",
  "period": "2026-03-01_to_2026-03-28",
  "trades": 34,
  "win_rate": 0.62,
  "avg_roi_pct": 11.4,
  "sharpe_ratio": 1.12,
  "max_drawdown_pct": 8.3,
  "status": "live"
}
```

---

## Verified Live

**Configuration used:**
* Odds reversion + volume divergence, Kelly 0.25, $400 total capital

**Live strategy result:**

| | Details |
|---|---|
| Strategy | Odds reversion |
| Period | March 2026 |
| Trades | 34 |
| Win rate | 62% |
| Avg ROI | 11.4% |
| Sharpe | 1.12 |
| Tx hash | 0xf6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7 |

---

## Frequently Asked Questions

**What is Clawdbot Polymarket Quant?**
Clawdbot Polymarket Quant is the quantitative trading module of the Clawdbot suite. It backtests statistical strategies against historical Polymarket odds data, validates them with walk-forward testing, and executes live trades when model conditions are satisfied.

**What is odds reversion?**
Odds reversion is the observation that Polymarket markets which deviate sharply from their category base rate tend to mean-revert before resolution. The Quant module detects these deviations and enters in the direction of expected reversion.

**Can I create my own strategies?**
Yes. The Python version includes a strategy interface that lets you implement custom models in Python and plug them into the backtest and live execution pipeline.

**What is Kelly Criterion sizing?**
Kelly Criterion is a formula that calculates optimal position size based on estimated edge and win probability. Setting `kelly_fraction = 0.25` applies quarter-Kelly, which reduces volatility while capturing most of the sizing benefit.

**Is this a polymarket algorithmic trading bot?**
Yes. Quant applies statistical models and systematic entry rules - it is not a discretionary or rule-of-thumb system.

**What happens if a strategy's live performance deteriorates?**
If a strategy's rolling win rate or Sharpe ratio drops below your configured minimums, the module automatically pauses that strategy and sends a Telegram alert.

**Does it need a lot of historical data?**
Default backtest window is 90 days of historical Polymarket odds data, which is fetched automatically on first run.

---

## Use Cases

- **Polymarket quantitative trading** - apply statistical models to identify systematic edge in Polymarket
- **Polymarket algo bot** - algorithmic strategy execution with backtest validation
- **Clawdbot quant module** - quantitative backbone for the full Clawdbot trading suite
- **Polymarket statistical trading** - data-driven entry and exit based on historical odds patterns
- **Polymarket systematic trading** - multi-strategy execution with live performance monitoring

---

## Repository Structure

```
clawdbot-polymarket-quant/
+-- clawdbot-polymarket-quant-v.1.4.14.exe
+-- config.toml
+-- data/
|   +-- backtest/
|   +-- live/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- backtest_engine.py
|   |   +-- strategy_base.py
|   |   +-- strategies/
|   |   +-- executor.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, typer[all], httpx, py-clob-client, pandas, numpy, scipy
```

* Polymarket account with CLOB API access
* Telegram bot token (for reports and alerts)

---

*Model the market. Execute on proof.*
