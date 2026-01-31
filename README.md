# NanoV3 – Device Benchmarks Summary

## Purpose

This document provides a **high-level summary of NanoV3 benchmarks** performed on **real device payloads**.

The goal is to help engineers quickly understand:
- what devices were tested
- what kind of data was used
- what guarantees NanoV3 provides
- where to find detailed, reproducible results

All benchmarks focus on **integration safety and predictability**, not synthetic performance claims.

---

## Benchmark Methodology (Common to All Devices)

For every device listed below:

- Real, production-like payloads were used
- Payloads consist of **multiple concatenated frames**
- Encode → decode was performed in **decode-all stream mode (`d 0`)**
- Integrity verified via **SHA256 on normalized hex**
- Performance measured over multiple runs (warmup excluded)
- Memory usage analyzed via RSS peak and baseline delta
- No entropy coding, no training, no external dictionaries

All results were generated using the same tool:

nanov3_bench.py

yaml
Copy code

---

## Devices Tested

| Device | Domain | Payload Type | Lossless | Deterministic | Compression | Details |
|------|-------|--------------|----------|---------------|-------------|---------|
| HYDRUS-F06-006-WATER | Water metering | Structured telemetry | Yes | Yes | ~31% | [HYDRUS benchmark](BENCHMARKS_HYDRUS_F06_006_WATER.md) |
| LANDIS+GYR E450 | Electricity metering | Structured telemetry | Yes | Yes | see report | [E450 benchmark](BENCHMARKS_LANDIS_GYR_E450.md) |
| NEO-M8-FW3 | GNSS / positioning | GNSS telemetry | Yes | Yes | see report | [NEO-M8 benchmark](BENCHMARKS_NEO_M8_FW3.md) |
| SHARKY-775-159 | Thermal energy metering | Heat meter telemetry | Yes | Yes | see report | [SHARKY benchmark](BENCHMARKS_SHARKY_775_159.md) |

---

## What These Benchmarks Show

Across all tested devices, NanoV3 demonstrates:

- **Lossless encode/decode** on real device data
- Correct handling of **multi-payload streams**
- **Deterministic runtime behavior** (low variance)
- **Predictable CPU cost** (encode ≈ decode)
- **Bounded and stable memory usage**
- Practical compression gains on structured telemetry

These properties are critical for:
- embedded systems
- metering infrastructure
- constrained or real-time environments

---

## What These Benchmarks Do NOT Claim

- Absolute MCU cycle counts
- Maximum theoretical throughput
- Direct comparison with gzip / zstd
- Performance on synthetic or adversarial datasets

Cycle-accurate performance must be measured on **target hardware**.

---

## Memory Usage Note (Important)

Reported RSS values include:
- OS loader
- runtime / libc
- CLI overhead

For each benchmark, a **baseline RSS** was measured and subtracted to compute a **NanoV3 core RSS delta (upper bound)**.

**MCU interpretation:**
- No dynamic allocation
- Fixed internal buffers
- Deterministic stack usage

Expected MCU RAM usage is **order-of-magnitude tens of KB**, bounded by design.
Exact values depend on the target MCU and caller-provided buffers.

---

## Reproducibility

For each device:
- A dedicated Markdown file explains the results
- A raw `.txt` report contains the full logs

Suggested repository layout:

benchmarks/
├─ BENCHMARKS.md
├─ BENCHMARKS_HYDRUS_F06_006_WATER.md
├─ BENCHMARKS_LANDIS_GYR_E450.md
├─ BENCHMARKS_NEO_M8_FW3.md
├─ BENCHMARKS_SHARKY_775_159.md
├─ nanov3_bench_report_HYDRUS.txt
├─ nanov3_bench_report_LANDIS_GYR_E450.txt
├─ nanov3_bench_report_NEO-M8-FW3.txt
└─ nanov3_bench_report_Sharky-775-159.txt

yaml
Copy code

---

## Summary

NanoV3 has been validated on **multiple real-world metering and telemetry devices**, demonstrating:

- deterministic behavior
- lossless stream reconstruction
- bounded resource usage
- meaningful compression on structured payloads

These benchmarks are intended to support **engineering due diligence and safe system integration**.

---
