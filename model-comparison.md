# Comparing the LSTM and language model forecasts

[Return to the failure example](README.md)

The LSTM and the language model predicted the same next half hour direction on 12 selected Bitcoin opportunities. Both LSTMs made four directional errors. The language model made seven. The table below shows every forecast, including the cases where the models disagreed.

## Models used

| Specification | Primary LSTM | Larger LSTM |
| --- | --- | --- |
| Recurrent architecture | One LSTM layer with 32 hidden units | One LSTM layer with 128 hidden units |
| Trainable parameters | 4,513 | 67,201 |
| Output | One linear output followed by a sigmoid | One linear output followed by a sigmoid |
| Selected training epoch | 1 | 12 |

Both LSTMs received the previous 20 completed candles' close to close returns. Each candle covers 30 minutes. Inputs were standardized using training data alone. The target was whether the next executable open to open return was positive. A probability of at least 50% meant an up forecast and a long position. Otherwise the strategy held cash.

Training used binary cross entropy and Adam with a learning rate of 0.001 and batches of 128. The training period ran from October 2022 through December 2023. January through June 2024 supplied validation data. Training allowed up to 50 epochs and stopped after five epochs without improved validation loss. The best validation checkpoint was selected. This comparison uses the primary seed for each size. Four additional seeds were evaluated for the smaller model.

The language model was **GPT-6 Astra**, as identified in the historical prediction session records.

## Individual predictions

Times below give the simulated entry at a candle's opening price in UTC. Each target ends at the following opening price, 30 minutes later. Each forecast cell gives the probability of an increase and the resulting direction. **Bold forecasts are wrong.** The actual return is the price change before trading costs.

| Entry time in UTC | Actual return and direction | Primary LSTM | Larger LSTM | GPT-6 Astra |
| --- | --- | --- | --- | --- |
| 2024-07-19 09.30 | -0.2911% down | 49.82% down | 49.72% down | **72.00% up** |
| 2024-08-17 20.00 | -0.1553% down | **50.37% up** | 48.05% down | **65.00% up** |
| 2024-09-16 06.30 | -0.0729% down | 46.84% down | 41.97% down | **67.00% up** |
| 2024-10-15 16.30 | +0.3734% up | **41.94% down** | **38.05% down** | 60.00% up |
| 2024-11-14 03.00 | -0.2248% down | 49.83% down | 45.36% down | **60.00% up** |
| 2024-12-13 13.30 | +0.1714% up | 52.92% up | 55.59% up | **48.00% down** |
| 2025-02-18 12.00 | +0.3111% up | 51.22% up | 52.09% up | 60.00% up |
| 2025-05-07 02.00 | -0.3649% down | 49.61% down | **54.08% up** | 46.00% down |
| 2025-07-23 16.30 | -0.1352% down | **53.47% up** | **55.50% up** | 40.00% down |
| 2025-10-09 06.30 | -0.1139% down | **50.74% up** | **52.28% up** | **63.00% up** |
| 2025-12-25 21.00 | +0.0745% up | 52.25% up | 51.41% up | 63.00% up |
| 2026-03-13 11.00 | -0.1816% down | 44.47% down | 43.51% down | **61.00% up** |

## Matched results

| Model | Errors in the six 2024 cases | Errors in the six later cases | Total errors | Mean net return per opportunity |
| --- | --- | --- | --- | --- |
| Primary LSTM | 2 of 6 | 2 of 6 | 4 of 12 | -0.0773% |
| Larger LSTM | 1 of 6 | 3 of 6 | 4 of 12 | -0.0947% |
| GPT-6 Astra | 5 of 6 | 2 of 6 | 7 of 12 | -0.1583% |

The net figure treats every opportunity as an isolated decision starting from cash. A long forecast buys at the first opening price and sells at the next one. Cash earns zero. Fees and slippage are assumed to total 0.09% for each one way trade. These averages are not a continuous portfolio return or a drawdown estimate.

## What this comparison supports

The cases were selected at six evenly spaced positions in each evaluation period. Each language model forecast came from a fresh context with the same 20 standardized returns used by the LSTMs and eight fixed examples from the training period. It received no ticker, timestamp, future price or evaluation label. Each case received one response with no retries. All three models used the same target, decision threshold and scoring.

The language model sometimes missed a decline that both LSTMs predicted. On July 23, 2025, it correctly predicted a decline that both LSTMs missed. A correct up forecast could still lose money when the increase was smaller than trading costs.

Twelve observations are too few to establish a general model ranking. The LLM received a few training examples in its prompt, while the LSTMs learned from the full training period. Possible historical overlap with the LLM's pretraining data is unknown. The comparison therefore does not isolate the effect of model size. These are exploratory historical simulations, not live trades.
