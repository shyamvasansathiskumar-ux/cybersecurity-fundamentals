# Networking lab — subnetting a /24 into equal /27s — 2026-09-04

## What I was trying to understand

Why splitting a network into N equal subnets costs you exactly `2 × (number of subnets)` usable host addresses compared to one flat network — not a vague "subnetting wastes addresses," but the actual count, for a concrete case: one /24 split into departments.

## Setup

| Field | Value |
|---|---|
| Tools | Python `ipaddress` module, used to check hand-worked math, not to do it for me |
| Environment | Worked on paper first, then verified |

## What I did

Scenario: `10.0.0.0/24` (254 usable addresses flat) needs to be split into subnets for up to 8 departments, each with at most 30 hosts.

By hand: 30 hosts needed → need at least 5 host bits (2⁵ − 2 = 30 usable). That leaves `32 − 24 − 5 = 3` bits for subnetting the original /24, giving `2³ = 8` subnets, each a /27 (24 + 5 = 27, wait — /24 network bits + 3 subnet bits + 5 host bits = 32, so mask is /27).

Checked against `ipaddress`:

```python
import ipaddress
net = ipaddress.ip_network('10.0.0.0/24')
for s in net.subnets(new_prefix=27):
    hosts = list(s.hosts())
    print(s, hosts[0], '-', hosts[-1], f'({len(hosts)} hosts)', 'broadcast:', s.broadcast_address)
```

```
10.0.0.0/27    10.0.0.1   - 10.0.0.30    (30 hosts)   broadcast: 10.0.0.31
10.0.0.32/27   10.0.0.33  - 10.0.0.62    (30 hosts)   broadcast: 10.0.0.63
10.0.0.64/27   10.0.0.65  - 10.0.0.94    (30 hosts)   broadcast: 10.0.0.95
10.0.0.96/27   10.0.0.97  - 10.0.0.126   (30 hosts)   broadcast: 10.0.0.127
10.0.0.128/27  10.0.0.129 - 10.0.0.158   (30 hosts)   broadcast: 10.0.0.159
10.0.0.160/27  10.0.0.161 - 10.0.0.190   (30 hosts)   broadcast: 10.0.0.191
10.0.0.192/27  10.0.0.193 - 10.0.0.222   (30 hosts)   broadcast: 10.0.0.223
10.0.0.224/27  10.0.0.225 - 10.0.0.254   (30 hosts)   broadcast: 10.0.0.255
```

8 subnets of 30 usable hosts each = 240 usable addresses, out of 254 the flat /24 would have offered. The other 14 are gone: 2 per subnet (network + broadcast address) × 8 subnets — minus the 2 the flat network would have lost anyway. Net cost of subnetting here: exactly 14 addresses, for the ability to isolate 8 broadcast domains.

## What I got wrong first

I initially assumed the "wasted" addresses scaled with the size of each subnet, not the count of subnets. They don't — a /27 loses exactly 2 addresses (network + broadcast) regardless of whether you cut a /24 into two /25s or thirty-two /29s. The cost of subnetting is a flat 2 addresses per subnet you create, not a percentage of the address space.

## What this maps to in a real system

This is the actual tradeoff a network design has to state explicitly: more, smaller subnets buy you smaller broadcast domains and cleaner segmentation between departments (finance's traffic never broadcasts into engineering's segment) at a fixed, countable address cost. Where I'd expect this to matter for a real system: a flat /24 with everything on one broadcast domain means one compromised host can ARP-spoof or broadcast-flood every other host on the network; splitting departments into their own /27s (plus VLANs and inter-VLAN ACLs) means a compromise in one segment doesn't get free Layer 2 visibility into the others. The 14-address cost above is the price of that isolation, and it's a number a network design document should actually state, not hand-wave.

## Concepts to go read about

- [ ] VLSM (variable-length subnet masking) — this exercise used equal-size subnets; real designs rarely need equal sizes
- [ ] How VLANs enforce the broadcast-domain boundary that subnetting only implies on paper
