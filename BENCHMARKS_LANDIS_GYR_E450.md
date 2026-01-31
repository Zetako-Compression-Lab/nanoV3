# NanoV3 Benchmark – LANDIS+GYR E450 Payload

## Overview

This document presents benchmark results obtained using **real LANDIS+GYR E450 smart meter payloads**.

The objective of these benchmarks is **engineering evaluation**, focusing on functional correctness, determinism, and integration cost, rather than synthetic compression performance.

All measurements were produced using the NanoV3 benchmark tool in **decode-all stream mode**.

---

## Payload Description

- **Device type:** LANDIS+GYR E450
- **Domain:** Smart electricity metering
- **Format:** ASCII hexadecimal telemetry stream
- **Structure:** Multiple concatenated payload frames
- **Processing mode:** Full stream (decode-all)

The tested payload reflects **production-like meter data**, representative of real device transmissions.

---

## Test Environment

### Host System
- Platform: macOS (Apple Silicon)
- Execution mode: NanoV3 CLI
- Benchmark tool: `nanov3_bench.py`
- Timing: multiple iterations with warmup
- Memory measurement: RSS peak via `/usr/bin/time`

### NanoV3 Configuration
- Encode mode: `e`
- Decode mode: `d 0` (decode all payloads)
- Payload size: size of one E450 frame (hex characters)
- Input: concatenated payload stream

### Methodology
- One functional encode/decode pass (integrity check)
- Multiple benchmark runs (warmup excluded)
- SHA256 computed on normalized hexadecimal output
- RSS baseline measured to isolate OS/runtime overhead

---

## Functional Integrity

Results (from `nanov3_bench_report_LANDIS_GYR_E450.txt`):

SHA256 match (normalized hex): Yes

yaml
Copy code

### Interpretation

- Encode → decode reconstructs the **entire original stream**
- No data loss, corruption, or reordering detected
- Correct handling of concatenated payload frames

This confirms NanoV3 is **lossless and deterministic** on LANDIS+GYR E450 payloads.

---

## Performance Results

Performance measurements are taken from repeated runs with warmup excluded.

### Encode
- Mean execution time: see report
- Variance: low and stable
- No execution spikes observed

### Decode
- Mean execution time: see report
- Variance: low and stable
- Encode and decode costs are comparable

### Interpretation

- Runtime behavior is **deterministic**
- CPU cost is predictable and symmetric
- Suitable for embedded and infrastructure use cases where timing must be budgeted

---

## Memory Usage (RSS Analysis)

Observed values (see report):

- Baseline RSS (OS/runtime)
- Encode RSS peak
- Decode RSS peak
- Estimated NanoV3 core RSS delta (upper bound)

### Interpretation

- RSS includes OS loader and runtime overhead
- The RSS delta provides a **conservative upper bound** for NanoV3’s core memory usage
- Memory footprint is stable and independent of stream length

### MCU-Oriented Interpretation

- No dynamic allocation
- Fixed internal buffers
- Deterministic stack usage

**Expected MCU RAM usage:**  
> Order of magnitude: **tens of KB (bounded)**  
> Exact value depends on target MCU and caller-provided buffers

---

## Compression Results

Compression measurements (from report):

- Compression ratio: see report
- Size reduction: meaningful reduction on structured E450 telemetry

### Interpretation

- Effective reduction on real structured data
- No entropy coding
- No training phase
- No external dictionary

This reflects **structure-aware compression**, not archival compression.

---

## What These Benchmarks Demonstrate

- Lossless encode/decode on real electricity metering data
- Safe handling of concatenated payload streams
- Deterministic runtime behavior
- Predictable and bounded memory usage
- Practical compression gains on structured telemetry

---

## What These Benchmarks Do Not Claim

- Absolute MCU cycle counts
- Maximum theoretical throughput
- Direct comparison with gzip/zstd

Cycle-accurate performance must be measured on target hardware.

---

## Reproducibility

- Payload: LANDIS+GYR E450 (production format)
- Tool: `nanov3_bench.py`
- Decode mode: `d 0` (decode all)
- Full results: `nanov3_bench_report_LANDIS_GYR_E450.txt`

---

## Summary

NanoV3 demonstrates **deterministic, lossless compression** on real LANDIS+GYR E450 meter payloads, with predictable CPU cost, bounded memory usage, and effective size reduction.

These results support **safe embedded and infrastructure integration evaluation**.
