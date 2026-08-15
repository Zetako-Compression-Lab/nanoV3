# ZNano

**Lossless compression for embedded devices, telemetry, and constrained infrastructure.**

ZNano is Zetako's proprietary compression technology designed for structured device payloads where deterministic behavior, bounded resource usage, and byte-for-byte recovery matter more than headline benchmark numbers.

> This repository is a public technical showcase. The ZNano core implementation, release binaries, and proprietary source code are maintained privately.

## What ZNano is built for

ZNano targets environments such as:

- smart metering and utility telemetry
- industrial and embedded devices
- GNSS and positioning payloads
- edge infrastructure
- constrained links and low-bandwidth transports

The design goal is simple: reduce structured payload size while preserving exact reconstruction and predictable runtime behavior.

## Engineering properties

- **Lossless** — decoded output is verified against the original payload.
- **Deterministic** — the same input produces the same encoded representation and the same decoded output.
- **Stream-safe** — supports concatenated multi-frame payloads.
- **No training phase** — no model training is required before deployment.
- **No external dictionary dependency** — integration does not rely on remotely managed dictionaries.
- **Bounded implementation model** — designed for predictable memory use and embedded integration.

## Public benchmark set

The public repository currently documents validation on four production-like device families:

| Device | Domain | Payload | Result |
|---|---|---|---|
| HYDRUS-F06-006-WATER | Water metering | Structured telemetry | ~31% reduction |
| LANDIS+GYR E450 | Electricity metering | Structured telemetry | See report |
| NEO-M8-FW3 | GNSS / positioning | GNSS telemetry | See report |
| SHARKY-775-159 | Thermal energy metering | Heat-meter telemetry | See report |

Detailed reports:

- [HYDRUS-F06-006-WATER](BENCHMARKS_HYDRUS_F06_006_WATER.md)
- [LANDIS+GYR E450](BENCHMARKS_LANDIS_GYR_E450.md)
- [NEO-M8-FW3](BENCHMARKS_NEO_M8_FW3.md)
- [SHARKY-775-159](BENCHMARKS_SHARKY_775_159.md)

## Benchmark methodology

Across the published device tests, the methodology focuses on integration safety rather than synthetic compression contests:

1. use production-like structured payloads;
2. encode the complete stream;
3. decode the complete stream;
4. verify normalized output integrity with SHA-256;
5. measure repeated runtime after warm-up;
6. inspect peak memory behavior and baseline overhead;
7. report limitations alongside positive results.

The published tests intentionally do **not** claim cycle-accurate MCU performance. Final cycle counts and memory requirements must be validated on the intended target hardware.

## Example: HYDRUS validation

The HYDRUS-F06-006-WATER benchmark demonstrates:

- complete lossless stream reconstruction;
- SHA-256 integrity match;
- approximately 31% size reduction on the tested payload;
- low runtime variance in the host benchmark environment;
- bounded memory behavior in the tested implementation.

See the [full HYDRUS report](BENCHMARKS_HYDRUS_F06_006_WATER.md) for methodology and interpretation.

## Integration model

ZNano is intended to sit close to the source of structured data:

```text
Device / Sensor
      |
      v
+-------------+
| ZNano Encode|
+-------------+
      |
      v
Transport / Storage
      |
      v
+-------------+
| ZNano Decode|
+-------------+
      |
      v
Original Payload
```

The codec core is proprietary. Customer and evaluation releases are distributed separately from this public repository.

## What is public vs. private

**Public here**

- benchmark methodology
- benchmark reports
- integration-oriented technical documentation
- product behavior and engineering claims that can be externally reviewed

**Private**

- compression core source code
- release binaries
- internal test harnesses and proprietary datasets
- customer-specific integrations
- release engineering and CI/CD

## Product naming

**ZNano** is the current product name. Earlier internal and benchmark material may refer to **NanoV3**; those references describe the same technology lineage.

## Commercial evaluation

For technical evaluation, licensing, or integration discussions:

**Zetako S.à r.l.**  
Luxembourg  
https://zetako.ai/  
contact@zetako.ai

---

© Zetako. Proprietary technology. Public documentation in this repository does not grant rights to reproduce, reverse engineer, or redistribute the ZNano implementation.