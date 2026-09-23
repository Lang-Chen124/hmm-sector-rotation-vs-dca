# HMM Sector Rotation vs. CSI 300 DCA

An internship research project comparing a three-state Hidden Markov Model (HMM) sector-rotation strategy with a monthly fixed-investment benchmark on the CSI 300.

The repository presents the complete research workflow: market-data collection, point-in-time feature construction, regime estimation, portfolio allocation, backtesting, performance evaluation, visualisation, and accounting checks.

> **Research only.** The results are historical simulations, not investment advice or evidence of guaranteed future performance.

## Research question

Can a market-regime model combined with industry momentum improve the performance of a regular monthly investment plan relative to passively investing the same cash flows in the CSI 300?

## Strategy overview

### Benchmark: CSI 300 monthly DCA

- Invest CNY 1,000 on the first trading day of each month.
- Use the CSI 300 price index as a virtual investment vehicle.
- Evaluate investor experience with terminal value, cumulative profit and XIRR.
- Evaluate the underlying strategy with time-weighted return and maximum drawdown.

### HMM sector rotation

- Estimate a three-state Gaussian HMM from CSI 300 log returns and 20-day annualised volatility.
- Refit the model with an expanding window every three months.
- Map latent states to Risk-On, Neutral and Risk-Off by in-sample state volatility.
- Generate signals after month-end and rebalance on the first trading day of the following month.
- Rank CSI Shenwan Level-1 industries using a weighted combination of 20-, 60- and 120-day momentum.
- Blend regime-specific portfolios using smoothed state probabilities.
- Compare the strategy and benchmark under identical dates and monthly external cash flows.

## Repository contents

| File | Description |
| --- | --- |
| [沪深300定投基准收益.ipynb](./沪深300定投基准收益.ipynb) | Builds the monthly CSI 300 DCA benchmark and reports cash-flow-aware performance measures. |
| [HMM市场状态与申万一级行业轮动策略.ipynb](./HMM市场状态与申万一级行业轮动策略.ipynb) | Implements expanding-window HMM regime detection, industry momentum selection, portfolio backtesting and benchmark comparison. |
| [requirements.txt](./requirements.txt) | Python dependencies required by the notebooks. |
| [data/README.md](./data/README.md) | Data sources, expected cache layout and data limitations. |
| [results/README.md](./results/README.md) | Output files produced by each notebook. |

## Backtest configuration

| Item | Setting |
| --- | --- |
| Data preparation period | 2015-01-01 to 2025-12-31 |
| Formal backtest period | 2016-01-01 to 2025-12-31 |
| Contribution | CNY 1,000 per month |
| Execution | First trading day of each month at the close |
| Market features | Daily log return and 20-day annualised volatility |
| HMM | 3-state Gaussian HMM |
| Momentum windows | 20, 60 and 120 trading days |
| Default trading costs | 0 in the current notebooks |
| Benchmark | CSI 300 price index monthly DCA |

## Selected results

The saved notebook outputs report the following results for the current configuration:

| Metric | CSI 300 DCA | HMM sector rotation |
| --- | ---: | ---: |
| Total contributions | CNY 120,000 | CNY 120,000 |
| Terminal value | CNY 143,713 | CNY 196,165 |
| Investment return | 19.76% | 63.47% |
| XIRR | 3.54% | 9.50% |
| Time-weighted CAGR | 2.93% | 5.62% |
| Annualised volatility | 18.86% | 22.03% |
| Sharpe ratio | 0.253 | 0.368 |
| Maximum drawdown | -45.60% | -51.16% |

The strategy achieved a higher historical return in this sample, but also had higher volatility, a deeper maximum drawdown and very high turnover. Because the current configuration assumes zero trading costs, these figures should be interpreted as a research comparison rather than an implementable performance claim.

## Installation

Python 3.11 is recommended.

```bash
git clone https://github.com/Lang-Chen124/hmm-sector-rotation-vs-dca.git
cd hmm-sector-rotation-vs-dca

python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Launch Jupyter:

```bash
jupyter lab
```

Run the notebooks in this order:

1. `沪深300定投基准收益.ipynb`
2. `HMM市场状态与申万一级行业轮动策略.ipynb`

Each notebook can run independently and automatically creates its own data cache and output directory. Public AKShare endpoints may occasionally be unavailable or rate-limited; rerun later or provide a compatible local CSV where supported.

## Research safeguards

The strategy notebook includes assertions that check:

- signal dates precede execution dates;
- rebalancing occurs on the first trading day of each month;
- state probabilities and target weights sum correctly;
- no negative cash balance is created;
- benchmark and strategy receive identical external cash flows;
- the portfolio accounting identity holds;
- required output series contain no missing values.

These checks reduce implementation errors but do not eliminate model risk or data bias.

## Limitations

- The HMM is unsupervised; economic regime names are interpretations imposed after fitting.
- The initial HMM training window is short and parameter estimates may be unstable.
- Today's Shenwan industry classification is applied retrospectively, which may introduce classification changes and survivorship bias.
- The benchmark and industry series are price indices and exclude dividends.
- Indices are not directly tradable; an implementation would require investable ETFs or funds.
- The current results assume zero fees, slippage, market impact and tracking error.
- Candidate industry pools and model parameters require out-of-sample robustness testing.
- Historical performance does not imply future performance.

Useful extensions include transaction-cost sensitivity, alternative state counts, longer initial training windows, walk-forward parameter selection, sub-period analysis and comparison with a full-industry momentum baseline.

## Data and licence

Market data are retrieved from public interfaces through AKShare. Data are not redistributed in this repository; users are responsible for complying with the terms of the original providers.

Code in this repository is released under the [MIT License](./LICENSE). Market data and third-party content are not covered by that licence.
