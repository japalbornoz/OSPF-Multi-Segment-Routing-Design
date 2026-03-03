# Failure Test 3 – R5-WAN Ethernet Uplink to Internal OSPF Segment Down

## Objective
Simulate loss of the WAN-edge router from the internal Ethernet OSPF segment and observe the effect on default-route origination while keeping internal site routing under review.

---

## Failure Action
Shutdown performed on:
- **R5-WAN FastEthernet0/0**

### Command Used
- `interface fa0/0`
- `shutdown`

---

## Expected Outcome
- OSPF adjacency between R5-WAN and the internal Ethernet segment drops
- internal routers lose the OSPF external default route
- site-to-site internal OSPF routing between HQ and Branch may remain functional if R2-HQ and R4-BRANCH still share the Ethernet segment
- WAN-edge reachability is lost

---

## Verification Commands
- `show ip ospf neighbor`
- `show ip route`
- verify removal of `O*E2 0.0.0.0/0` on R1-HQ, R2-HQ, R3-BRANCH, and R4-BRANCH

---

## Expected Observations
- R5-WAN is no longer present in OSPF neighbor tables of R2-HQ and R4-BRANCH
- internal routers lose the external default route
- internal site LAN routes may still remain if R2-HQ and R4-BRANCH adjacency survives over the Ethernet segment

---

## Actual Result
The failure was successfully reproduced by shutting down `FastEthernet0/0` on R5-WAN.

Observed results:
- R5-WAN interface state changed to administratively down/down
- OSPF adjacencies between R5-WAN and both R2-HQ and R4-BRANCH dropped
- R2-HQ no longer displayed R5-WAN as an OSPF neighbor, but retained adjacencies with R1-HQ and R4-BRANCH
- R4-BRANCH no longer displayed R5-WAN as an OSPF neighbor, but retained adjacencies with R2-HQ and R3-BRANCH
- internal routers lost the external default route
- `Gateway of last resort is not set` was observed on internal routers after default-route withdrawal
- internal HQ-to-Branch routing remained operational through the remaining OSPF topology
- PC-HQ could still reach PC-BRANCH
- PC-BRANCH could still reach PC-HQ
- external reachability tests to `8.8.8.8` failed from both HQ and Branch hosts
- ping failures returned `Destination host unreachable` from the local default gateways, confirming loss of default-route forwarding

---

## Conclusion
Failure Test 3 behaved as expected.

Shutting down the WAN-edge router’s Ethernet interface removed R5-WAN from the OSPF broadcast segment and caused the external default route to be withdrawn from the internal routing domain. Internal OSPF site-to-site communication between HQ and Branch remained functional because the remaining routers stayed connected through Area 0. This confirmed that internal reachability and WAN-edge default-route origination are separate dependencies in the topology.

---

## Evidence (Screenshots)
- ![Failure Test 3 - R5 Ethernet Down](../screenshots/failure-test-3-link-down.png)
  
- ![Failure Test 3 - R2 Neighbor Loss and Default Route Removed](../screenshots/failure-test-3-r2-neighbor-loss-and-default-route-removed.png)
  
- ![Failure Test 3 - R4 Neighbor Loss and Default Route Removed](../screenshots/failure-test-3-r4-neighbor-loss-and-default-route-removed.png)

- ![Failure Test 3 - PC-HQ to Branch Still Works but 8.8.8.8 Fails](../screenshots/failure-test-3-pc-hq-internal-ok-external-fail.png)
  
- ![Failure Test 3 - PC-BRANCH to HQ Still Works but 8.8.8.8 Fails](../screenshots/failure-test-3-pc-branch-internal-ok-external-fail.png)
