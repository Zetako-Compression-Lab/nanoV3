# NanoV3 Benchmark – SHARKY-775-159 Payload

## Overview

This document presents benchmark results obtained using **real SHARKY-775-159 device payloads**.

The purpose of these benchmarks is **engineering and integration evaluation**, with emphasis on functional correctness, determinism, and predictable resource usage rather than synthetic compression performance.

All measurements were produced using the NanoV3 benchmark tool in **decode-all stream mode**.

---

## Payload Description

- **Device type:** SHARKY-775-159
- **Domain:** Thermal energy / heat metering
- **Format:** ASCII hexadecimal telemetry stream
- **Structure:** Multiple concatenated payload frames
- **Processing mode:** Full stream (decode-all)

The tested payload reflects **production-like SHARKY heat meter telemetry**, representative of real device transmissions.

---

## Test Environment

### Host System
- Platform: macOS (Apple Silicon)
- Execution mode: NanoV3 CLI
- Benchmark tool: `nanov3_bench.py`
- Timing method: multiple iterations with warmup excluded
- Memory measurement: RSS peak via `/usr/bin/time`

### NanoV3 Configuration
- Encode mode: `e`
- Decode mode: `d 0` (decode all payloads)
- Payload size: size of one SHARKY-775-159 frame (hex characters)
- Input: concatenated payload stream

### Methodology
- One functional encode/decode pass (integrity validation)
- Multiple benchmark iterations
- SHA256 computed on normalized hexadecimal output
- RSS baseline measured to isolate OS/runtime overhead

---

## Functional Integrity

Results (from `nanov3_bench_report_Sharky-775-159.txt`):

SHA256 match (normalized hex): Yes

yaml
Copy code

### Interpretation

- Encode → decode reconstructs the **entire original stream**
- No data loss, corruption, or reordering
- Correct handling of concatenated heat-meter payload frames

This confirms NanoV3 is **lossless and deterministic** on SHARKY-775-159 payloads.

---

## Performance Results

Performance measurements are extracted from repeated runs with warmup excluded.

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
- Suitable for thermal metering and embedded systems with strict timing constraints

---

## Memory Usage (RSS Analysis)

Observed values (see report):

- Baseline RSS (OS/runtime overhead)
- Encode RSS peak
- Decode RSS peak
- Estimated NanoV3 core RSS delta (upper bound)

### Interpretation

- RSS includes OS loader, runtime, and CLI overhead
- RSS delta provides a **conservative upper bound** for NanoV3 core memory usage
- Memory footprint is stable and independent of stream length

### MCU-Oriented Interpretation

- No dynamic memory allocation
- Fixed internal buffers
- Deterministic stack usage

**Expected MCU RAM usage:**  
> Order of magnitude: **tens of KB (bounded)**  
> Exact value depends on target MCU and caller-provided buffers

---

## Compression Results

Compression measurements (from report):

- Compression ratio: see report
- Size reduction: meaningful reduction on structured thermal metering telemetry

### Interpretation

- Effective reduction on real SHARKY heat-meter payloads
- No entropy coding
- No training phase
- No external dictionary

This reflects **structure-aware compression**, not archival compression.

---

## What These Benchmarks Demonstrate

- Lossless encode/decode on real thermal metering data
- Safe handling of concatenated telemetry streams
- Deterministic runtime behavior
- Predictable and bounded memory usage
- Practical compression gains on structured payloads

---

## What These Benchmarks Do Not Claim

- Absolute MCU cycle counts
- Maximum theoretical throughput
- Direct comparison with gzip/zstd

Cycle-accurate performance must be measured on target hardware.

---

## Reproducibility

- Payload: SHARKY-775-159 (production format)
- Tool: `nanov3_bench.py`
- Decode mode: `d 0` (decode all)
- Full results: `nanov3_bench_report_Sharky-775-159.txt`

---

## Summary

NanoV3 demonstrates **deterministic, lossless compression** on real SHARKY-775-159 thermal energy meter payloads, with predictable CPU cost, bounded memory usage, and effective size reduction.

These results support **safe embedded and infrastructure integration evaluation** for heat-meter telemetry workloads.

