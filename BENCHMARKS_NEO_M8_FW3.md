# NanoV3 Benchmark – NEO-M8-FW3 Payload

## Overview

This document presents benchmark results obtained using **real NEO-M8-FW3 device payloads**.

The purpose of these benchmarks is **engineering and integration evaluation**, focusing on correctness, determinism, and predictable resource usage rather than synthetic compression performance.

All measurements were produced using the NanoV3 benchmark tool in **decode-all stream mode**.

---

## Payload Description

- **Device type:** NEO-M8-FW3
- **Domain:** GNSS / positioning telemetry
- **Format:** ASCII hexadecimal payload
- **Structure:** Multiple concatenated frames
- **Processing mode:** Full stream (decode-all)

The payload reflects **production-like GNSS device output**, representative of real telemetry streams emitted by the device.

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
- Payload size: size of one NEO-M8-FW3 frame (hex characters)
- Input: concatenated payload stream

### Methodology
- One functional encode/decode pass (integrity validation)
- Multiple benchmark iterations
- SHA256 computed on normalized hexadecimal output
- RSS baseline measured to isolate OS/runtime overhead

---

## Functional Integrity

Results (from `nanov3_bench_report_NEO-M8-FW3.txt`):

SHA256 match (normalized hex): Yes

yaml
Copy code

### Interpretation

- Encode → decode reconstructs the **entire original stream**
- No data loss, corruption, or reordering
- Correct handling of concatenated GNSS payload frames

This confirms NanoV3 is **lossless and deterministic** on NEO-M8-FW3 payloads.

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
- CPU cost is predictable
- Suitable for GNSS and embedded systems where timing must be controlled

---

## Memory Usage (RSS Analysis)

Observed values (see report):

- Baseline RSS (OS/runtime overhead)
- Encode RSS peak
- Decode RSS peak
- Estimated NanoV3 core RSS delta (upper bound)

### Interpretation

- RSS includes OS loader, runtime, and CLI overhead
- RSS delta provides a **conservative upper bound** on NanoV3 core memory usage
- Memory footprint remains stable and independent of stream length

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
- Size reduction: meaningful reduction on structured GNSS telemetry

### Interpretation

- Effective reduction on real GNSS payloads
- No entropy coding
- No training phase
- No external dictionary

This reflects **structure-aware compression**, not archival compression.

---

## What These Benchmarks Demonstrate

- Lossless encode/decode on real GNSS device data
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

- Payload: NEO-M8-FW3 (production format)
- Tool: `nanov3_bench.py`
- Decode mode: `d 0` (decode all)
- Full results: `nanov3_bench_report_NEO-M8-FW3.txt`

---

## Summary

NanoV3 demonstrates **deterministic, lossless compression** on real NEO-M8-FW3 GNSS payloads, with predictable CPU cost, bounded memory usage, and effective size reduction.

These results support **safe embedded and infrastructure integration evaluation** for GNSS telemetry workloads.
