# Quantalytics: Regime-Adaptive Volatility-Aware Momentum Strategy

A research-grade quantitative system designed for Gold (**XAUUSD**) and Silver (**XAGUSD**). This project moves beyond retail-grade "M1 noise" to focus on high-fidelity institutional timeframes (1H) and regime-adaptive logic.

###  Research Question
> **"Does adapting momentum strategies to market volatility regimes significantly improve risk-adjusted returns in precious metals compared to static momentum strategies?"**



##  Quick Start

### 1. Setup Environment
```bash
python -m venv venv
# Activate venv (Windows)
.\venv\Scripts\Activate.ps1
# Install dependencies
pip install -r requirements.txt
```

### 2. Research-Grade Preprocessing (M1 -> 1H)
We strictly avoid M1 data to eliminate microstructure noise and artificial backtest inflation.
```bash
python data/processed/preprocess.py
```

### 3. Exploratory Analysis
Run the notebooks to validate regime detection logic.
```bash
# Check notebooks/exploration.ipynb for regime visualization
```

---

##  Strategy Results & Benchmarking
### 1. Final Comparison (2024 Full Year)
| Asset | Strategy | Sharpe | Max Drawdown | Total Return | Verdict |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **XAUUSD** | Baseline | **2.12** | -5.09% | **24.8%** | Aggressive |
| **XAUUSD** | Adaptive | 1.70 | **-3.69%** | 12.98% | **Stable** |
| **XAGUSD** | Baseline | 0.51 | -16.43% | 11.16% | Noisy |
| **XAGUSD** | Adaptive | -0.08 | **-10.34%** | -1.18% | **Protected** |

**Observation**: The Adaptive Strategy prioritizes capital preservation. While it sacrifices some upside in Gold's massive 2024 trend (where a static fast lookback was ideal), it reduced Max Drawdown by **~28% (Gold)** and **~37% (Silver)**.

### 2. Walk-Forward Validation (Quarterly)
| Period | Base Sharpe | Adapt Sharpe | Base DD | Adapt DD |
| :--- | :--- | :--- | :--- | :--- |
| **Q2 2024** | 0.50 | -0.04 | -5.09% | **-2.96%** |
| **Q3 2024** | 3.34 | 2.92 | -2.66% | -2.86% |
| **Q4 2024** | 2.10 | 1.89 | -3.38% | **-2.58%** |

---

## Walk-Forward Validation Methodology
We employ a rolling walk-forward evaluation:
- Train window: Rolling snapshots (3 months min)
- Test window: Disjoint quarter steps
- Step size: 3 months

At each step, regime thresholds and strategy parameters remain fixed to ensure no look-ahead bias or over-optimization.

---

##  Data Methodology
- **Raw Data**: 1-minute (M1) institutional feeds.
- **Research Timeframe**: 1-hour (1H) bars. This removes "noise" and provides a statistically stable foundation for regime detection.
- **Validation**: Strict Time-Based Split:
  - **In-Sample (Train)**: First 70% (2024-01-01 to 2024-09-12).
  - **Out-of-Sample (Test)**: Last 30% (Unseen verification).

---

##  Project Structure
```text
quantalytics/
├── data/
│   ├── raw/           # Raw M1 CSV files
│   └── processed/     # 1H Resampled Research Data
├── features/          # Feature engineering modules (Volatility, Regimes)
├── strategies/        # Baseline & Regime-Adaptive strategies
├── backtests/         # Walk-forward analysis scripts
├── evaluation/        # Statistical significance tests
├── notebooks/         # EDA & visual analysis
├── strategy.py        # Placeholder (Development Phase)
└── README.md
```

---
**Author**: Amarendra Pratap Singh
**License**: MIT
