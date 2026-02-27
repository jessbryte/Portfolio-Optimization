# Portfolio Optimization — Markowitz MPT & Monte Carlo Simulation

Systematic portfolio construction using Modern Portfolio Theory (MPT). This project applies **Markowitz mean-variance optimisation** and **Monte Carlo simulation** to real equity data to identify optimal portfolio allocations across three portfolio sizes (3, 6, and 8 stocks).

Live price data is fetched directly from Yahoo Finance via the `yfinance` API — no static datasets.

---

## What This Project Does

Given a set of equity tickers, the notebooks:

1. **Fetch live historical price data** via `yfinance` and compute daily/annualised returns and the covariance matrix
2. **Run Monte Carlo simulation** — generate 50,000+ random portfolio weight combinations and calculate the expected return, volatility, and Sharpe ratio for each
3. **Plot the Efficient Frontier** — the risk-return tradeoff curve across all simulated portfolios
4. **Identify two optimal portfolios** using `scipy.optimize`:
   - **Maximum Sharpe Ratio Portfolio** — maximises risk-adjusted return
   - **Minimum Variance Portfolio** — minimises portfolio volatility regardless of return
5. **Visualise results** — scatter plot of all simulated portfolios colour-coded by Sharpe ratio, with the two optimal points highlighted

---

## Notebooks

| Notebook | Description |
|---|---|
| `3_Stocks_yfinance_Portfolio_Optimization.ipynb` | 3-stock portfolio — baseline case |
| `Portfolio_Optimization_6_Stocks_yfinance.ipynb` | 6-stock portfolio — moderate diversification |
| `8_Stocks_yfinance_MC_Portfolio_Optimization.ipynb` | 8-stock portfolio — Monte Carlo focus |
| `8_Stocks_yfinance_Portfolio_Optimization.ipynb` | 8-stock portfolio — optimisation focus |

Each notebook is self-contained and can be run independently by swapping in any ticker symbols.

---

## Key Results

| Metric | Value |
|---|---|
| Monte Carlo simulations per run | 50,000+ |
| Max Sharpe Ratio achieved | > 1.65 |
| Max annualised return found | > 50% |
| Portfolio sizes tested | 3, 6, 8 stocks |
| Data source | Yahoo Finance (live via yfinance API) |

> These figures are based on the historical period used during development. Results will vary depending on the tickers selected and the date range of the analysis.

---

## Methodology

### Modern Portfolio Theory (Markowitz, 1952)
MPT formalises the intuition that diversification reduces risk. Given `N` assets, an investor constructs a portfolio by allocating weights `w₁, w₂, ..., wN` (summing to 1) to minimise variance for a target return, or equivalently to maximise the Sharpe ratio.

**Portfolio return:**
```
E(Rp) = Σ wᵢ · E(Rᵢ)
```

**Portfolio variance:**
```
σ²p = wᵀ · Σ · w
```
where `Σ` is the covariance matrix of asset returns.

**Sharpe Ratio:**
```
S = (E(Rp) − Rf) / σp
```
A risk-free rate of 0 is used here for simplicity (adjustable in code).

### Monte Carlo Simulation
Rather than analytically solving for the full frontier, the simulation approach samples the weight space stochastically — generating tens of thousands of random valid portfolios and recording their risk-return coordinates. This traces the shape of the efficient frontier empirically and is computationally intuitive.

### SciPy Constrained Optimisation
`scipy.optimize.minimize` is used with:
- **Objective function**: negative Sharpe ratio (for maximisation) or portfolio variance
- **Constraints**: weights sum to 1.0
- **Bounds**: each weight in [0, 1] (long-only)

---

## Installation

```bash
git clone https://github.com/jessbryte/Portfolio-Optimization.git
cd Portfolio-Optimization
pip install -r requirements.txt
```

### Dependencies

```
yfinance
numpy
pandas
scipy
matplotlib
jupyter
```

Or install directly:
```bash
pip install yfinance numpy pandas scipy matplotlib jupyter
```

---

## Usage

1. Open any notebook in Jupyter:
```bash
jupyter notebook 8_Stocks_yfinance_MC_Portfolio_Optimization.ipynb
```

2. Edit the tickers list at the top of the notebook:
```python
tickers = ['AAPL', 'MSFT', 'GOOGL', 'AMZN', 'META', 'TSLA', 'NVDA', 'JPM']
```

3. Set your date range:
```python
start_date = '2020-01-01'
end_date   = '2024-12-31'
```

4. Run all cells. The notebook will fetch live data, run the simulation, and output the efficient frontier plot with the two optimal portfolios marked.

---

## Output

Each notebook produces:

- **Efficient Frontier scatter plot** — all simulated portfolios, colour-coded by Sharpe ratio
- **Optimal portfolio annotations** — Maximum Sharpe Ratio and Minimum Variance portfolios highlighted
- **Weight allocation table** — exact percentage allocation to each asset at the optimum
- **Performance summary** — expected annual return, annual volatility, and Sharpe ratio for each optimal portfolio

---

## Project Structure

```
Portfolio-Optimization/
├── 3_Stocks_yfinance_Portfolio_Optimization.ipynb
├── 8_Stocks_yfinance_MC_Portfolio_Optimization.ipynb
├── 8_Stocks_yfinance_Portfolio_Optimization.ipynb
├── Portfolio_Optimization_6_Stocks_yfinance.ipynb
├── README.md
└── .gitignore
```

---

## Theoretical Background

- **Harry Markowitz** (1952) — *Portfolio Selection*, Journal of Finance. Nobel Prize in Economics, 1990.
- **Efficient Frontier** — the set of portfolios that offer the highest expected return for a given level of risk.
- **Capital Market Line** — connects the risk-free asset to the tangency portfolio (Maximum Sharpe Ratio point) on the efficient frontier.
- **Modern Portfolio Theory** forms the foundation of quantitative asset management and is directly applied in institutional portfolio construction.

---

## Limitations & Extensions

This project implements the classical MPT framework. Known limitations and potential extensions:

| Limitation | Possible Extension |
|---|---|
| Uses historical returns as expected returns | Implement forward-looking return estimates (e.g. CAPM, Black-Litterman) |
| Assumes normal return distributions | Model fat tails using CVaR or robust optimisation |
| Long-only constraints | Add short-selling capability |
| No transaction costs | Add rebalancing cost model |
| Point-in-time optimisation | Add rolling walk-forward backtesting |
| No benchmark comparison | Compare against SPY / equal-weight portfolio |

---

## Authors

**Jessie I. Ifeanyi**
MSc Mathematical Engineering — Università degli Studi dell'Aquila, Italy
ACA Professional Level Ongoing - Institute of Chartered Accountants of Nigeria
[GitHub](https://github.com/jessbryte) · [LinkedIn](https://linkedin.com/in/jessie-ifeanyi)

**Amos T. Ezeh**
MSc Mathematics — Università degli Studi dell'Aquila, Italy
FMVA & BIDA Certified — Corporate Finance Institute
[GitHub](https://github.com/AmoscoTk) · [LinkedIn](https://linkedin.com/in/amoscotk)

---

## License

MIT License — see [LICENSE](LICENSE) for details.

> **Disclaimer:** This project is for educational and research purposes only. It does not constitute financial advice. Past performance does not predict future results. Always consult a qualified financial advisor before making investment decisions.
