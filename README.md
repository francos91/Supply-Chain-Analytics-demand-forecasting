
---

# Project 01: Demand Forecasting & (s, S) Inventory Optimization

**Author:** F. Saayman  
**Date:** June 2026  
 MEng Industrial Engineering – Logistics & Supply Chain Systems  
**Methodology:** CRISP-DM | Monte Carlo Simulation | Pareto Frontier

---

## 📄 Abstract

This project presents a comprehensive demand forecasting and inventory optimization pipeline for retail supply chains, applied to Walmart store–department SKU data. Following the CRISP-DM methodology, the pipeline integrates exploratory data analysis, probabilistic forecasting, and stochastic simulation to derive operationally feasible (s, S) inventory policies under lead‑time uncertainty.

Exploratory analysis reveals that 75% of SKUs exhibit a coefficient of variation below 0.60, validating the use of a continuous‑review policy for core items, while a 16% sales lift during holiday weeks confirms the necessity of dynamic forecast adjustments. Augmented Dickey‑Fuller testing confirms non‑stationarity (p = 0.11), motivating the use of Prophet over classical ARIMA models. Forecasting performance is benchmarked between Prophet and XGBoost; Prophet achieves the lowest mean absolute percentage error (12.4%) due to its explicit seasonality decomposition.

Forecasted weekly demand over a 4‑week horizon serves as input to a Monte Carlo simulation that evaluates 64 (s, S) policy combinations under stochastic lead times (mean = 1.0 week, sigma = 0.2 weeks). A Pareto frontier identifies the optimal knee‑point policy: a reorder point of 22,924 units and an order‑up‑to level of 40,317 units, achieving a 96.8% service level at an expected total cost of $115,547. This policy surpasses the theoretical Economic Order Quantity baseline by explicitly accounting for demand variability and supply uncertainty.

The complete pipeline is implemented in Python, with all visualizations and summary metrics exported to an `outputs/` directory, offering supply chain managers a data‑driven decision‑support tool for balancing cost efficiency against service reliability.

---

## 📊 Key Results (Sample SKU: `1_1`)

| Metric | Value |
| :--- | :--- |
| **Best Forecasting Model** | Prophet (MAPE: **12.4%**) |
| **Theoretical EOQ** | 7,675 units |
| **Selected Reorder Point (s)** | **22,924** units |
| **Selected Order‑Up‑To (S)** | **40,317** units |
| **Achieved Service Level** | **96.8%** |
| **Expected Total Cost** | **$115,547** |

---

## 🧰 Tech Stack

- **Language:** Python 3.10+
- **Libraries:** Pandas, NumPy, Matplotlib, Seaborn, Scikit‑learn, Prophet, Statsmodels, Plotly, XGBoost, IPyWidgets.
- **Data Source:** Kaggle – Walmart Recruiting Store Sales Forecasting.
- **Environment:** Google Colab (interactive) / Local Terminal (batch).

---

## 📂 Repository Structure

```
project01-demand-forecasting/
├── outputs/                              # Generated outputs (static plots & metrics)
│   ├── pareto_frontier.png               # Final Pareto frontier
│   ├── forecast_plot.png                 # 4‑week forecast bar chart
│   ├── Demand Volatility Profile Across SKUs.png
│   ├── Demand distribution.png
│   ├── AUTOCORRELATION & STATIONARITY.png
│   ├── Holiday Impact – SKU 1_1.png
│   ├── Pareto Frontier Cost vs Service Level.png
│   ├── Dashboard.png                     # Screenshot of interactive dashboard
│   ├── eda_summary.json
│   ├── summary_metrics.csv
│   └── sku_holiday_multiplier.csv
├── Proj01_Demand_Forecasting (2) (1).ipynb   # Colab notebook (with upload prompt)
├── proj01_demand.py                      # Standalone terminal version
├── train.csv                             # Raw data (also placed in ./data/raw/ for notebook)
├── features.csv
├── stores.csv
├── test.csv                              # If present, may be used for validation
├── master_data.parquet                   # Cached merged data (generated)
├── requirements.txt                      # Python dependencies (if included)
├── .gitignore
├── LICENSE
└── README.md
```

> **Note:** For the notebook to run automatically without upload prompts, place the three CSV files (`train.csv`, `features.csv`, `stores.csv`) inside a `data/raw/` folder. The notebook will also look for a cached `master_data.parquet` to speed up loading.

---

## ⚠️ Interactive Dashboard on GitHub

The notebook includes an **interactive dashboard** built with `ipywidgets` that lets you adjust the target service level and toggle holiday effects. However, GitHub’s notebook viewer **does not render interactive widgets** – it strips the custom metadata, so you will only see a static placeholder.

To interact with the dashboard, please **open the notebook in Google Colab** (or run it locally).  
For a quick look at the final policy and forecast, check the static images in the `outputs/` folder, especially:

- `pareto_frontier.png` – the optimal policy on the cost‑service frontier.
- `forecast_plot.png` – the 4‑week demand forecast.

---

## 🚀 How to Run

