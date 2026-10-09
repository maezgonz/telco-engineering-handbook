# Core Network Evolution — 2G/3G → IMS/VoLTE → 5G SBA

> Anonymized architecture reference on core network evolution, distilled from hands-on core engineering across three generations of mobile systems. No operator or vendor data — the architecture is 3GPP standard.

## The three generations at a glance

| Generation | Core architecture | Voice | Key elements |
|---|---|---|---|
| **2G/3G (CS/PS)** | Circuit-switched + Packet-switched | CS calls (TDM) | MSC, MGW, HLR, SGSN, GGSN |
| **4G (EPC)** | All-IP packet core | CSFB / VoLTE (IMS) | MME, SGW, PGW, HSS, IMS |
| **5G (5GC)** | Service-Based Architecture (SBA) | VoNR / VoLTE (IMS) | AMF, SMF, UPF, NRF, HSS+ |

## 1. The circuit-switched era (2G/3G)

Voice rides a dedicated circuit-switched path: MSC (switching) + MGW (media) + HLR/HSS (subscriber data). Strengths: deterministic, rock-solid QoS. Cost: dedicated resources, expensive scaling — and the reason the transition to all-IP was inevitable.

**Engineering reality of the CS era:** the MSC/MGW pairs are the availability backbone — redundancy patterns (N+1, geographic redundancy) and ATM-to-IP transport migrations dominated the operational work. Subscriber data integrity (HLR/HSS) is the most critical asset in the network.

## 2. The all-IP transition (EPC + IMS)

4G's EPC is all-IP: no circuit domain at all. Voice migrates to **VoLTE** — IMS (IP Multimedia Subsystem) delivers voice as a data service with carrier-grade QoS:

- **Circuit Switched Fallback (CSFB)** — the interim: 4G phones drop to 2G/3G for calls
- **VoLTE (IMS)** — the destination: voice as QCI-1 bearer, SRVCC for continuity to CS when coverage demands

**Engineering reality:** the ATM→IP migration in MGWs (transport modernization) and the IMS deployment (CSCF, HSS/SLF, MTAS, SBG elements) are the core milestones. VoLTE QoS depends on end-to-end bearer management — voice quality now spans RAN + transport + core, and the troubleshooting surface grows accordingly.

## 3. The service-based era (5GC)

5G Core re-architects everything into **Service-Based Architecture (SBA)**: network functions as software services on a common bus (HTTP/2 APIs), discovered via NRF (Network Repository Function):

| Function | Role (3GPP) |
|---|---|
| **AMF** | Access & Mobility — registration, connection, mobility management |
| **SMF** | Session Management — PDU sessions, QoS flows |
| **UPF** | User Plane — data forwarding (the "MGW" of the SBA era) |
| **NRF** | Service discovery — the SBA's DNS |
| **PCF/UDM** | Policy / Unified Data Management (the HSS successor) |

**Engineering reality:** the migration from EPC to 5GC is a virtualization/cloud transformation — network functions (AMF/SMF/UPF) run as VNF/CNF in cloud environments. The operational skill set shifts toward cloud-native operations: orchestration, scaling, container networking — while the availability discipline (99.9%) and the subscriber-data integrity mindset remain exactly the same.

## 4. The invariant across all three generations

Technology changes; the engineering constants do not:

1. **Subscriber data integrity** (HLR → HSS → UDM) is the most critical asset — its loss is a catastrophic event
2. **Availability targets** (99.9%+) drive every redundancy and maintenance decision
3. **Voice is the hardest service** — it has the strictest latency/jitter budgets in every generation (CS TDM → VoLTE QCI-1 → VoNR 5QI-1)
4. **Signaling is the silent killer** — protocol faults (MAP, Diameter, HTTP/2 SBA) degrade service without user-visible symptoms until they don't

## 5. Related content

- [ran_kpi_reference.md](ran_kpi_reference.md) — the KPI layer that validates each generation
- [mml_command_reference.md](mml_command_reference.md) — the configuration management patterns
- [alarm_correlation.md](alarm_correlation.md) — the operational discipline that keeps every generation up
