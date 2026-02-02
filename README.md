# Volume-Spread-Analysis-VSA-Scanner
A real-time Binance scanner that detects anomalies between trading volume and price movement (Volume Spread Analysis principles). It identifies moments where high volume fails to move the price significantly, suggesting potential market exhaustion or absorption.

---

# 📊 Binance Volumetric Anomaly Scanner

A real-time market monitoring tool designed to detect **Volumetric Divergence** and **Hidden Absorption** on Binance USDT-futures and spot markets. This scanner identifies high-effort/low-result price action, which often precedes significant market reversals or exhaustion.

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Binance](https://img.shields.io/badge/Binance-F3BA2F?style=for-the-badge&logo=binance&logoColor=black)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

## 🎯 Core Logic: Efficiency Divergence
The scanner is based on the principles of **VSA (Volume Spread Analysis)**. It monitors the relationship between trading volume (Effort) and price movement (Result).

**The Anomaly:** When trading volume surges significantly (e.g., >1.5x average) but the price remains relatively stagnant, it indicates:
*   **Absorption:** A large player is absorbing all market orders using limit orders.
*   **Exhaustion:** The current trend has lost its "efficiency" and is likely to stall or reverse.

*   The general situation of divergence between volume and movement indicates that momentum is to be expected.

## 🛠 Key Features

### 1. Statistical Anomaly Detection
Instead of using fixed values, the scanner calculates a **historical baseline** for each coin:
*   **Robust Statistics**: Uses **Median** instead of Mean to define "normal" behavior, making the system resistant to extreme price outliers.
*   **Efficiency Coefficient ($K$):** Calculated as `Volatility / Normalized Volume`. The scanner triggers a signal when the current $K$ falls below a defined percentage (e.g., 40%) of the historical median.

### 2. Multi-Metric Volatility Engine
The tool supports 4 different ways to measure price action intensity:
*   **True Range**: Classic High-Low spread.
*   **Body**: Focuses strictly on the Open-Close distance.
*   **Log Range**: Mathematically robust metric for assets with high price variance.
*   **Price Impact**: Normalized movement relative to volume.

### 3. Real-time Forward Backtesting
The scanner includes a built-in performance tracker (`check_outcomes`):
*   After a signal is triggered, the bot monitors the price for the next **N candles**.
*   It automatically prints the ROI (Return on Investment) for each signal, providing immediate feedback on the strategy's accuracy.

### 4. Candle Shadow Analysis
Automatically detects **Bullish/Bearish Pressure** by analyzing wicks (shadows):
*   **Bullish Pressure**: Long lower wicks indicating strong rejection of lower prices.
*   **Bearish Pressure**: Long upper wicks indicating heavy selling at higher levels.

---

## ⚙️ Configuration (Hyperparameters)

| Parameter | Default | Description |
| :--- | :--- | :--- |
| `EFFICIENCY_THRESHOLD` | 0.4 | Triggers signal if efficiency is < 40% of normal. |
| `MIN_VOLUME_SPIKE` | 1.5 | Minimum volume surge required (1.5x vs average). |
| `REFERENCE_WINDOW` | 200 | Lookback period for calculating statistical baseline. |
| `METRIC_TYPE` | `true_range` | The math model for price movement calculation. |
More parameters and operating logic are available in the "Parameter Guide & Trading Logic.md" file.
---

## 🚀 Installation & Usage

1. **Clone the repository:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/binance-volumetric-scanner.git
   ```
2. **Install dependencies:**
   ```bash
   pip install python-binance pandas
   ```
   Enter your Binance API Key and Secret in the `CONFIGURATION` section of the script. (Note: Only 'Read' permissions are required for scanning).
3. **Run the scanner:**
   ```bash
   python scanner.py
   ```

(4. **Setup API Keys:** This is not necessary!)
---

## 📈 Example Output
```text
⏳ Scanning Cycle #42 [14:30:05]... 
🔥 SIGNAL: SOLUSDT 🔥
Price: 145.200000 (+0.15%)
⚡ VOLUMETRIC DIVERGENCE (true_range)!
Volume surge: 2.45x vs avg
Efficiency: 32.15% of median norm
Movement: 0.120% (Expected: 0.450%)
☄️ Bearish Pressure (Upper Wick)

📊 Outcome for SOLUSDT: 1cndl: -0.25% | 2cndl: -0.85% | 3cndl: -1.40%
```

---
**Disclaimer**: *This software is for educational and analytical purposes only. Trading cryptocurrencies involves significant risk of loss.*
