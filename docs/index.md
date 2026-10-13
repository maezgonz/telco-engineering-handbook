# Telco Engineering Handbook

A carrier-grade telecom engineering knowledge base: **7 reference documents** distilled from **17 years in mission-critical infrastructure** (12 in mobile network operations, RAN and core, across 2G/3G/4G/5G).

Every document is standards-based (3GPP, ITU-T, ICNIRP — public standards) or anonymized methodology — **no operator, vendor or confidential material**.

The handbook pairs with the automation repository — [telco-network-automation](https://github.com/maezgonz/telco-network-automation) — where the same discipline is implemented as tested tooling.

## Start here

| If you are... | Start with |
|---|---|
| Operating networks | [RAN KPI Reference](docs/ran_kpi_reference.md) → [Alarm Correlation](docs/alarm_correlation.md) |
| Interviewing candidates | [Stress Testing & Redundancy](docs/ran_stress_testing.md) → [Alarm Correlation](docs/alarm_correlation.md) |
| Working in HPC/quant | [Core Network Evolution](docs/core_network_evolution.md) — the invariants section |

## The knowledge base

- **[RAN KPI Reference](docs/ran_kpi_reference.md)** — 3GPP-standard KPI definitions: accessibility, retainability, mobility, utilization
- **[Alarm Correlation](docs/alarm_correlation.md)** — severity taxonomy (ITU-T X.733), root-cause patterns in alarm storms, MTTR
- **[Stress Testing & Redundancy](docs/ran_stress_testing.md)** — failover, load, MTTR and soak tests methodology
- **[MML Command Reference](docs/mml_command_reference.md)** — MML-style verb grammar and patterns per domain
- **[Spectrum Refarming](docs/spectrum_refarming.md)** — pilot-cluster gating and the KPI gates that matter
- **[Core Network Evolution](docs/core_network_evolution.md)** — 2G/3G → EPC+IMS → 5G SBA, and the invariants
- **[RNI/EMF Analysis](docs/rni_emf_reference.md)** — ICNIRP-based immission analysis and hot-spot mitigation

## Author

Matias Gonzalez — [github.com/maezgonz](https://github.com/maezgonz) · Systems Engineer → HPC & Quantitative Analysis · MSc in HPC @ USC
