## OSPF Neighbor Adjacency Troubleshooting Checklist

### First command to run
- `show ip ospf neighbor`

### Supporting verification commands
- `show ip interface brief`
- `show ip protocols`
- `show running-config | section router ospf`
- `show running-config | section interface`

### Common OSPF adjacency requirements
1. Interfaces must be **up/up**
2. Interfaces must be in the **same subnet**
3. OSPF must be enabled on the correct interfaces or networks
4. Area numbers must match
5. Hello and Dead timers must match
6. Authentication settings must match
7. OSPF network type must match
8. MTU settings must match
9. Transit interfaces must not be configured as passive if adjacency is expected
10. Router IDs should be unique

### Neighbor-state clue
- **Down / no neighbor** → interface, subnet, OSPF enablement, passive interface, or area mismatch
- **Init** → one-way hello issue
- **ExStart / Exchange** → possible MTU mismatch
- **Full** → adjacency healthy
