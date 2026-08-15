# ZNano MCU Profile

This document summarizes the MCU-oriented evidence currently available for ZNano. It intentionally separates **build/footprint evidence** from **runtime target performance**, which has not yet been measured on physical MCU boards in the public evidence set.

## Rebuilt SDK archive footprint

Measured with `arm-none-eabi-size -t` on 2026-08-15:

| MCU target | Variant | text B | data B | bss B | total B |
|---|---|---:|---:|---:|---:|
| Cortex-M0/M0+ | global encode+decode | 3261 | 4 | 30 | 3295 |
| Cortex-M0/M0+ | encode-only | 1592 | 2 | 9 | 1603 |
| Cortex-M0/M0+ | decode-only | 1028 | 2 | 9 | 1039 |
| Cortex-M3 | global encode+decode | 3167 | 4 | 30 | 3201 |
| Cortex-M3 | encode-only | 1556 | 2 | 9 | 1567 |
| Cortex-M3 | decode-only | 1008 | 2 | 9 | 1019 |
| Cortex-M4 soft-float | global encode+decode | 3171 | 4 | 30 | 3205 |
| Cortex-M4 soft-float | encode-only | 1556 | 2 | 9 | 1567 |
| Cortex-M4 soft-float | decode-only | 1012 | 2 | 9 | 1023 |
| Cortex-M4F hard-float | global encode+decode | 3179 | 4 | 30 | 3213 |
| Cortex-M4F hard-float | encode-only | 1556 | 2 | 9 | 1567 |
| Cortex-M4F hard-float | decode-only | 1012 | 2 | 9 | 1023 |

## Static profile harness

Current profile harness values using a HYDRUS 6-frame payload:

| CPU | text B | data B | bss B | Flash approx | Static RAM approx | Max single-function stack |
|---|---:|---:|---:|---:|---:|---:|
| Cortex-M0 | 2759 | 84 | 344 | 2835 B | 428 B | 80 B |
| Cortex-M3 | 2383 | 84 | 344 | 2459 B | 428 B | 56 B |
| Cortex-M4 | 2407 | 84 | 344 | 2483 B | 428 B | 56 B |

### Interpretation

- Static RAM above is **profile/harness evidence**, not a measured runtime peak on target hardware.
- Stack values are **static compiler/profile artifacts**, not peak runtime stack traces.
- Caller-owned input/output buffers and integration-layer memory must be budgeted separately.
- The optimized MCU SDK path uses fixed internal buffers and no dynamic allocation according to the current implementation scan; this claim is scoped to that SDK path.

## QEMU functional evidence

A Cortex-M3 bare-metal functional run is documented with:

- Machine: `lm3s6965evb`
- CPU: `cortex-m3`
- Result: **MATCH: YES**
- Payload: NEO-M8-FW3, 6 frames, 152 hex chars/frame
- Original stream: 912 hex characters
- Compressed stream: 349 hex characters
- Compression buffer: 512 B
- Decode buffer: 1024 B
- Harness ELF total: 7556 B

QEMU wall-clock timing is intentionally **not** published as MCU performance because emulator/process overhead is not equivalent to target cycles.

## Build targets represented

- Cortex-M0 / M0+
- Cortex-M3
- Cortex-M4 soft-float
- Cortex-M4F hard-float

## What can be published now

- Build support / artifact availability for the target families above
- Measured archive section sizes
- Static profile footprint values with clear qualification
- Functional QEMU round-trip evidence

## What still needs physical-board measurement

- cycles per encode/decode
- throughput by Cortex target and clock
- peak runtime RAM
- peak runtime stack
- energy per encoded frame
- radio/network savings translated into application-level battery impact

Those measurements would materially strengthen ZNano's embedded due-diligence package and should be treated as the next benchmark tier rather than inferred from host results.
