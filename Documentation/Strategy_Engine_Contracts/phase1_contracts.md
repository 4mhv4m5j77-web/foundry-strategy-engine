# Strategy Engine Phase 1 Contracts

Generated from `foundry.data.contracts` and `foundry.data.schemas`.

This export is the handoff surface for the strategy-engine repository. It captures:

- the complete 32-column `phase1_float32` storage contract
- the complete 32-column `fixed_point_v2` storage contract
- the current experimental fixed-point candidates that are not yet canonical

## Contract Summary

- `phase1_float32`: 32 columns
- `fixed_point_v2`: 32 columns
- experimental candidates: 8

## `phase1_float32`

| Column | Ordinal | Persisted | Training | Nullable | Fixed Point | Scale | Family |
| --- | ---: | --- | --- | --- | --- | ---: | --- |
| time | 1 | Datetime(us) | Datetime(us) | no | no |  | timestamp |
| symbol | 2 | Utf8 | Utf8 | no | no |  | symbol |
| open | 3 | Float32 | Float32 | no | no |  | price_level |
| high | 4 | Float32 | Float32 | no | no |  | price_level |
| low | 5 | Float32 | Float32 | no | no |  | price_level |
| close | 6 | Float32 | Float32 | no | no |  | price_level |
| volume | 7 | UInt32 | Float32 | no | no |  | count |
| ema_9 | 8 | Float32 | Float32 | yes | no |  | trend |
| sma_20 | 9 | Float32 | Float32 | yes | no |  | trend |
| sma_50 | 10 | Float32 | Float32 | yes | no |  | trend |
| macd | 11 | Float32 | Float32 | yes | no |  | trend |
| macd_signal | 12 | Float32 | Float32 | yes | no |  | trend |
| macd_hist | 13 | Float32 | Float32 | yes | no |  | trend |
| rsi_14 | 14 | Float32 | Float32 | yes | no |  | bounded_oscillator |
| stoch_k | 15 | Float32 | Float32 | yes | no |  | bounded_oscillator |
| stoch_d | 16 | Float32 | Float32 | yes | no |  | bounded_oscillator |
| adx_14 | 17 | Float32 | Float32 | yes | no |  | bounded_oscillator |
| bb_middle | 18 | Float32 | Float32 | yes | no |  | volatility |
| atr_14 | 19 | Float32 | Float32 | yes | no |  | volatility |
| volatility_20 | 20 | Float32 | Float32 | yes | no |  | volatility |
| vwap | 21 | Float32 | Float32 | yes | no |  | liquidity |
| vwap_deviation | 22 | Float32 | Float32 | yes | no |  | liquidity |
| volume_sma_20 | 23 | Float32 | Float32 | yes | no |  | liquidity |
| amihud_illiq | 24 | Float32 | Float32 | yes | no |  | liquidity |
| close_location_value | 25 | Float32 | Float32 | yes | no |  | bounded_fraction |
| up_down_volume_ratio | 26 | Float32 | Float32 | yes | no |  | liquidity |
| returns_1 | 27 | Float32 | Float32 | yes | no |  | returns |
| returns_5 | 28 | Float32 | Float32 | yes | no |  | returns |
| returns_10 | 29 | Float32 | Float32 | yes | no |  | returns |
| high_low_range | 30 | Float32 | Float32 | yes | no |  | range_ratio |
| close_to_high | 31 | Float32 | Float32 | yes | no |  | range_ratio |
| bar_return_skew | 32 | Float32 | Float32 | yes | no |  | returns |

## `fixed_point_v2`