### Option 1: Google Colab (Interactive)
1. Open the notebook in [Google Colab](https://colab.research.google.com/).
2. Click `Runtime` → `Run all`.
3. If the CSV files are not found locally, Cell 4 will prompt you to upload them.
4. After execution, the interactive dashboard will appear at the bottom – use the slider and checkbox to explore trade‑offs.

### Option 2: Local Terminal (Standalone Script)
1. Clone this repository and navigate to the project folder.
2. Ensure the three CSV files (`train.csv`, `features.csv`, `stores.csv`) are placed in the **same directory** as the script (or adjust the paths inside `proj01_demand.py`).
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
   (If `requirements.txt` is not present, install the libraries manually: `pandas numpy matplotlib seaborn scikit-learn prophet statsmodels plotly ipywidgets xgboost`.)
4. Run the script:
   ```bash
   python proj01_demand.py
   ```
5. The script will generate all outputs (plots, CSV summaries, JSON) in the `outputs/` folder.

---

## 🔬 Methodology (CRISP‑DM)

1. **Business Understanding** – Define holding/ordering costs, shortage penalties, and target service level (95%).
2. **Data Understanding** – EDA reveals 0.3% negative sales (returns), missing markdowns (filled with 0), and a 16% sales lift during holiday weeks.
3. **Data Preparation** – Created lag features (1–4 weeks), rolling means, and holiday flags for the target SKU.
4. **Modeling (Forecasting)** – Trained Prophet and XGBoost. Prophet achieved the lowest MAPE (12.4%), handling the non‑stationary demand patterns.
5. **Modeling (Optimization)** – Grid search over 64 (s, S) pairs using a Monte Carlo simulator (stochastic lead times). Pareto frontier identifies the efficient frontier.
6. **Evaluation** – Selected knee‑point policy achieving 96.8% service level at minimum cost.
7. **Deployment** – Interactive dashboard allows dynamic trade‑off analysis; standalone script provides batch processing.

---

## 📈 Key EDA Insights

- **Demand Volatility:** 75% of SKUs have a Coefficient of Variation (CV) < 0.60, validating the (s, S) approach for stable core items.
- **Non‑Stationarity:** ADF test confirmed non‑stationarity (p = 0.11), justifying the use of Prophet over classical ARIMA.
- **Holiday Effect:** The sample SKU shows a consistent +16% sales lift, which is applied dynamically in the forecast.
- **Right‑Skewed Demand:** The distribution for SKU 1_1 shows a long tail to the right (mean > median), confirming sporadic high‑demand spikes.

---

## 📚 Academic Reference

You, D.-G., Wang, C.-H., & Li, Y.-T. (2026). Data‑Driven Simulation‑Optimization for Sustainable (s, S) Inventory Policy Design Under Demand and Lead‑Time Uncertainty. *Sustainability*, 18(9), 4305.

1. Ahmadi, E., Masel, D. T., Hostetler, S., Maihani, R., & Ghalehkhandabi, I. (2019). A centralized stochastic inventory control model for perishable products considering age-dependent purchase price and lead time. *TOP, 28*, 231-269.

2. Arrow, K. J., Harris, T. E., & Marschak, J. (1951). Optimal inventory policy. *Econometrica, 19*(3), 250-272.

3. Iglehart, D. L. (1963). Optimality of (s, S) policies in the finite-horizon dynamic inventory problem. *Management Science, 9*(2), 259-267.

4. Makridakis, S., Spiliotis, E., & Assimakopoulos, V. (2018). Statistical and Machine Learning forecasting methods: Concerns and ways forward. *PLOS ONE, 13*(3), e0194889.

5. Perera, S. C., & Sethi, S. P. (2022). A survey of stochastic inventory models with fixed costs: Optimality of (s,S) and (s,S)-type policies – Discrete-time case. *Production and Operations Management, 31*(1), 1-23.

6. Scarf, H. (1960). The optimality of (s,S) policies in the dynamic inventory problem. In *Mathematical Methods in the Social Sciences* (pp. 196–202). Stanford University Press.

7. Shakya, M., Lee, B.-S., & Ng, H. Y. (2022). A Deep Reinforcement Learning Approach for Inventory Control under Stochastic Lead Time and Demand. *arXiv preprint*.

8. Tai, P. D., Huyen, P. P. N., & Buddhakulsomsiri, J. (2021). A novel modeling approach for a capacitated (S,T) inventory system with backlog under stochastic discrete demand and lead time. *International Journal of Industrial Engineering Computations, 12*(1), 1-14.

9. Taylor, S. J., & Letham, B. (2018). Forecasting at scale. *The American Statistician, 72*(1), 37-45.

10. You, D.-G., Wang, C.-H., & Li, Y.-T. (2026). Data-Driven Simulation-Optimization for Sustainable (s, S) Inventory Policy Design Under Demand and Lead-Time Uncertainty. *Sustainability, 18*(9), 4305.

---
---

## 📝 License

This project is for educational and portfolio purposes. Please cite the author and the reference above if reused.
