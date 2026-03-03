# OSPF Neighbor Adjacency Troubleshooting Checklist

## Purpose
This note summarizes the checks I use when OSPF neighbors do not form correctly or when an existing adjacency fails unexpectedly.

The goal is to troubleshoot OSPF systematically by checking interface state, OSPF enablement, neighbor status, and protocol parameters before making changes.

---

## First Checks

### 1. Check interface status
Verify that the interfaces expected to form OSPF adjacency are operational.

**Command:**
- `show ip interface brief`

**What to verify:**
- interface is not administratively down
- interface protocol is up
- correct IP address is assigned
- both sides are using the correct subnet

**Common issue examples:**
- interface shutdown
- wrong IP address
- wrong subnet mask
- serial clocking issue
- cabling/module issue in Packet Tracer

---

### 2. Check OSPF neighbor state
Confirm whether a neighbor exists and what state it is in.

**Command:**
- `show ip ospf neighbor`

**What to verify:**
- expected neighbor appears
- neighbor state is correct
- dead timer is counting normally

**Neighbor state clues:**
- **No neighbor shown** → interface, subnet, passive interface, or OSPF enablement issue
- **Init** → one-way hello issue
- **2-Way** → normal on some Ethernet multiaccess segments for DROTHER relationships, but not FULL adjacency in all cases
- **ExStart / Exchange** → possible MTU mismatch
- **Full** → adjacency healthy

---

## OSPF Configuration Checks

### 3. Confirm OSPF is enabled on the correct interfaces/networks
Make sure the interface or matching network is included in the OSPF process.

**Commands:**
- `show ip protocols`
- `show running-config | section router ospf`

**What to verify:**
- correct OSPF process is active
- correct network statements are configured
- interface subnet matches the intended network statement
- interface belongs to the correct area

**Common issue examples:**
- missing network statement
- incorrect wildcard mask
- interface not matched by OSPF configuration

---

### 4. Check area number
OSPF neighbors must be in the same area on the shared link.

**Commands:**
- `show ip protocols`
- `show running-config | section router ospf`

**What to verify:**
- both sides advertise the shared subnet into the same area

**Common issue example:**
- one side in Area 0, the other side in Area 1

---

### 5. Check passive-interface configuration
Transit interfaces must not be passive if OSPF adjacency is expected on them.

**Command:**
- `show running-config | section router ospf`

**What to verify:**
- LAN/user-facing interfaces may be passive
- transit interfaces must not be passive

**Common issue example:**
- OSPF network is advertised, but no adjacency forms because the interface is passive

---

## Parameter Matching Checks

### 6. Check subnet consistency
OSPF neighbors must be in the same IP subnet on the shared link.

**Commands:**
- `show ip interface brief`
- `show running-config | section interface`

**What to verify:**
- both ends of the link use matching subnet and mask
- point-to-point links use valid /30 or intended mask
- Ethernet segment devices share the same subnet

---

### 7. Check hello and dead timers
OSPF neighbors must use matching hello/dead intervals on the shared link.

**Commands:**
- `show ip ospf interface`
- `show running-config | section interface`

**What to verify:**
- hello interval matches
- dead interval matches

**Common issue example:**
- one side uses default timer values and the other side uses custom timers

---

### 8. Check authentication settings
If OSPF authentication is configured, both sides must match.

**Commands:**
- `show running-config | section interface`
- `show ip ospf interface`

**What to verify:**
- authentication is enabled on both ends if required
- authentication type matches
- authentication key matches

**Common issue example:**
- wrong OSPF authentication key on one router

---

### 9. Check MTU settings
MTU mismatches can prevent adjacency from reaching FULL state.

**Commands:**
- `show ip ospf interface`
- `show interface`

**What to verify:**
- MTU matches across the shared link

**Common clue:**
- neighbor stuck in **ExStart** or **Exchange**

---

### 10. Check OSPF network type
OSPF network type should match expected behavior on the link.

**Commands:**
- `show ip ospf interface`
- `show running-config | section interface`

**What to verify:**
- point-to-point links use expected point-to-point behavior
- Ethernet links use broadcast unless intentionally changed
- network type mismatch is not causing adjacency issues

**Common issue example:**
- one side uses point-to-point while the other uses broadcast or non-broadcast unexpectedly

---

## Router ID Checks

### 11. Check router ID uniqueness
Router IDs should be unique within the OSPF domain.

**Commands:**
- `show ip protocols`
- `show ip ospf neighbor`

**What to verify:**
- each router has a unique router ID

**Common issue example:**
- duplicate router ID causing unexpected OSPF behavior

---

## Useful Command Summary

### Core Commands
- `show ip interface brief`
- `show ip ospf neighbor`
- `show ip protocols`
- `show running-config | section router ospf`
- `show running-config | section interface`
- `show ip ospf interface`
- `show ip route`

### If deeper verification is needed
- `show ip ospf database`
- `clear ip ospf process` *(use with caution in lab environments only)*

---

## Practical Troubleshooting Flow

1. Check whether the interface is up/up
2. Check whether the neighbor exists
3. Check the neighbor state
4. Confirm the interface is included in OSPF
5. Confirm area number matches
6. Confirm passive-interface is not blocking adjacency
7. Confirm subnet/mask match
8. Confirm timers match
9. Confirm authentication matches
10. Confirm MTU and network type match
11. Confirm router IDs are unique
