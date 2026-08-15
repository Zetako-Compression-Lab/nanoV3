# ZNano Benchmark Rerun — 2026-08-15

This document is the publication-oriented summary of the fresh ZNano host CLI rerun performed on 2026-08-15.

## Test environment

- Host: `Darwin 25.5.0 (arm64)`
- Binary SHA-256: `aa9ea4025b3971a94117112c69803ab88864ae66a95002b5dc192f2a33d73019`
- Runs: **30 measured runs** after **5 warmup runs** per encode/decode command
- Baseline RSS: **1,376,256 B**
- Integrity method: SHA-256 comparison of normalized input hex and normalized decoded hex

These are **host CLI measurements**, not MCU cycle measurements.

## Structured telemetry results

| Dataset | Case | Frames | Input B | Output B | Reduction | SHA-256 | Enc ms ± std | Dec ms ± std | Enc KB/s | Dec KB/s | Peak RSS delta B |
|---|---:|---:|---:|---:|---:|---|---:|---:|---:|---:|---:|
| HYDRUS-F06-006-WATER | 1 | 6 | 216 | 149 | 31.02% | Yes | 1.498 ± 0.123 | 1.585 ± 0.260 | 140.8 | 133.1 | 32768 |
| HYDRUS-F06-006-WATER | 2 | 20 | 720 | 361 | 49.86% | Yes | 1.473 ± 0.108 | 1.466 ± 0.061 | 477.3 | 479.7 | 49152 |
| HYDRUS-F06-006-WATER | 3 | 40 | 1440 | 641 | 55.49% | Yes | 1.380 ± 0.048 | 1.616 ± 0.065 | 1019.1 | 870.4 | 49152 |
| HYDRUS-F06-006-WATER | 4 | 10 | 820 | 317 | 61.34% | Yes | 1.351 ± 0.053 | 1.481 ± 0.039 | 592.8 | 540.5 | 65536 |
| HYDRUS-F06-006-WATER | 5 | 40 | 3280 | 300 | 90.85% | Yes | 1.395 ± 0.044 | 2.016 ± 0.051 | 2296.4 | 1589.0 | 65536 |
| LANDIS_GYR_E450 | 1 | 6 | 324 | 127 | 60.80% | Yes | 1.387 ± 0.068 | 1.401 ± 0.063 | 228.1 | 225.8 | 32768 |
| LANDIS_GYR_E450 | 2 | 10 | 540 | 182 | 66.30% | Yes | 1.393 ± 0.081 | 1.491 ± 0.069 | 378.7 | 353.7 | 49152 |
| LANDIS_GYR_E450 | 3 | 20 | 1080 | 317 | 70.65% | **No** | 1.422 ± 0.054 | 1.592 ± 0.078 | 741.6 | 662.5 | 49152 |
| LANDIS_GYR_E450 | 4 | 40 | 2160 | 1193 | 44.77% | **No** | 1.476 ± 0.050 | 1.817 ± 0.073 | 1429.5 | 1160.9 | 49152 |
| NEO-M8-FW3 | 1 | 6 | 456 | 349 | 23.46% | Yes | 1.382 ± 0.032 | 1.407 ± 0.050 | 322.2 | 316.4 | 65536 |
| NEO-M8-FW3 | 2 | 10 | 760 | 232 | 69.47% | Yes | 1.416 ± 0.057 | 1.486 ± 0.045 | 524.3 | 499.4 | 65536 |
| NEO-M8-FW3 | 3 | 20 | 1520 | 803 | 47.17% | **No** | 1.428 ± 0.042 | 1.671 ± 0.043 | 1039.5 | 888.6 | 65536 |
| NEO-M8-FW3 | 4 | 40 | 3040 | 1443 | 52.53% | **No** | 1.502 ± 0.042 | 1.993 ± 0.046 | 1976.8 | 1489.6 | 65536 |
| SHARKY-775-159 | 1 | 6 | 456 | 207 | 54.61% | Yes | 1.392 ± 0.043 | 1.417 ± 0.050 | 319.9 | 314.2 | 65536 |
| SHARKY-775-159 | 2 | 10 | 760 | 617 | 18.82% | **No** | 1.453 ± 0.064 | 1.514 ± 0.054 | 510.6 | 490.1 | 65536 |
| SHARKY-775-159 | 3 | 20 | 1520 | 563 | 62.96% | **No** | 1.547 ± 0.138 | 1.685 ± 0.053 | 959.7 | 880.9 | 65536 |
| SHARKY-775-159 | 4 | 40 | 3040 | 2067 | 32.01% | **No** | 1.591 ± 0.058 | 2.022 ± 0.045 | 1866.1 | 1468.0 | 65536 |

## Aggregates

| Scope | Rows | SHA pass | Reduction range | Reduction mean | Reduction median | Enc mean | Dec mean | Enc KB/s mean | Dec KB/s mean | Max RSS delta |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Structured all | 17 | 10 / 17 | 18.82% to 90.85% | 52.48% | 54.61% | 1.440 ms | 1.627 ms | 872.0 | 727.2 | 65536 B |
| **Structured verified only** | **10** | **10 / 10** | **23.46% to 90.85%** | **56.32%** | **58.14%** | **1.407 ms** | **1.537 ms** | **630.0** | **532.2** | **65536 B** |

## Verified reduction by device

Mean reduction using only SHA-verified structured rerun rows:

| Device | Verified rows | Mean reduction | Best verified reduction |
|---|---:|---:|---:|
| HYDRUS-F06-006-WATER | 5 | 57.71% | 90.85% |
| LANDIS+GYR E450 | 2 | 63.55% | 66.30% |
| NEO-M8-FW3 | 2 | 46.46% | 69.47% |
| SHARKY-775-159 | 1 | 54.61% | 54.61% |

## Incompressible control results

Deterministic pseudo-random hexadecimal payloads, 10 frames each:

| Payload size | Input B | Output B | Reduction | SHA-256 | Enc ms | Dec ms |
|---|---:|---:|---:|---|---:|---:|
| 72 hex/frame | 360 | 371 | -3.06% | Yes | 1.469 | 1.418 |
| 108 hex/frame | 540 | 1111 | -105.74% | Yes | 1.500 | 1.478 |
| 152 hex/frame | 760 | 781 | -2.76% | Yes | 1.554 | 1.497 |
| 164 hex/frame | 820 | 1685 | -105.49% | Yes | 1.599 | 1.555 |

All 4 incompressible controls round-trip correctly, but the encoded representation can be larger than the input. This is important integration evidence: a deployment requiring a strict no-expansion guarantee should add an explicit bypass/fallback policy.

## Publication rule

Compression numbers from rows with `SHA-256: No` are retained for investigation and transparency, but they are **not used as evidence for public lossless claims or verified aggregate compression claims**.
