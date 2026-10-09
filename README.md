# Telco Engineering Handbook

![License](https://img.shields.io/badge/License-MIT-blue)
![Docs](https://img.shields.io/badge/Docs-7%20references-informational)

## Objective

A carrier-grade telecom engineering knowledge base: **7 reference documents** distilled from **17 years in mission-critical infrastructure** (12 in mobile network operations, RAN and core, across 2G/3G/4G/5G). Every document is standards-based (3GPP, ITU-T, ICNIRP — public standards) or anonymized methodology — **no operator, vendor or confidential material**.

The handbook pairs with the automation repository — [telco-network-automation](https://github.com/maezgonz/telco-network-automation) — where the same discipline is implemented as tested tooling (config drift detection, fleet probing, MOP/ATP templates).

## The knowledge base

### Quality & KPIs

| Document | Contents |
|---|---|
| **[ran_kpi_reference.md](docs/ran_kpi_reference.md)** | 3GPP-standard KPI definitions: accessibility, retainability, mobility, utilization — formulas, industry targets and engineering drivers |

### Operations & reliability

| Document | Contents |
|---|---|
| **[alarm_correlation.md](docs/alarm_correlation.md)** | Alarm severity taxonomy (ITU-T X.733), lifecycle, root-cause correlation patterns in alarm storms, and the path from reactive alarms to proactive KPIs |
| **[ran_stress_testing.md](docs/ran_stress_testing.md)** | Controller stress testing & redundancy validation: failover, load, MTTR and soak tests, with the lessons only field experience teaches |

### Configuration & spectrum

| Document | Contents |
|---|---|
| **[mml_command_reference.md](docs/mml_command_reference.md)** | MML-style verb grammar and generic command patterns per domain (RAN, transport, core) |
| **[spectrum_refarming.md](docs/spectrum_refarming.md)** | Refarming methodology: pilot-cluster gating, the KPI gates that matter, and the silent-neighbor-relation trap |

### Architecture & regulation

| Document | Contents |
|---|---|
| **[core_network_evolution.md](docs/core_network_evolution.md)** | 2G/3G CS → 4G EPC+IMS → 5G SBA: the three generations, their engineering realities, and the invariants that never change |
| **[rni_emf_reference.md](docs/rni_emf_reference.md)** | Non-ionizing radiation (EMF) immission analysis: ICNIRP reference levels, measurement methodology, hot-spot mitigation |

## How to read this handbook

- **If you operate networks**: start with the KPI reference → alarm correlation → configuration management patterns
- **If you interview candidates**: the stress-testing methodology and the alarm-correlation patterns show how the author reasons about root causes and availability
- **If you work in HPC/quant**: the invariants section of the core evolution document — availability discipline and subscriber-data integrity — is the mindset that transfers directly to low-latency systems

## Companion repository

**[telco-network-automation](https://github.com/maezgonz/telco-network-automation)** — the automation layer: config drift detection, parallel fleet probing and carrier-grade MOP/ATP templates, with unit tests and CI.

## License

[MIT](LICENSE) — Matias Gonzalez
