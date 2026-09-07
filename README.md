# A trading forecast that went wrong

This example comes from a historical simulation of a Bitcoin strategy. An LSTM estimates whether the next half hour will bring a price increase. The strategy holds Bitcoin when that probability reaches one half and otherwise holds cash. Forecasts use completed observations, with simulated execution at the following candle's opening price.

The primary model has one LSTM layer with 32 hidden units. The larger version has 128 hidden units. The language model was **GPT-6 Astra**, confirmed from the historical prediction sessions. [Model specifications and all 12 matched predictions](model-comparison.md) show the setup and each model's forecast beside the observed result.

## The failure example

On October 10, 2025, the forecast for the half hour beginning at 9 p.m. UTC assigned about 65% probability to an increase. The strategy was already holding Bitcoin. Instead, the price fell from 114,266.82 to 108,432.09 USDT, producing a loss of about 5.1% during that interval. There was no new entry cost because the position did not change.

![Bitcoin candlesticks and volume with the actual trade entry, exit, peak, low and maximum price drawdown](failure-example.png)

The position had entered at 2 p.m. and stayed invested until 10 p.m. UTC. The selected half-hour loss occurred inside that trade. Its beginning and end are not the trade's entry and exit.

| Trade event or measure | Verified observation |
| --- | --- |
| Entry on October 10 at 2 p.m. UTC | 121,684.18 USDT |
| Exit on October 10 at 10 p.m. UTC | 113,451.87 USDT |
| Highest price while invested | 122,069.76 during the 2 to 2.30 p.m. candle |
| Lowest price while invested | 102,000.00 during the 9 to 9.30 p.m. candle |
| Maximum price drawdown within the trade | 16.44% from the earlier high to the later low |
| Gross and net trade return | −6.77% and −6.93% |

The candlesticks show open, high, low and close, with volume below. The high occurs in an earlier candle than the low, which establishes the order needed for the price drawdown. Their exact intrabar times and achievable execution prices are unknown. Measuring only at successive half-hour openings gives a smaller drawdown of 10.89% because it misses the deepest intrabar fall.

I selected this example retrospectively because it was the most costly wrong long forecast in the primary run's later period. All five repeated training runs held Bitcoin during this interval. The next half hour rebounded by about 4.6%, and the primary strategy remained invested through that recovery. This example is not typical or an independent confirmation of a general failure pattern.

## Comparing model sizes

The following results compare the primary LSTM with a larger LSTM over identical evaluation periods. Directional error counts incorrect up or down predictions. Net returns compound the simulated strategy's returns after assumed fees and slippage totaling 0.09% for each one way position change. Each period starts separately from cash and includes final liquidation.

| Evaluation period | Primary error | Larger error | Primary net return | Larger net return |
| --- | --- | --- | --- | --- |
| July through December 2024 | 47.06% | 46.74% | −86.43% | −89.03% |
| January 2025 through April 2026 | 47.92% | 48.23% | −99.66% | −99.86% |

Frequent trading costs overwhelmed the primary strategy. Removing those assumed costs while holding its decisions fixed gives returns of about 34.82% and 0.49% in the two periods. A slightly better directional score therefore did not ensure a useful trading strategy.

## Comparing with a language model

A separate blinded comparison gave GPT-6 Astra the same historical inputs and prediction targets for 12 selected opportunities. It made seven directional errors. Each LSTM made four errors on those same cases. The [individual prediction table](model-comparison.md#individual-predictions) includes probabilities, forecast directions and actual returns, with wrong forecasts highlighted. This small comparison does not establish a general model ranking.

These are exploratory historical results from previously inspected data. No real trades occurred. The single larger model run and the language model comparison do not isolate a causal effect of model size. Losses alone also do not demonstrate concept drift.
