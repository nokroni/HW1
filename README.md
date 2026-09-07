# Bitcoin trading results

These results come from a historical Bitcoin trading simulation. The strategy holds Bitcoin when the predicted probability of a price increase reaches one half and otherwise holds cash. Forecasts use completed observations, with simulated execution at the following candle's opening price. No real trades occurred.

The primary model has one LSTM layer with 32 hidden units. The larger version has 128 hidden units. The language model used was GPT-6 Astra. [Model specifications, access method and all 12 matched predictions](model-comparison.md) accompany the results.

## Observed trade

On October 10, 2025, the forecast for the half hour beginning at 9 p.m. UTC assigned about 65% probability to an increase. The strategy held Bitcoin throughout the interval. The opening price fell from 114,266.82 to 108,432.09 USDT, a return of about −5.1%. Turnover and trading costs were zero during this interval.

![Bitcoin candlesticks and volume with the actual trade entry, exit, peak, low and maximum price drawdown](failure-example.png)

The complete position ran from 2 p.m. to 10 p.m. UTC on the same day.

| Trade event or measure | Result |
| --- | --- |
| Entry on October 10 at 2 p.m. UTC | 121,684.18 USDT |
| Exit on October 10 at 10 p.m. UTC | 113,451.87 USDT |
| Highest price while invested | 122,069.76 during the 2 to 2.30 p.m. candle |
| Lowest price while invested | 102,000.00 during the 9 to 9.30 p.m. candle |
| Maximum price drawdown within the trade | 16.44% from the earlier high to the later low |
| Gross and net trade return | −6.77% and −6.93% |

The candlesticks show open, high, low and close, with volume below. High and low timestamps refer to their half-hour candles. Exact intrabar times are unavailable. Maximum drawdown measured at successive half-hour openings was 10.89%.

All five repeated training runs held Bitcoin during the 9 p.m. interval. The next half hour returned about +4.6%, and the primary strategy remained invested.

## Comparing model sizes

The following results compare the primary LSTM with a larger LSTM over identical evaluation periods. Directional error counts incorrect up or down predictions. Net returns compound the simulated strategy's returns after assumed fees and slippage totaling 0.09% for each one way position change. Each period starts separately from cash and includes final liquidation.

| Evaluation period | Primary error | Larger error | Primary net return | Larger net return |
| --- | --- | --- | --- | --- |
| July through December 2024 | 47.06% | 46.74% | −86.43% | −89.03% |
| January 2025 through April 2026 | 47.92% | 48.23% | −99.66% | −99.86% |

The primary strategy's returns before costs were +34.82% in the first period and +0.49% in the second period.

## Comparing with a language model

A separate comparison gave GPT-6 Astra the same historical inputs and prediction targets for 12 opportunities. It made seven directional errors. Each LSTM made four errors on those same cases. The [individual prediction table](model-comparison.md#individual-predictions) includes probabilities, forecast directions and actual returns, with wrong forecasts highlighted.
