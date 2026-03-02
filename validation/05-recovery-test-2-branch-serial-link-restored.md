# Recovery Test 2 – R4-BRANCH to R3-BRANCH Serial Link Restored

## Objective
Restore the Branch-side serial link and verify OSPF reconvergence and Branch reachability restoration.

---

## Recovery Action
Interface enabled on:
- **R4-BRANCH Serial0/3/0**
or
- **R3-BRANCH Serial0/3/0**

### Command Used
- `interface s0/3/0`
- `no shutdown`

---

## Expected Outcome
- adjacency between R4-BRANCH and R3-BRANCH returns
- Branch-side learned routes are reinstalled
- external default route returns on R3-BRANCH
- Branch-to-HQ communication is restored

---

## Verification Commands
- `show ip ospf neighbor`
- `show ip route`
- ping from PC-BRANCH to PC-HQ

---

## Expected Observations
- R3-BRANCH shows FULL adjacency to R4-BRANCH
- learned routes reappear in the routing table
- `O*E2 0.0.0.0/0` returns on R3-BRANCH
- host communication succeeds again

---

## Actual Result
The recovery was successful after enabling `Serial0/3/0` on R4-BRANCH.

Observed results:
- R4-BRANCH interface state returned to up/up
- OSPF adjacency between R4-BRANCH and R3-BRANCH re-formed successfully
- R3-BRANCH displayed R4-BRANCH again as an OSPF neighbor in FULL state
- R3-BRANCH relearned remote OSPF routes, including:
  - HQ LAN `192.168.10.0/24`
  - transit networks
  - remote loopbacks
  - external default route `O*E2 0.0.0.0/0`
- R4-BRANCH also showed restored Branch and HQ reachability through OSPF
- PC-BRANCH regained reachability to PC-HQ `192.168.10.10`
- end-to-end host communication was restored successfully

---

## Conclusion
Recovery Test 2 behaved as expected.

Re-enabling the serial link between R4-BRANCH and R3-BRANCH restored the branch-side OSPF adjacency and allowed the Branch router to rejoin the OSPF domain. Remote routes, including the HQ LAN and external default route, were relearned, and end-to-end communication between Branch and HQ was restored successfully.

---

## Evidence (Screenshots)
- ![Recovery Test 2 - Link Restored](../screenshots/recovery-test-2-link-restored.png)
  
- ![Recovery Test 2 - R3 Neighbor Restored](../screenshots/recovery-test-2-r3-neighbor-and-routes-restored.png)
  
- ![Recovery Test 2 - PC-BRANCH to PC-HQ Ping Success](../screenshots/recovery-test-2-pc-branch-to-pc-hq-success.png)
