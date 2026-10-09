# Spectrum Refarming — Methodology

> Anonymized methodology write-up from spectrum refarming campaigns in multi-technology mobile networks (GSM/UMTS/LTE coexistence). No operator or vendor data — the workflow is industry practice.

## Objective

Reclaim spectrum from a legacy technology (typically GSM) and hand it to a modern one (typically LTE/NR) **without degrading the service of either technology** during the transition. Refarming is a balancing act: every MHz moved from GSM to LTE improves data capacity but erodes the GSM fallback layer that carries voice and machine traffic.

## 1. The preconditions (what makes refarming safe)

- **Voice traffic migration first**: confirm CS voice (or VoLTE) traffic can be served by the surviving GSM carriers or has migrated to VoLTE — refarming the last GSM carrier under live voice traffic is the classic outage scenario
- **Machine-to-machine inventory**: M2M/IoT devices on legacy bands are slow to migrate — inventory them before planning
- **Coverage recheck**: the new LTE carrier must cover the same footprint (link budget for the new center frequency — propagation differs by band)
- **Interference analysis**: adjacent-band interference between the surviving GSM carrier and the new LTE carrier (guard bands, channel raster)

## 2. The refarming workflow

```text
baseline KPIs (both technologies)
        │
        ▼
plan target frequency plan (new LTE center + surviving GSM)
        │
        ▼
pilot cluster: refarm a small geographic cluster first
        │
        ▼
KPI validation (72h: accessibility, drop rate, HO success, throughput)
        │  ┌── KPIs degrade → rollback pilot, revisit plan
        ▼
roll out cluster by cluster (with per-cluster KPI gates)
        │
        ▼
post-rollout optimization (retune thresholds, neighbor relations)
```

### The pilot-cluster gate

Never refarm network-wide. Refarm one pilot cluster (a handful of sites), validate KPIs against baseline for 72 hours, and only then expand cluster by cluster — each cluster expansion gated on the previous one's KPIs.

### The KPI gates that matter during refarming

| KPI | Watch for | Why |
|---|---|---|
| GSM Call Setup / Drop Rate | degradation in the surviving layer | the fallback layer erodes as carriers move |
| LTE Accessibility / Throughput | improvement (the goal) | validates the new carrier performs |
| Inter-RAT HO Success (GSM↔LTE) | degradation | mobility between layers gets harder |
| Coverage complaints | increase | link budget changes with the new center frequency |

## 3. What experience teaches

- **Refarming is a KPI project, not a frequency-plan exercise** — the plan is 20% of the work; the KPI validation and rollout gating are the other 80%
- **The last GSM carrier is sacred until voice traffic is proven migrated** — machine traffic and emergency calls live there
- **Neighbor relations break silently** — after every frequency change, verify inter-RAT/inter-frequency neighbor definitions (no alarms fire for a missing relation; only KPIs and drive tests reveal it)
- **Roll back fast, expand slow** — a pilot cluster that degrades KPIs tells you the plan is wrong before it hurts the whole network

## 4. Related content

- [ran_kpi_reference.md](ran_kpi_reference.md) — the KPI definitions behind the gates
- [mml_command_reference.md](mml_command_reference.md) — the command patterns for frequency/cell changes
- [alarm_correlation.md](alarm_correlation.md) — why missing neighbors fire no alarms
