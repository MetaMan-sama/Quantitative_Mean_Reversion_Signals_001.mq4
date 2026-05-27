# 🔗 Custom Correlation Coefficient Alert — `Quantitative_Mean_Reversion_Signals_001.mq4`

> **MQL4 Script for MetaTrader 4**  
> Tracks the Pearson correlation coefficient between two assets in real time and fires alerts whenever the correlation shifts significantly, signaling potential divergence trades, pair relationship breakdowns, or hedging opportunities.

---

## Overview

Asset correlations are not static — they drift and break down as market regimes change. A sudden shift in correlation between two historically linked instruments (e.g., EURUSD and GBPUSD, or Gold and USD) can signal regime change, risk-off flows, or emerging divergence opportunities.

This script continuously computes the **Pearson correlation coefficient** between two configurable assets over a rolling lookback window and alerts the trader whenever the correlation changes by more than a defined threshold in a single polling cycle.

---

## How It Works

The script uses the classic **Pearson correlation formula** applied to closing prices:

```
r = [n·Σ(xy) - Σx·Σy] / √{[n·Σx² - (Σx)²][n·Σy² - (Σy)²]}
```

Where `x` = Asset1 closes and `y` = Asset2 closes over `LookbackPeriod` bars.

**Result ranges:**

| Correlation (r) | Relationship |
|---|---|
| `+0.7` to `+1.0` | Strong positive correlation |
| `+0.3` to `+0.7` | Moderate positive correlation |
| `-0.3` to `+0.3` | Little or no correlation |
| `-0.7` to `-0.3` | Moderate negative correlation |
| `-1.0` to `-0.7` | Strong negative correlation |

**Alert trigger:**

```
|currentCorrelation - previousCorrelation| >= AlertThreshold → Alert fired
```

The script polls every 60 seconds, comparing the current correlation to the previous cycle's value.

---

## Input Parameters

| Parameter | Default | Type | Description |
|---|---|---|---|
| `Asset1` | `"EURUSD"` | string | First instrument in the pair |
| `Asset2` | `"GBPUSD"` | string | Second instrument in the pair |
| `Timeframe` | `PERIOD_H1` | ENUM_TIMEFRAMES | Timeframe for correlation calculation |
| `LookbackPeriod` | `20` | int | Rolling window of bars for correlation |
| `AlertThreshold` | `0.3` | double | Minimum correlation change to trigger alert |
| `EnableAlerts` | `true` | bool | Trigger MT4 sound alerts |
| `EnableEmail` | `false` | bool | Send email notifications |
| `EnablePush` | `false` | bool | Send push notifications to mobile |

---

## Alert Signals

```
Significant Correlation Shift Detected between EURUSD and GBPUSD (Timeframe: PERIOD_H1)
Current Correlation: 0.42, Previous Correlation: 0.78
```

All alerts are logged to the MT4 **Experts journal**.

---

## Trading Applications

### Correlation Breakdown Trading
When two historically correlated pairs suddenly diverge in correlation, the lagging asset often catches up. Monitor EURUSD vs GBPUSD, AUDUSD vs NZDUSD, or XAUUSD vs USDJPY.

### Hedging Validation
Verify that your hedge remains effective. If correlation between your hedge and primary position drops significantly, your hedge may no longer be offsetting risk as intended.

### Risk-Off Detection
Safe haven correlations (Gold/JPY vs risk assets) shift dramatically during market stress. A sharp correlation change between USDJPY and XAUUSD can signal risk-off flows before they're visible in price.

### Pairs Trading
Identify when a historically stable spread between two assets is widening — the correlation drop signals the divergence, creating a potential mean-reversion opportunity.

---

## Common Asset Pairs to Monitor

| Pair | Typical Correlation | Use Case |
|---|---|---|
| EURUSD / GBPUSD | High positive (~0.80) | EUR/GBP divergence |
| AUDUSD / NZDUSD | High positive (~0.90) | Antipodean spread |
| XAUUSD / USDJPY | Negative (~-0.60) | Risk sentiment proxy |
| USOIL / CAD pairs | Moderate positive | Commodity currency hedge |
| EURUSD / USDCHF | Strong negative (~-0.95) | Safe haven vs risk |

---

## Installation

1. Copy `Quantitative_Mean_Reversion_Signals_001.mq4` to:
   ```
   MetaTrader 4/MQL4/Scripts/
   ```
2. Restart MT4 or right-click **Navigator** → **Refresh**
3. Drag the script onto any chart
4. Enter both asset symbols and configure parameters, then click **OK**

> **Important:** Both `Asset1` and `Asset2` must be available in MT4's Market Watch with sufficient history for `LookbackPeriod` bars on the selected `Timeframe`.

---

## Requirements

- MetaTrader 4 (Build 600+)
- Both assets must be active in Market Watch
- Sufficient historical bars loaded for both symbols
- `#property strict` compliance (enforced)
- MT4 must remain running for continuous monitoring

---

## Disclaimer

This script is provided for **educational and informational purposes only**. Correlation-based trading involves assumptions about mean reversion that may not hold in trending or regime-change environments. Always validate signals with additional analysis. Test on a demo account before live use.

---

## License

MIT License — free to use, modify, and distribute with attribution.
