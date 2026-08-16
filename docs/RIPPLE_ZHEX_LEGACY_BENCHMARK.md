# ZHex / ZNano Legacy Benchmark — Ripple real-data regression

Generated: 2026-08-16T10:09:08.055549+00:00  
Host: Darwin 25.5.0 arm64 / Python 3.12.10  
Iterations per case: 30

## Scope

This historical regression run uses only real/local Ripple meter payloads extracted from the preserved encoder material, including the `Real` / `Realtime` sections. Synthetic generator datasets are excluded from the default report.

The benchmark is retained as lineage evidence for the early customer-specific codec that preceded ZNano. It does not expose the proprietary compression mechanism.

## Summary

| Variant | Cases | Valid round-trips | Avg reduction | Avg encode ms | Avg decode ms |
| --- | ---: | ---: | ---: | ---: | ---: |
| current_zhex_c | 5 | 5 | 63.48% | 7.782 | 1.608 |
| legacy_zhex_c_ori | 5 | 5 | 63.48% | 2.578 | 1.529 |
| legacy_global_c | 5 | 0 | 0.00% | 3.203 | 9.371 |

## Detail

| Variant | Case | Measurements | Raw chars | Encoded chars | Reduction | Round-trip | Encode avg ms | Decode avg ms |
| --- | --- | ---: | ---: | ---: | ---: | --- | ---: | ---: |
| current_zhex_c | real_time_meter_data_from_server_6_packets | 6 | 144 | 81 | 43.75% | OK | 32.904 | 2.008 |
| current_zhex_c | real_time_meter_data_from_server_7_packets | 7 | 168 | 91 | 45.83% | OK | 1.486 | 1.502 |
| current_zhex_c | realtime_server_data_same_data_repeated_12_times_as_12_packets | 12 | 288 | 53 | 81.60% | OK | 1.506 | 1.513 |
| current_zhex_c | realtime_server_data_same_data_repeated_13_times_as_13_packets | 13 | 312 | 53 | 83.01% | OK | 1.504 | 1.523 |
| current_zhex_c | realtime_server_data_6_repeated_packets | 6 | 144 | 53 | 63.19% | OK | 1.508 | 1.496 |
| legacy_zhex_c_ori | real_time_meter_data_from_server_6_packets | 6 | 144 | 81 | 43.75% | OK | 6.970 | 1.743 |
| legacy_zhex_c_ori | real_time_meter_data_from_server_7_packets | 7 | 168 | 91 | 45.83% | OK | 1.448 | 1.444 |
| legacy_zhex_c_ori | realtime_server_data_same_data_repeated_12_times_as_12_packets | 12 | 288 | 53 | 81.60% | OK | 1.480 | 1.474 |
| legacy_zhex_c_ori | realtime_server_data_same_data_repeated_13_times_as_13_packets | 13 | 312 | 53 | 83.01% | OK | 1.475 | 1.481 |
| legacy_zhex_c_ori | realtime_server_data_6_repeated_packets | 6 | 144 | 53 | 63.19% | OK | 1.516 | 1.504 |
| legacy_global_c | real_time_meter_data_from_server_6_packets | 6 | 144 | 134 | 6.94% | FAIL | 7.415 | 9.485 |
| legacy_global_c | real_time_meter_data_from_server_7_packets | 7 | 168 | 156 | 7.14% | FAIL | 2.132 | 9.457 |
| legacy_global_c | realtime_server_data_same_data_repeated_12_times_as_12_packets | 12 | 288 | 238 | 17.36% | FAIL | 2.167 | 9.343 |
| legacy_global_c | realtime_server_data_same_data_repeated_13_times_as_13_packets | 13 | 312 | 256 | 17.95% | FAIL | 2.153 | 9.222 |
| legacy_global_c | realtime_server_data_6_repeated_packets | 6 | 144 | 130 | 9.72% | FAIL | 2.145 | 9.347 |

## Notes

- `roundtrip_ok` compares decoded uppercase hex to the original uppercase hex.
- The default run uses only real/local meter payloads extracted from the historical Ripple encoder files.
- The Windows `.exe` files were not executed in this run; the benchmark compiles the matching C/C++ sources locally.
- `legacy_global_c` is retained as a historical reference and fails round-trip on these current regression cases; its apparent size reductions must therefore not be presented as valid compression results.
- Host command timing is included for reproducibility but should not be presented as MCU runtime performance.

## Interpretation

The purpose of this benchmark is historical validation, not comparison with the current ZNano public benchmark suite.

The strongest supported conclusion is that two preserved, functioning implementations from the Ripple/ZHex lineage still reconstruct **5/5 real-data cases exactly** while producing an average encoded-size reduction of **63.48%** on this small historical dataset.

For the broader, device-agnostic current evidence set, see [`BENCHMARKS_2026-08-15.md`](BENCHMARKS_2026-08-15.md).
