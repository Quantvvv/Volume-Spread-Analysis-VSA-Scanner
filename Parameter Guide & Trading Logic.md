## 🧠 Logic Behind the Hyperparameters

### The Signal Core (Divergence Logic)
* **`EFFICIENCY_THRESHOLD` (Default: 0.4)**: This is the sensitivity of the anomaly detector. If a coin's price-per-volume efficiency drops below 40% of its normal state, we suspect a "Wall" (large limit orders) is blocking the move. 
    * *Higher values (0.6):* More signals, but more noise.
    * *Lower values (0.2):* Only extremely heavy absorption events.
* **`MIN_VOLUME_SPIKE` (Default: 1.5)**: A filter to ensure we only look at "smart money" participation. We ignore random price wiggles and focus only on moves where volume is at least 150% of the recent average.

### Mathematical Foundations
* **`METRIC_TYPE`**: 
    * `true_range`: Best for general volatility.
    * `log_range`: Used for high-priced assets where percentage moves are more relevant than absolute price points.
    * `body`: Filters out "fake" volatility from long wicks, focusing only on where the market actually closed.
* **`USE_MEDIAN` (True)**: We use the **Median** instead of the Mean for the baseline. In crypto, a single "pump" can skew the average volume for days. The Median is statistically "robust" and keeps our baseline stable.

### Risk & Execution
* **`FORWARD_CANDLES` ([1, 2, 3...])**: This is our internal "Reality Check." The bot tracks price action after the signal for continuous optimization.
* **`COOLDOWN_MINUTES` (30)**: Prevents repetitive alerts for the same coin, reducing "Alert Fatigue".

### ⚙️ Other System Parameters
| Parameter | Value | Description |
| :--- | :--- | :--- |
| **TIMEFRAME** | 15m | Balancing noise reduction and signal frequency. |
| **REFERENCE_WINDOW** | 200 | Statistical sample size for a stable baseline. |
| **MIN_VOL_USDT** | 50,000 | **Liquidity Floor**. Ensures anomalies are backed by real capital. |
| **SLEEP_INTERVAL** | 60s | Cycle delay to avoid API rate-limiting. |

---

## 🌐 Asset Selection & Filtering
The scanner doesn't use a hardcoded list. It builds the target list dynamically to ensure maximum market coverage:

* **Dynamic Discovery**: Queries Binance API (`get_exchange_info`) to fetch all currently tradable assets.
* **Garbage Filtering**:
    * **USDT Pairs Only**: Focuses on the highest liquidity quotes.
    * **Trading Status**: Automatically removes delisted or suspended pairs.
    * **Leveraged Tokens Exclusion**: Filters out "UP"/"DOWN" tokens to avoid artificial volatility.
* **Liquidity Filter**: Uses `MIN_VOL_USDT` to skip "ghost" coins with empty order books, preventing false signals on low-cap pumps.
