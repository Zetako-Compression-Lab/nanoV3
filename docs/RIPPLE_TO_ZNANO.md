# From Ripple gas-meter telemetry prototype to ZNano

ZNano did not begin as a generic compression project.

Its lineage starts with a real embedded metering problem: reducing the size of structured telemetry produced by smart gas-meter devices while preserving exact reconstruction and keeping the implementation suitable for constrained environments.

## The Ripple Metering POC

Zetako developed an early customer-specific proof of concept for **Ripple Metering**, a smart metering company working across gas, water, electricity, wireless connectivity, cloud infrastructure, and utility applications.

Reference: https://ripplemetering.com/

The original codec was designed specifically around the telemetry format and operational requirements of Ripple's gas-metering system. It was integrated into the target environment and operated successfully in the intended workflow.

This first generation was therefore deliberately **application-specific**: the data format was known, the device behavior was known, and the compression model could be engineered around that exact workload.

The result was important for a reason larger than the POC itself: it demonstrated that meaningful lossless reduction could be achieved directly on structured meter telemetry without relying on a conventional general-purpose compression library.

## From a custom codec to a general model

The research question then changed.

Instead of asking:

> How do we compress this particular meter payload efficiently?

we began asking:

> Can the same research principles be turned into a codec that is independent of one customer, one meter, and one fixed telemetry schema?

That transition became the foundation of ZNano.

The successive generations moved away from a codec tailored to one Ripple payload format toward a more **device-agnostic and telemetry-agnostic lossless model** intended for structured data produced by constrained systems.

The objective is not to claim that every possible byte stream is equally compressible. ZNano is designed around a more specific and useful domain: structured machine telemetry where fields, measurements, state, counters, timestamps, sensor values, and repeated frames create exploitable regularity.

Today the public evidence set includes multiple unrelated device classes rather than one customer-specific gas meter, including water metering, electricity metering, GNSS/positioning, and thermal-energy metering.

## Historical validation on real Ripple data

The original Ripple material was preserved and can still be used as a historical regression dataset.

A new benchmark run was performed against **only real/local meter payloads extracted from the historical Ripple encoder data**, excluding the synthetic generator datasets from the default report.

The two functioning ZHex lineage implementations both completed all five real-data round trips successfully:

| Variant | Cases | Valid round-trips | Average reduction |
| --- | ---: | ---: | ---: |
| `current_zhex_c` | 5 | **5 / 5** | **63.48%** |
| `legacy_zhex_c_ori` | 5 | **5 / 5** | **63.48%** |
| `legacy_global_c` | 5 | 0 / 5 | historical reference only |

The five real/realtime cases include short six- and seven-packet captures as well as repeated six-, twelve-, and thirteen-packet server payloads.

For the two valid implementations, measured reductions across those cases ranged from **43.75% to 83.01%**, with an average of **63.48%**, while preserving exact encode/decode round trips in every tested case.

See [`RIPPLE_ZHEX_LEGACY_BENCHMARK.md`](RIPPLE_ZHEX_LEGACY_BENCHMARK.md) for the complete historical benchmark table and methodology notes.

## What changed between then and now

The most important evolution is conceptual:

**Then — customer-specific**  
One known gas-meter payload, one integration target, one codec designed around that workload.

**Now — device-agnostic**  
A reusable embedded lossless compression technology evaluated across multiple structured telemetry domains and MCU targets.

The Ripple POC proved the practical premise. The work that followed was about removing the dependency on a single device format while retaining the properties that mattered: exact reconstruction, small footprint, predictable integration, and bandwidth reduction on structured telemetry.

That research path is what became **ZNano**.

## Public disclosure boundary

This document intentionally describes the **history, engineering objective, validation results, and product evolution** without documenting the proprietary compression mechanism.

The public repository does not disclose internal data transformations, field-selection logic, compression rules, codec state layout, or other implementation details that would enable reconstruction of the proprietary core.

## Historical note

This page documents Zetako's internal technology lineage and a past customer-specific POC. It should not be interpreted as a statement of a current commercial relationship, certification, partnership, or endorsement by Ripple Metering.
