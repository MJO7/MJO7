# Megh Joshi

CS at UCLA, class of 2029. Software lead on the [UCLA Rocket Project](https://github.com/UCLA-Rocket-Project) and flight software engineer on the [Bruin Spacecraft Group](https://github.com/Bruin-Spacecraft-Group)'s RAPID CubeSat.

Most of what I build is systems software that has to keep working when something under it fails: a storage engine that gets killed mid-write, a telemetry pipeline that sees duplicate and malformed packets, a radio link that drops during a launch. I write the prediction down before I measure, and I keep the result when it disagrees with me.

Looking for **Summer 2027** internships in software engineering, systems and infrastructure, and quant development.

## Selected work

| Project | What it is | The result I'd point to |
|---|---|---|
| [**Chronoscale**](https://github.com/MJO7/Chronoscale) | Columnar storage engine for exchange tick data, C++20 | Predicted the as-of join crossover from operation counts. It landed 15 to 28× lower, because binary search pays cache misses the model can't see ([ADR 0004](https://github.com/MJO7/Chronoscale/tree/main/docs/adr)) |
| [**Mission Telemetry Platform**](https://github.com/MJO7/mission-telemetry-platform) | Multi-tenant telemetry: Go ingest → Kafka → FastAPI + TimescaleDB → React | Flight summaries from 73.4 ms to 0.33 ms (224×) with continuous aggregates; 69 tests running in CI |
| [**Winnow**](https://github.com/MJO7/winnow) | Triage for failed GitHub Actions jobs on AWS: flaky test, real regression, or infrastructure | pgvector beat Postgres full-text by 0.004 recall@5 at 11× the latency, so full-text is what shipped |
| [**Flight dashboard**](https://github.com/UCLA-Rocket-Project/Prometheus-2026/tree/main/Web_app) | React + WebSocket ground station with deterministic replay | Used live by 15+ engineers during the Mojave launch to 13,400 ft |

### Chronoscale

- WAL durability tested by killing the process: 8 named crash failpoints with predicted post-crash state, plus randomized `SIGKILL` runs across three durability modes.
- Lock-free SPSC ingest queue with ~74× lower mean handoff latency than a mutex queue, and 26M trades/sec end to end on real Binance BTCUSDT data.
- Every test runs under Release, ASan + UBSan, and TSan builds. The sanitizers caught a signed-overflow bug on an `INT64_MIN` delta before it shipped.
- VWAP accumulates in `__int128`, tested against a single trade whose `price * qty` already exceeds `INT64_MAX`.

### Mission Telemetry Platform

- Go ingest partitions to Kafka by vehicle so each stream stays ordered while consumers scale out. Duplicates are dropped, malformed packets go to a dead-letter queue, and events commit in the same transaction as the data.
- 42× fewer buffer pages read per history query from a composite index, measured with query plans committed to the repo.
- An Ansible playbook takes a bare Ubuntu host to a running k3s cluster with 7 services; CI stands up a throwaway cluster and smoke-tests the stack on every run.

### Where things lost

I keep these in the READMEs on purpose. Chronoscale's timestamp, price, and quantity codecs lose to zstd on real data (the id column wins by 8×), and the mechanism is written up next to the numbers. Winnow's flake detector scores zero on a first corpus with only 14 flakes; the diagnosis and the fix are in its docs.

## Experience

**Software Lead, UCLA Rocket Project** (2025 to present). Flight software for a hybrid rocket that flew to 13,400 ft, a two-way LoRa link between rocket and ground, and a 12-person team across four coordinated boards.

**Software Engineer, Bruin Spacecraft Group** (2025 to present). Fixed-rate ADCS control loop for the RAPID CubeSat on an STM32H743, debugged at the register level.

## Right now

- Running [astro-stack-cuda](https://github.com/MJO7/astro-stack-cuda)'s kernels on real GPU hardware so the roofline analysis gets measured numbers instead of placeholders.
- Growing Winnow's corpus so flake detection has more than 14 examples to learn from.

**Stack:** C++20, C, Python, Go, TypeScript, SQL · PostgreSQL / TimescaleDB, Kafka, Redis · AWS, Terraform, Kubernetes, Docker · STM32, LoRa, real-time control

[megh07joshi@g.ucla.edu](mailto:megh07joshi@g.ucla.edu) · [LinkedIn](https://linkedin.com/in/meghjoshi)