| Column | Ordinal | Persisted | Training | Nullable | Fixed Point | Scale | Family |
| --- | ---: | --- | --- | --- | --- | ---: | --- |
| time | 1 | Datetime(us) | Datetime(us) | no | no |  | timestamp |
| symbol | 2 | Utf8 | Utf8 | no | no |  | symbol |
| open | 3 | Float32 | Float32 | no | no |  | price_level |
| high | 4 | Float32 | Float32 | no | no |  | price_level |
| low | 5 | Float32 | Float32 | no | no |  | price_level |
| close | 6 | Float32 | Float32 | no | no |  | price_level |
| volume | 7 | UInt32 | Float32 | no | no |  | count |
| ema_9 | 8 | Float32 | Float32 | yes | no |  | trend |
| sma_20 | 9 | Float32 | Float32 | yes | no |  | trend |
| sma_50 | 10 | Float32 | Float32 | yes | no |  | trend |
| macd | 11 | Float32 | Float32 | yes | no |  | trend |
| macd_signal | 12 | Float32 | Float32 | yes | no |  | trend |
| macd_hist | 13 | Float32 | Float32 | yes | no |  | trend |
| rsi_14 | 14 | UInt16 | Float32 | yes | yes | 100 | bounded_oscillator |
| stoch_k | 15 | UInt16 | Float32 | yes | yes | 100 | bounded_oscillator |
| stoch_d | 16 | UInt16 | Float32 | yes | yes | 100 | bounded_oscillator |
| adx_14 | 17 | UInt16 | Float32 | yes | yes | 100 | bounded_oscillator |
| bb_middle | 18 | Float32 | Float32 | yes | no |  | volatility |
| atr_14 | 19 | Float32 | Float32 | yes | no |  | volatility |
| volatility_20 | 20 | Float32 | Float32 | yes | no |  | volatility |
| vwap | 21 | Float32 | Float32 | yes | no |  | liquidity |
| vwap_deviation | 22 | Float32 | Float32 | yes | no |  | liquidity |
| volume_sma_20 | 23 | Float32 | Float32 | yes | no |  | liquidity |
| amihud_illiq | 24 | Float32 | Float32 | yes | no |  | liquidity |
| close_location_value | 25 | UInt16 | Float32 | yes | yes | 10000 | bounded_fraction |
| up_down_volume_ratio | 26 | Float32 | Float32 | yes | no |  | liquidity |
| returns_1 | 27 | Float32 | Float32 | yes | no |  | returns |
| returns_5 | 28 | Float32 | Float32 | yes | no |  | returns |
| returns_10 | 29 | Float32 | Float32 | yes | no |  | returns |
| high_low_range | 30 | Float32 | Float32 | yes | no |  | range_ratio |
| close_to_high | 31 | Float32 | Float32 | yes | no |  | range_ratio |
| bar_return_skew | 32 | Float32 | Float32 | yes | no |  | returns |

## Fixed-Point Deltas

| Column | Ordinal | Persisted | Training | Nullable | Fixed Point | Scale | Family |
| --- | ---: | --- | --- | --- | --- | ---: | --- |
| rsi_14 | 14 | UInt16 | Float32 | yes | yes | 100 | bounded_oscillator |
| stoch_k | 15 | UInt16 | Float32 | yes | yes | 100 | bounded_oscillator |
| stoch_d | 16 | UInt16 | Float32 | yes | yes | 100 | bounded_oscillator |
| adx_14 | 17 | UInt16 | Float32 | yes | yes | 100 | bounded_oscillator |
| close_location_value | 25 | UInt16 | Float32 | yes | yes | 10000 | bounded_fraction |

## Experimental Candidates

| Column | Ordinal | Persisted | Training | Nullable | Fixed Point | Scale | Family |
| --- | ---: | --- | --- | --- | --- | ---: | --- |
| rsi_2 | 1 | UInt16 | Float32 | yes | yes | 100 | bounded_oscillator |
| sector_breadth_50d | 2 | UInt16 | Float32 | yes | yes | 10000 | bounded_fraction |
| zweig_breadth_thrust | 3 | UInt16 | Float32 | yes | yes | 10000 | bounded_fraction |
| days_to_earnings | 4 | UInt16 | Float32 | yes | no |  | calendar_count |
| days_since_earnings | 5 | UInt16 | Float32 | yes | no |  | calendar_count |
| regime_age_days | 6 | UInt16 | Float32 | yes | no |  | calendar_count |
| fomc_days_to_next | 7 | UInt16 | Float32 | yes | no |  | calendar_count |
| fomc_days_since_last | 8 | UInt16 | Float32 | yes | no |  | calendar_count |
