# MML-Style Command Reference — Generic Patterns per Domain

> Generic command patterns for MML-style (Man-Machine Language) configuration management across RAN and core domains. Distilled from years of hands-on configuration work with vendor LMT/OSS tools. All syntax is generic — vendor manuals define the exact proprietary syntax per release.

## The universal MML verbs

Every MML-style interface follows the same verb grammar — learn the grammar once, and any vendor's tool becomes readable:

| Verb | Meaning | Risk level |
|---|---|---|
| `LST` (list) | Query/display state — read only | None |
| `DSP` (display) | Real-time status of units/boards | None |
| `SET` | Modify an existing parameter | Medium — changes behavior |
| `MOD` | Modify object properties | Medium |
| `ADD` | Create a new object (cell, route, neighbor) | Medium-High |
| `DEL` | Delete an object | High — service impact |
| `ACT` | Activate (bring into service) | High — traffic impact |
| `DEA` | Deactivate (take out of service) | High — traffic impact |
| `BLK` / `UBP` | Block / unblock a unit (maintenance) | Medium |

## The golden rule: LST before everything

Every procedure starts and ends with read-only queries:

```text
LST <object>          ← record the BEFORE state (archive to the ticket)
... changes ...
LST <object>          ← verify the AFTER state matches intent
DSP <unit>            ← real-time confirmation of hardware state
```

A change without a captured before/after pair is unverifiable — and unverifiable changes are how silent KPI degradation happens.

## Patterns per domain

### RAN — cells and neighbor relations

```text
ADD CELL / ADD ECELL / ADD UCELL    ← 2G / LTE / UMTS cell creation
ADD NR: SRC=<cell>, DST=<neighbor>  ← neighbor relation for mobility
ACT CELL / DEA CELL                 ← traffic-affecting activation
```
Neighbor relations are the mobility backbone: a missing relation silently degrades handover success (see the KPI reference) without any alarm — this is where proactive KPI analysis beats reactive alarms.

### Transport / IP

```text
ADD IPINTERFACE: IPADDR=..., MASK=..., VLAN=...
ADD OAMROUTE: DSTIP=..., NEXTHOP=...
DSP ETHPORT: IFIDX=...              ← link state, errors
```

Transport faults cascade into full cell-down storms — the transport layer is the most common root cause in alarm correlation.

### Core (Circuit/Packet/IMS)

```text
LST HLR / LST HSS                   ← subscriber data integrity checks
ADD MGW / SET MGW: ...              ← media gateway configuration
ACT <unit>                          ← bring core network elements into service
```

Core changes carry the widest blast radius — subscriber-affecting from day one; every core MOP needs a rollback plan and a maintenance window.

## Configuration management workflow

1. **Backup** the running config before every change (and verify it is restorable)
2. **Change** with the smallest blast radius that achieves the goal
3. **Verify** with read-only queries (before/after pairs)
4. **Drift-check** afterwards — config drift detection (see the automation repository) catches silent regressions
5. **Archive** the before/after pair in the change ticket

## Why MML literacy matters

Vendor GUIs come and go (LMT, CME, OSS versions) — the underlying object model (cells, neighbors, routes, units) is stable across decades of telecom. Engineers who think in the object model and the verb grammar adapt to any vendor's tooling in days, not months.
