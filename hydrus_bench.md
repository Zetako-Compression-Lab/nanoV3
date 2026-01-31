# NanoV3 Benchmark – HYDRUS-F06-006-WATER Payload

## Overview

This document presents benchmark results obtained using **real HYDRUS-F06-006-WATER meter payloads**.

The goal of these benchmarks is **engineering evaluation**, not synthetic performance competition.  
They are intended to help integrators assess:

- Functional correctness (lossless encode/decode)
- Runtime determinism
- CPU cost symmetry (encode vs decode)
- Memory behavior
- Compression effectiveness on real structured telemetry data

---

## Payload Description

- **Device type:** HYDRUS-F06-006-WATER
- **Domain:** Smart water metering / telemetry
- **Format:** ASCII hexadecimal stream
- **Structure:** Multiple concatenated payload frames
- **Processing mode:** Full stream (decode-all)

The payload used in this benchmark reflects **production-like device data**, not synthetic or artificially optimized input.

---

## Test Environment

### Host System
- Platform: macOS (Apple Silicon)
- Execution mode: NanoV3 CLI
- Timing method: multiple iterations with warmup
- Memory measurement: RSS peak via `/usr/bin/time`

### NanoV3 Configuration
- Encode mode: `e`
- Decode mode: `d 0` (decode all payloads)
- Payload size: size of one single HYDRUS frame (hex characters)
- Input: concatenated payload stream

### Methodology
- One functional encode/decode pass for integrity validation
- Multiple benchmark runs (warmup excluded)
- SHA256 computed on normalized hexadecimal output
- RSS peak measured for encode and decode
- Baseline RSS measured separately to estimate OS/runtime overhead

---

## Functional Integrity



Input normalized hex length: 432
Decoded normalized hex length: 432
SHA256 match (normalized hex): Yes


### Interpretation

- Encode → decode reconstructs the **entire original stream**
- No data loss, corruption, or reordering
- Multi-payload stream handling is correct

This confirms NanoV3 is **lossless and deterministic** on this payload type.

---

## Performance Results

### Encode



Mean time: ~12.7 ms
Std dev: ~0.3 ms
Min / Max: ~12.3 ms / ~13.4 ms


### Decode



Mean time: ~12.6 ms
Std dev: ~0.3 ms
Min / Max: ~12.2 ms / ~13.1 ms


### Interpretation

- Encode and decode costs are **symmetric**
- Low variance (~2–3%)
- No execution spikes observed

This indicates **deterministic and predictable CPU cost**, suitable for real-time and embedded integration.

---

## Memory Usage (RSS Analysis)



Baseline RSS (OS/runtime): ~1.0 MB
Encode RSS peak: ~1.3 MB
Decode RSS peak: ~1.28 MB
Estimated core RSS delta: <300 KB (upper bound)


### Interpretation

- RSS includes OS loader, runtime, and libc overhead
- The *RSS delta* represents a **conservative upper bound** for NanoV3 core usage
- Memory usage remains stable and independent of stream length

### MCU-Oriented Interpretation

- No dynamic memory allocation
- Fixed internal buffers
- Deterministic stack usage

**Expected MCU RAM usage:**  
> Order of magnitude: **tens of KB (bounded)**  
> Exact value depends on target and caller-provided buffers

---

## Compression Results



Compression ratio: 0.6898
Size reduction: ~31%


### Interpretation

- ~31% reduction on real structured telemetry data
- No entropy coding
- No training phase
- No external dictionaries

This reflects **structure-aware compression**, not archival compression.

---

## What These Benchmarks Demonstrate

- Lossless encode/decode on real metering data
- Safe handling of concatenated payload streams
- Deterministic runtime behavior
- Predictable and bounded memory usage
- Meaningful compression gains on structured payloads

---

## What These Benchmarks Do Not Claim

- Absolute MCU cycle counts
- Maximum theoretical throughput
- Comparison with gzip/zstd at high compression levels

Cycle-accurate performance must be measured on target hardware.

---

## Reproducibility

- Payload: HYDRUS-F06-006-WATER (production format)
- Tool: `nanov3_bench.py`
- Decode mode: `d 0` (decode all)
- Full logs: `nanov3_bench_report.txt`

---

## Summary

NanoV3 demonstrates **deterministic, lossless compression** on real HYDRUS water-meter payloads, with predictable CPU cost, bounded memory usage, and ~31% size reduction.

These results are suitable for **embedded and infrastructure integration evaluation**.

