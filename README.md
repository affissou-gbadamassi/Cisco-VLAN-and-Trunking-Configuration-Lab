# Cisco VLAN and Trunking Configuration Lab

## Project Overview
This hands-on Cisco Packet Tracer lab demonstrates how to create and name VLANs, assign switch access ports individually and in a range, configure an 802.1Q trunk port, and verify configuration using Cisco IOS CLI commands. It also documents command-entry errors encountered during the lab and the corrections applied.

**Platform:** Cisco Packet Tracer  
**Device:** Cisco switch (Switch1)  
**Focus:** Layer 2 switching, VLAN segmentation, access ports, trunking, and verification

## Objectives
- Create VLAN 50 for HR and VLAN 60 for MANAGEMENT.
- Configure GigabitEthernet0/1 as an access port in VLAN 50 and GigabitEthernet0/2 as an access port in VLAN 60.
- Assign FastEthernet0/2 through FastEthernet0/7 to VLAN 50 using `interface range`.
- Configure FastEthernet0/22 as a trunk and restrict allowed VLANs to 50 and 60.
- Inspect VLAN membership and the running configuration.

## Configuration Summary

| VLAN | Name | Access ports shown in verification |
|---|---|---|
| 50 | HR | Fa0/2–Fa0/7, Gi0/1 |
| 60 | MANAGEMENT | Gi0/2 |

**Trunk configured:** Fa0/22, allowed VLANs 50 and 60. The supplied screenshots establish configuration, not end-to-end trunk forwarding.

## Implementation and Evidence

### 1. Create and name VLANs
```cisco
configure terminal
vlan 50
 name HR
exit
vlan 60
 name MANAGEMENT
exit
end
show vlan
```
![VLAN 50 HR and VLAN 60 MANAGEMENT configured](screenshots/01-vlan-creation-and-naming.png)

*Figure 1 — Created VLAN 50 (HR) and VLAN 60 (MANAGEMENT), verified using `show vlan`.*

### 2. Assign an individual access port to a VLAN
```cisco
configure terminal
interface GigabitEthernet0/2
 switchport mode access
 switchport access vlan 60
exit
```
![Individual access-port configuration and command correction](screenshots/02-access-port-vlan60.png)

*Figure 2 — Configured Gi0/2 as a VLAN 60 access port; corrected an invalid command.*

### 3. Assign multiple access ports using interface range
```cisco
interface range FastEthernet0/2-7
 switchport mode access
 switchport access vlan 50
exit
```
![Range of access ports assigned to VLAN 50](screenshots/03-interface-range-vlan50.png)

*Figure 3 — Used `interface range` to assign Fa0/2 through Fa0/7 to VLAN 50 in one operation.*

### 4. Verify VLAN membership
```cisco
do show vlan
```
![VLAN membership verification](screenshots/04-vlan-membership-verification.png)

*Figure 4 — Verified VLAN 50 membership for Fa0/2–Fa0/7 and Gi0/1, and VLAN 60 membership for Gi0/2.*

### 5. Configure a trunk port
```cisco
interface FastEthernet0/22
 switchport mode trunk
 switchport trunk allowed vlan 50,60
exit
```
![Trunk port configuration](screenshots/05-trunk-configuration.png)

*Figure 5 — Configured Fa0/22 for trunking, allowing only VLANs 50 and 60.*

### 6. Confirm the trunk commands are in the running configuration
```cisco
end
show running-config
```
![Trunk running configuration](screenshots/06-trunk-running-config.png)

*Figure 6 — Running configuration confirms the trunk settings on Fa0/22.*

## Troubleshooting and Lessons Learned

| Issue observed | Cause | Correction |
|---|---|---|
| `switchport vlan 60` rejected | Incomplete/incorrect access-VLAN syntax | Used `switchport access vlan 60` |
| `interface range Gig0/2-4` rejected | Specified ports do not exist on this switch | Used valid FastEthernet range `Fa0/2-7` |
| `switchport mode trunk allowed vlan` rejected | Mixed separate `mode` and `trunk allowed` commands | Used `switchport trunk allowed vlan 50,60` |

## Results and Verification Scope
**Completed:** VLAN creation and naming; individual and range-based access VLAN assignments; trunk port and allowed-VLAN configuration; VLAN membership and running-configuration checks.

**Not yet evidenced:** Operational trunk status (`show interfaces trunk`), connectivity across two switches, same-VLAN endpoint pings, or inter-VLAN routing. These can be added in a future extension of the lab.

## Recommended Next Steps
1. Connect a second switch to the trunk and configure the far-end trunk.
2. Run `show interfaces trunk` and `show interfaces Fa0/22 switchport` and capture results.
3. Place two PCs in VLAN 50 on opposite switches, give them IPs in the same subnet, and verify ping connectivity.
4. Test VLAN isolation and document a controlled troubleshooting scenario.
5. Save the Packet Tracer `.pkt` file alongside the screenshots if available.

## Additional Evidence
- [Initial VLAN state](screenshots/07-initial-vlan-state-optional.png)
- [Access port running configuration](screenshots/08-access-port-running-config-optional.png)
