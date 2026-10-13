# Simulated Command Outputs — What an Operator Sees

> Simulated command outputs with **fictional site IDs and masked values** — showing what a network operator actually sees when executing commands on RAN/core elements and transport switches. Format mirrors industry tooling; every ID, IP and value below is invented.

Fictional naming convention used throughout: `SITE-DEMO-xxx`, `BSC-DEMO-x`, `switch-demo-xx`, RFC 1918 documentation IPs (10.200.0.0/16).

## 1. RAN — cell inventory query (MML-style)

```text
+++    SITE-DEMO-001                2026-10-09 15:30:12
O&M    #987654

RETCODE = 0  Operation succeeded

LST CELLDATA: ;

LOCAL CELL ID  CELL NAME   FREQ BAND   EARFCN  PCI  CELL STATE
1              DEMO-A-1    LTE B3      1300    101  ACTIVE
2              DEMO-A-2    LTE B3      1650    102  ACTIVE
3              DEMO-A-3    LTE B1      100     103  BLOCKED

(Number of results = 3)
```

**How to read it:** two active cells on band 3 (1800 MHz) with different EARFCNs (two carriers), one blocked cell on band 1 — a blocked state in the inventory is the first thing to triage: is it administratively blocked (planned) or fault-blocked?

## 2. RAN — board state query

```text
+++    SITE-DEMO-001                2026-10-09 15:32:45
O&M    #987655

RETCODE = 0  Operation succeeded

DSP BRD: ;

SLOT  BOARD TYPE      STATUS    SOFT VERSION    HW VERSION
0     MAIN CONTROL    NORMAL    V100R020C10     V100R020
1     BASEBAND        NORMAL    V100R020C10     V100R020
2     TRANSCEIVER     NORMAL    V100R020C10     V100R020
3     POWER           NORMAL    ---             V100R020
4     (EMPTY)         ---       ---             ---
```

**How to read it:** slot 0 is the main control unit, slot 1 baseband, slot 2 transceiver, slot 3 power — a healthy site. Any `FAULT`/`OFFLINE` state here correlates with the alarm storm (see [alarm_correlation.md](alarm_correlation.md)) — the board-state query is the first diagnostic step for cell-down cascades.

## 3. Transport switch — interface inventory (Cisco IOS-style)

```text
switch-demo-01#show ip interface brief
Interface              IP-Address      OK? Method Status       Protocol
GigabitEthernet0/1     10.200.1.2      YES manual up           up
GigabitEthernet0/2     unassigned      YES unset  down         down
GigabitEthernet0/3     10.200.1.3      YES manual up           up
Vlan100                10.200.1.1      YES manual up           up
Vlan200                10.200.2.1      YES manual up           up
```

**How to read it:** two uplinks/uplinks (Gi0/1, Gi0/3), one unused port, and the two VLAN interfaces (OAM + service) — the Status/Protocol pair is the transport health check: `up/up` = operational, `up/down` = carrier issue, `down/down` = unused or unplugged.

## 4. Transport switch — VLAN inventory

```text
switch-demo-01#show vlan brief

VLAN Name             Status    Ports
---- ---------------- --------- -------------------------------
100  OAM              active    Gi0/1, Gi0/2, Gi0/3
200  SERVICE-DATA     active    Gi0/3, Gi0/4
300  SERVICE-VOICE    active    Gi0/4
```

**How to read it:** the traffic separation pattern — OAM (management), service data and service voice on separate VLANs; Gi0/3 carries both data and voice (trunk).

## 5. Transport switch — spanning-tree state

```text
switch-demo-01#show spanning-tree

VLAN0100
  Spanning tree enabled protocol ieee
  Root ID    Priority    4096
             Address     0000.0c11.1100
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/1               Desg FWD 4         128.1    P2p
Gi0/2               Desg FWD 4         128.2    P2p
```

**How to read it:** this bridge is the root for VLAN 100 — in a ring/topology with redundancy, the root placement and port roles (Desg/Altn/Blk) decide the failover path — the classic loop-prevention layer that keeps planta interna stable.

## 6. Core — subscriber data integrity query (MML-style)

```text
+++    BSC-DEMO-A                   2026-10-09 15:40:01
O&M    #987700

RETCODE = 0  Operation succeeded

LST HLRDATA: MSISDN="5491100000001";

MSISDN            IMSI               STATE     SUBSCRIBER PROFILE
5491100000001     722310000000001    ACTIVE    VOICE+DATA
```

**How to read it:** the subscriber-data integrity check — STATE=ACTIVE with the expected profile. The HLR/HSS is the most critical asset in the network: a subscriber not found / wrong profile here means service-wide failure, which is why core procedures start and end with integrity queries (masked above — real IMSI/MSISDN never appear in documentation).

## Why simulated outputs matter

The gap between "knowing the command" and "reading the output" is where operational expertise lives: a candidate who can triage a BLOCKED cell state, a `up/down` interface or a missing root bridge in seconds has operated real networks. These simulations are the training data for that skill — with zero real infrastructure exposed.
