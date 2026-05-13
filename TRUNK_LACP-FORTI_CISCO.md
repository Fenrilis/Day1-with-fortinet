# FortiGate 60F and Cisco Catalyst 3560 LACP Configuration Guide

## Overview

This document provides a step-by-step procedure for configuring an LACP (802.3ad Aggregate Interface) between a FortiGate 60F firewall and a Cisco Catalyst 3560 switch.

### Hardware Topology

```text
FortiGate internal2 <--> Cisco Fa0/10
FortiGate internal3 <--> Cisco Fa0/11
FortiGate internal4 <--> Cisco Fa0/12
```

---

# FortiGate 60F Configuration

## Step 1 — Remove Interfaces from VLAN Switch

Before configuring LACP, remove the interfaces from the default internal VLAN switch.

Navigate to:

```text
Network → Interfaces
```

Edit:

```text
internal
```

Remove:

- internal2
- internal3
- internal4

Click:

```text
OK
```




---

# Step 2 — Create the Aggregate Interface

Navigate to:

```text
Network → Interfaces
```

Click:

```text
Create New → Interface
```

Configure the following:

| Setting | Value |
|---|---|
| Name | LACP_TRUNK |
| Type | 802.3ad Aggregate |
| Interface Members | internal2, internal3, internal4 |
| Role | LAN |
| Addressing Mode | Manual |
| IP/Netmask | 192.168.10.1/255.255.255.0 |
| Administrative Access | HTTPS, PING, SSH |

Click:

```text
OK
```

<img width="1186" height="804" alt="{C855C297-F0DF-4422-8C42-B13E33FBB5EC}" src="https://github.com/user-attachments/assets/9bc37845-2086-4c79-967a-5780dcf550d6" />


---

# Step 3 — Configure DHCP Server (Optional)

If FortiGate will provide DHCP services to clients connected through the Cisco switch:

Navigate to:

```text
Network → Interfaces → LACP_TRUNK
```

Enable:

```text
DHCP Server
```

Example DHCP configuration:

| Setting | Value |
|---|---|
| Start IP | 192.168.10.100 |
| End IP | 192.168.10.200 |
| Gateway | 192.168.10.1 |

---

# Step 4 — Create Firewall Policy

Navigate to:

```text
Policy & Objects → Firewall Policy
```

Create a new policy:

| Setting | Value |
|---|---|
| Incoming Interface | LACP_TRUNK |
| Outgoing Interface | wan1 |
| Source | all |
| Destination | all |
| Service | ALL |
| NAT | Enabled |

Click:

```text
OK
```

<img width="1199" height="910" alt="{24BA048E-003E-4924-900D-043856F5B3CC}" src="https://github.com/user-attachments/assets/5a7e9ebe-d9ae-48fb-96ea-3aeb49cff906" />

---

# Verify LACP on FortiGate

## GUI Verification

Navigate to:

```text
Network → Interfaces
```

Verify:

```text
LACP_TRUNK
```

shows:

- internal2
- internal3
- internal4

as active members.

---

## CLI Verification

Run:

```bash
diagnose netlink aggregate name LACP_TRUNK
```

Expected output:

```text
mode: lacp
status: up
```

---

# Recommended VLAN Design

If VLAN trunking is used on the Cisco switch, it is recommended to create VLAN interfaces on top of the aggregate interface.

Example:

```text
LACP_TRUNK
 ├── VLAN10
 ├── VLAN20
 └── VLAN30
```

This design is commonly used in enterprise environments.

---

# Troubleshooting

## Problem: Cisco Ports Showing "(w)"

Example:

```text
Fa0/10(w)
Fa0/11(w)
Fa0/12(w)
```

### Possible Causes

- FortiGate aggregate interface not configured
- Interfaces still part of VLAN switch
- Cabling mismatch
- LACP mode mismatch

---

# Physical Cabling Reference

```text
FortiGate internal2 ---> Cisco Fa0/10
FortiGate internal3 ---> Cisco Fa0/11
FortiGate internal4 ---> Cisco Fa0/12
```

---

# Final Network Diagram

```text
                +-------------------+
                |   FortiGate 60F   |
                |    LACP_TRUNK     |
                +-------------------+
                  |    |     |
                  |    |     |
           int2---+    |     +---int4
                       |
                    int3
                       |
         +--------------------------------+
         |     Cisco Catalyst 3560        |
         |        Port-Channel1           |
         +--------------------------------+
           Fa0/10  Fa0/11  Fa0/12
```
