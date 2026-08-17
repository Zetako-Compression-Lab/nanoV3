# ZNano Public Claims

This document separates claims that are currently supportable from claims that should remain internal or qualified until stronger evidence exists.

## Safe claims

| Claim | Status | Scope |
|---|---|---|
| Lossless encode/decode | Verified | 10 structured rerun rows + 4 incompressible rerun rows with SHA-256 match in the last completed public rerun |
| Stream decode mode | Verified | Benchmarks use decode-all stream mode |
| Multiple concatenated frames | Verified | 6, 10, 20, and 40-frame cases depending on dataset in the last completed public rerun |
| SHA-256 integrity validation | Verified | Normalized input and decoded output are compared |
| Compression on verified structured telemetry | Calculated | 23.46% to 90.85% reduction in the last completed public rerun |
| Average compression on verified structured telemetry | Calculated | 56.32% mean, 58.14% median in the last completed public rerun |
| Host encode/decode timing | Measured | macOS Darwin 25.5.0 arm64 CLI rerun |
| Host RSS delta upper bound | Calculated | Up to 64 KB in the last completed public rerun |
| Random-access frame selection | Integration capability | A selected frame can be decoded from a multi-frame stream without first reconstructing every preceding frame |
| Payload scrambling | Integration capability | Optional deterministic reversible scrambling of the encoded payload |
| Scrambling overhead | Integration contract | One additional byte in the encoded stream when scrambling is enabled |
| V1 frame-size bound | Integration contract | 1 to 4095 hex characters per frame |
| V1 stream-length bound | Integration contract | Input and encoded streams up to 65535 hex characters |
| Optimized MCU SDK path uses no dynamic allocation | Verified from implementation scan | Applies specifically to the optimized MCU SDK path, not every legacy source path |
| MCU target builds exist | Verified | Cortex-M0/M0+, M3, M4 soft-float, M4F hard-float |
| MCU archive footprint | Measured | Around 3.2 KB total for global encode+decode archives, depending on target |
| MCU RAM | Target-dependent estimate/profile | Not a direct runtime board measurement |
| MCU stack | Static profile evidence | 80 B max single function on M0, 56 B on M3/M4 for current harness; not runtime peak |
| Incompressible payload integrity | Verified | 4/4 deterministic pseudo-random controls round-trip in the last completed public rerun |
| Incompressible payload expansion | Measured | Expansion observed; reduction from -105.74% to -2.76% in the last completed public rerun |

## Recommended public wording

- **“ZNano was verified lossless on 10 structured telemetry rerun rows and 4 incompressible control rows using SHA-256 checks of normalized decoded output in the last completed public rerun.”**
- **“Across SHA-verified structured telemetry rows in the last completed public rerun, observed size reduction ranged from 23.46% to 90.85%, with a 56.32% mean reduction.”**
- **“The public benchmark set covers water metering, electricity metering, GNSS telemetry, and thermal energy metering payloads.”**
- **“Fresh host CLI benchmarks were run on macOS arm64 with 30 measured runs per row after 5 warmups.”**
- **“ZNano supports selective frame decoding within a multi-frame compressed stream without requiring every preceding frame to be reconstructed first.”**
- **“ZNano offers optional reversible payload scrambling with one byte of stream overhead; scrambling is not encryption and is not a security control.”**
- **“The optimized MCU SDK path uses fixed internal buffers and no dynamic allocation; exact runtime RAM remains target and integration dependent.”**
- **“SDK build artifacts exist for Cortex-M0/M0+, Cortex-M3, Cortex-M4 soft-float, and Cortex-M4F hard-float.”**
- **“Incompressible deterministic control payloads round-trip correctly but may expand, so strict no-expansion transport requirements need an explicit wrapper or bypass policy.”**

## Claims to avoid for now

Do **not** publish the following as unconditional product claims:

- “Lossless on every structured benchmark row.” The expanded rerun must replace the historical benchmark set before this wording is used.
- “Random access makes frames independently corruption tolerant.” Random access is an addressing capability, not an error-resilience guarantee.
- “ZNano scrambling encrypts the payload” or any presentation of scrambling as a security, privacy or compliance control.
- “ZNano provides spread spectrum”, processing gain, link-budget improvement, extended radio range, or physical-layer gain from payload scrambling.
- Any description of the scrambling seed as a cryptographic key or key space.
- “MCU RAM is exactly X bytes.” Current values are static/profile evidence, not runtime board measurements.
- “No dynamic allocation anywhere in the repository.” Historical/legacy host paths include allocations.
- “Never expands incompressible data.” The fresh control rerun shows expansion.
- “MCU throughput is X MB/s” or “MCU cycles are X.” No target cycle/timer benchmark is currently published.
- “Cortex-M4 is X times faster than Cortex-M0.” Build support exists, but per-target runtime performance is not measured in the current evidence set.
- “Encoded output is bit-identical across repeated runs.” Decoded integrity and timing variance are measured, but repeated encoded-output identity is not explicitly logged.

## Stronger claims require

Before strengthening the public language, add:

1. target-board runtime timing and cycle measurements;
2. target-board runtime RAM and peak stack measurements;
3. explicit repeated encoded-output identity logging if deterministic encoded bytes are to be claimed;
4. completion of the expanded structured rerun with all publication rows SHA-256 validated;
5. measured selective-frame decode timing and working-memory comparison;
6. measured scrambling timing impact across representative device profiles;
7. a product-level bypass/fallback policy if ZNano should guarantee that transmitted output is never larger than input.

## Publication discipline

The public repository distinguishes between:

- **measured benchmark results**;
- **calculated metrics derived from those measurements**;
- **integration-contract properties**;
- **implementation observations that apply only to a named SDK path**;
- **future measurements or claims that remain explicitly pending**.

Historical benchmark values remain labeled as historical/current-public evidence until a complete rerun replaces them. New feature documentation must not be used to imply new performance numbers before those measurements exist.
