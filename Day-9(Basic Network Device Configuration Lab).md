
## Lab Objective
This lab teaches fundamental device configuration in a Cisco network, including setting hostnames, assigning IP addresses, manually configuring speed and duplex on interfaces connected to other network devices, adding interface descriptions for documentation, and disabling unused interfaces to enhance security and reduce unnecessary resource usage.

## Lab Topology
The network is a small LAN with a single router connected to two switches, each switch serving two end devices (PCs). All devices operate within a single subnet for basic connectivity testing.
- Total devices: 1 router (R1), 2 switches (SW1, SW2), 4 PCs (PC1, PC2, PC3, PC4).
- Connections:
  - R1 GigabitEthernet0/0 connected to SW1 GigabitEthernet0/1.
  - R1 GigabitEthernet0/2 connected to SW2 GigabitEthernet0/1 (inferred from configurations, though diagram shows G0/2 on SW1 as down).
  - SW1 FastEthernet0/1 connected to PC1 (IP ending .0.1).
  - SW1 FastEthernet0/2 connected to PC2 (IP ending .0.2).
  - SW2 FastEthernet0/1 connected to PC3 (IP ending .0.3).
  - SW2 FastEthernet0/2 connected to PC4 (IP ending .0.4).
- Subnet: 172.16.0.0/16 across the network.
The topology diagram (provided in the first screenshot) shows R1 in the center with links to SW1 and SW2, and PCs attached to switches, labeled with partial IPs and the overall subnet.

## Device & Configuration Table

| Device Name | Interface            | IP Address       | Subnet Mask     | Default Gateway    |
|-------------|----------------------|------------------|-----------------|--------------------|
| R1         | GigabitEthernet0/0  | 172.16.255.254  | 255.255.0.0    | N/A               |
| R1         | GigabitEthernet0/1  | N/A (unused)    | N/A            | N/A               |
| R1         | GigabitEthernet0/2  | N/A (to SW2)    | N/A            | N/A               |
| SW1        | GigabitEthernet0/1  | N/A             | N/A            | N/A               |
| SW1        | GigabitEthernet0/2  | N/A             | N/A            | N/A               |
| SW1        | FastEthernet0/1     | N/A             | N/A            | N/A               |
| SW1        | FastEthernet0/2     | N/A             | N/A            | N/A               |
| SW1        | FastEthernet0/3-24  | N/A (unused)    | N/A            | N/A               |
| SW2        | GigabitEthernet0/1  | N/A             | N/A            | N/A               |
| SW2        | FastEthernet0/1     | N/A             | N/A            | N/A               |
| SW2        | FastEthernet0/2     | N/A             | N/A            | N/A               |
| SW2        | FastEthernet0/3-24  | N/A (unused)    | N/A            | N/A               |
| PC1        | Fa0                 | 172.16.0.1      | 255.255.0.0    | 172.16.255.254    |
| PC2        | Fa0                 | 172.16.0.2      | 255.255.0.0    | 172.16.255.254    |
| PC3        | Fa0                 | 172.16.0.3      | 255.255.0.0    | 172.16.255.254    |
| PC4        | Fa0                 | 172.16.0.4      | 255.255.0.0    | 172.16.255.254    |

Note: IPs for PCs are inferred from topology labels (e.g., .0.1 for PC1). Switches are Layer 2 and do not require IPs. R1's G0/2 IP is not configured in commands but assumed similar if needed; only G0/0 is IP-assigned.

## Step-by-Step Configuration

### Router (R1) Configuration
Enter privileged mode and global configuration to set the hostname, configure the active interface (G0/0) with IP, speed, duplex, and description, then label and prepare unused interfaces. This ensures the router is identifiable, the link to SW1 is optimized, and unused ports are documented.

```
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# interface g0/0
R1(config-if)# do show ip interface brief
```

(Output shows initial down/unassigned state.)

```
R1(config-if)# ip address 172.16.255.254 255.255.0.0
R1(config-if)# speed 1000
R1(config-if)# duplex full
R1(config-if)# description ## to SW1 ##
R1(config-if)# no shutdown
```

(Link changes to up.)

```
R1(config-if)# do show ip interface brief
```

(Output shows G0/0 up with IP.)

```
R1(config-if)# interface range g0/1-2
R1(config-if-range)# description ## not in use ##
R1(config-if-range)# do show running-config
```

(Displays current config.)

```
R1(config-if-range)# end
R1# copy running-config startup-config
```

(Saves config; confirm with [OK].)

Note: Unused interfaces (G0/1-2) are described but not explicitly disabled (shutdown) in the provided commands. In a full lab, add `shutdown` for security.

### Switch (SW1) Configuration
Set hostname, verify interface status, manually set speed/duplex on Gigabit links (to networking devices), add descriptions to active and unused ports, and attempt to disable unused FastEthernet ports. This optimizes links and secures the switch.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW1
SW1(config)# do show interface status
```

(Output lists port statuses.)

```
SW1(config)# interface g0/1
SW1(config-if)# speed 1000
SW1(config-if)# duplex full
```

(Link up.)

```
SW1(config-if)# interface g0/2
SW1(config-if)# speed 1000
SW1(config-if)# duplex full
```

(Link down, possibly unused.)

```
SW1(config-if)# interface range f0/1-2
SW1(config-if-range)# description ## to end host ##
SW1(config-if-range)# interface range f0/3-24
SW1(config-if-range)# description ## not in use ##
SW1(config-if-range)# shutdown
```

(Note: Typo in input as "shut down" – correct is `shutdown`. This would disable ports if executed properly.)

```
SW1(config-if-range)# end
SW1# write memory
```

([OK] – saves config.)

### Switch (SW2) Configuration
Similar to SW1: Set hostname, configure Gigabit link to R1, descriptions on PC ports and unused, with attempts to disable unused ports.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW2
SW2(config)# do show interface status
```

(Output lists statuses.)

```
SW2(config)# interface g0/1
SW2(config-if)# speed 1000
SW2(config-if)# duplex full
```

(Link up.)

```
SW2(config-if)# interface range f0/1-2
SW2(config-if-range)# description ## to end host ##
SW2(config-if-range)# interface range f0/3-24
SW2(config-if-range)# description ## not in use ##
SW2(config-if-range)# shutdown
```

(Multiple typo attempts; correct command is `shutdown`.)

```
SW2(config-if-range)# end
SW2# write memory
```

([OK].)

## Results & Screenshots
<img width="690" height="602" alt="image" src="https://github.com/user-attachments/assets/e16200f9-a600-4eb8-ba21-57d16aa2af29" />
