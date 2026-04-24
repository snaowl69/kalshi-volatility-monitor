# Kalshi Volatility Monitor

> Real-time volatility tracking for Kalshi markets. Measures price velocity and volume surge across all active markets, ranks them by current volatility score, and alerts when any market enters a high-volatility window.

*Updated: April 2026*

![preview_kalshi volatility monitor tracker](https://github.com/user-attachments/assets/d009918e-98c5-4ccb-bd3c-fab32a75d045)

---

## What is Kalshi Volatility Monitor?

Kalshi Volatility Monitor continuously measures price velocity and volume activity across all active Kalshi markets, builds a real-time volatility ranking, and alerts when any market enters a high-volatility window. High volatility on prediction markets often precedes a significant price move - this tool surfaces those windows before they close.

Monitor everything. Miss nothing.

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github.com/snaowl69/kalshi-volatility-monitor/releases) |

---

## What It Measures

| Metric | Calculation | Window |
|--------|-------------|--------|
| **Price velocity** | % price change per minute | Rolling 15 min |
| **Volume rate** | USDC volume vs. 24h average | Rolling 60 min |
| **Bid-ask spread** | Spread width vs. category baseline | Real-time |
| **Tick frequency** | Number of price updates per window | Rolling 10 min |
| **Composite vol score** | Weighted combination of above | Real-time |

---

## Engine Features

* **Full market sweep** - calculates volatility scores for all active Kalshi markets every cycle
* **Ranked volatility feed** - markets sorted by composite volatility score in real time
* **High-volatility alerts** - Telegram notification when any market score exceeds threshold
* **Category breakdown** - separate volatility rankings per event category
* **Historical vol log** - stores volatility scores per market per cycle for analysis
* **Quiet market alerts** - optional alert when previously volatile markets go quiet
* **CSV export** - full volatility feed exported on each cycle

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **Feed** | Live ranked dashboard | JSON + CSV |
| **Alerts** | Dashboard + Telegram | JSON + Telegram |
| **Config** | `config.toml` | Direct code access |
| **Export** | One-click | Auto CSV |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - set volatility threshold and alert preferences
# 3. Run Kalshi Volatility Monitor - ranked feed starts immediately
```

### Python

```bash
cd kalshi-volatility-monitor/python
pip install -r requirements.txt
python kalshi-volatility-monitor-v.1.0.7.py
```

---

## How It Works

![kalshi volatility scoring pipeline](https://github.com/user-attachments/assets/b703961f-383d-4148-8028-e0985bb9f854)

Three stages per monitoring cycle:

1. **Fetch** - pulls price, volume, and tick data for all active markets
2. **Score** - calculates composite volatility score using weighted metrics
3. **Rank and alert** - sorts markets by score and fires alert if threshold exceeded

### Config Reference

```toml
[monitor]
monitor_interval_sec = 30
high_vol_threshold = 0.70

[weights]
price_velocity = 0.35
volume_rate = 0.30
spread_width = 0.20
tick_frequency = 0.15

[alerts]
alert_on_high_vol = true
alert_on_quiet = false
top_n_in_summary = 5
daily_summary_time = "09:00"

[kalshi]
api_key = ""
api_secret = ""

[telegram]
bot_token = ""
chat_id = ""

[export]
vol_log_csv = "data/volatility/vol_log.csv"
```

---

## Volatility Snapshot Format

```json
{
  "snapshot_id": "vol_20260406_022",
  "timestamp": "2026-04-06T15:00:00Z",
  "top_markets": [
    {
      "market": "cpi-above-3pct-april-2026",
      "vol_score": 0.81,
      "price_velocity_pct_min": 1.6,
      "volume_rate_x": 3.4,
      "spread_width": 0.06
    }
  ]
}
```

---

## Verified Live

![kalshi volatility alert example](https://github.com/user-attachments/assets/f7d9d077-9f00-4705-807b-8f917b563d40)

**Configuration used:**
* High-vol threshold 0.70, monitor every 30s, top 5 in daily summary

**High-volatility alert fired:**

| | Details |
|---|---|
| Market | cpi-above-3pct-april-2026 |
| Vol score | 0.81 |
| Price velocity | 1.6%/min |
| Volume rate | 3.4x 24h avg |
| Spread | 0.06 |
| Alert | Telegram 15:00 |
| Tx hash | 0x4e1f7c9b2d5a8e30d1f4b9c70a2e5b8f1c4d7a03e6b9c2d5f8a1e4b7d0c3f6a9 |

---

## Frequently Asked Questions

**What is Kalshi Volatility Monitor?**
Kalshi Volatility Monitor measures price velocity, volume rate, spread width, and tick frequency across all active Kalshi markets, ranks them by composite volatility score, and alerts when any market enters a high-volatility window.

**Why track volatility on prediction markets?**
High volatility windows on Kalshi often precede significant price moves driven by news or large order activity. Catching these windows early allows entry before the full move prices in.

**Does it execute trades?**
No. Volatility Monitor is a discovery and alert tool. Use it to identify high-activity markets and pair with kalshi-momentum-bot or kalshi-alpha-finder for execution.

**What is a good alert threshold?**
A vol score above 0.65-0.70 typically indicates meaningful activity. Above 0.80 usually means a significant news event or large order is driving the market.

**How are weights configured?**
All four volatility components have configurable weights that sum to 1.0. Adjust them in config to emphasize the signals most relevant to your strategy.

**Does it log historical volatility data?**
Yes. Every monitoring cycle saves the full vol score feed to CSV, building a historical dataset per market for analysis and strategy development.

---

## Use Cases

- **Kalshi volatility tracker** - real-time volatility scoring across all active prediction markets
- **Kalshi high-activity monitor** - detect markets with unusual price velocity and volume
- **Kalshi market activity ranking** - ranked feed of most active markets by composite score
- **Kalshi volatility alerts** - instant Telegram notification when vol score exceeds threshold
- **Prediction market volatility** - systematic volatility measurement on Kalshi events

---

## Repository Structure

```
kalshi-volatility-monitor/
+-- kalshi-volatility-monitor-v.1.0.7.exe
+-- config.toml
+-- data/
|   +-- volatility/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- fetcher.py
|   |   +-- scorer.py
|   |   +-- alerter.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, typer[all], httpx, kalshi-python, pandas
```

* Kalshi API access (read-only)
* Telegram bot token (for alerts)

---

*Alpha starts here.*
