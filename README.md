# Troubleshooting #35: Re-imaged Workstation Can't Reach the Gateway

_Level: Easy — BuildTheNet 3D Sandbox troubleshooting — IP addressing_

**Status: Verified.** Completed on 2026-11-15; every requirement was machine-checked against the network built in BuildTheNet and passed.

Snapshot taken 2026-11-15. Built in a simulation — no physical or production equipment was involved.

## The ticket

**Reported symptom:** ARI-PC can't reach its gateway since it was rebuilt from an old image.

Helpdesk Ticket #135: A workstation rebuilt from an old image cannot reach the Arista gateway ARI-GW. The user insists "the IP was fine before".

_Area: IP addressing_

## Required resolution

Re-address ARI-PC into the segment it is connected to so it reaches ARI-GW, without changing the router.

## Checks

| # | Check | Status | Checked by |
|---|---|---|---|
| 1 | ARI-PC to ARI-GW works | Passed | Sandbox check |
| 2 | ARI-GW keeps its documented addressing | Passed | Sandbox check |

## Root cause

ARI-PC holds 10.20.0.5/24 on a segment addressed 10.10.0.0/24.

## The fix

What was changed to resolve the ticket, compared with the network as it was reported:

Compared with the starting topology, the learner changed the configuration of 1 device.

**ARI-PC**

```diff
  interface FastEthernet0
-  ip address 10.20.0.5 255.255.255.0
+  ip address 10.10.0.5 255.255.255.0
```

## Result

Every check passed on this snapshot:

> ARI-PC is back in its own segment and reaches ARI-GW.

## Topology

![Topology diagram](topology.svg)

## Addressing

| Device | Interface | Address | Mask | Mode | Gateway |
|---|---|---|---|---|---|
| ARI-GW | Ethernet1 | 10.10.0.1/24 | 255.255.255.0 | routed |  |
| ARI-PC | FastEthernet0 | 10.10.0.5/24 | 255.255.255.0 | VLAN 1 | 10.10.0.1 |

## Device configurations

Running-configs as `show running-config` prints them in the BuildTheNet sandbox. Passwords, secrets, keys and SNMP communities are replaced with `<removed>`.

- [ARI-SW1](configs/ARI-SW1.txt)
- [ARI-GW](configs/ARI-GW.txt)

---

Built on [BuildTheNet](https://www.buildthenet.io/sandbox)
