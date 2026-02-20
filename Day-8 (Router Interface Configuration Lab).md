# Basic Router Interface Configuration Lab

## Lab Objective
This lab teaches the basics of configuring IP addresses on router interfaces, enabling them, and verifying their status. It also covers testing connectivity between end devices connected to the router via switches, demonstrating fundamental network setup and troubleshooting skills in a simple routed environment.

## Lab Topology
The network is a star topology centered on a single router (R1), with three separate LAN segments each connected via a switch to a PC. This design allows the router to act as the gateway for each subnet, enabling communication between the PCs.
- Total devices: 1 router, 3 switches (2960-24TT), 3 PCs.
- Connections:
  - R1 GigabitEthernet0/0 connected to SW1 (subnet 15.0.0.0/8), which connects to PC1.
  - R1 GigabitEthernet0/1 connected to SW2 (subnet 182.98.0.0/16), which connects to PC2.
  - R1 GigabitEthernet0/2 connected to SW3 (subnet 201.191.20.0/24), which connects to PC3.
The topology diagram (shown in the first screenshot of the lab input) illustrates the router in the center with branches to each switch and PC, including labeled IP addresses and subnets.

## Device & Configuration Table

| Device Name | Interface          | IP Address       | Subnet Mask     | Default Gateway    |
|-------------|--------------------|------------------|-----------------|--------------------|
| R1         | GigabitEthernet0/0 | 15.255.255.254  | 255.0.0.0      | N/A               |
| R1         | GigabitEthernet0/1 | 182.98.255.254  | 255.255.0.0    | N/A               |
| R1         | GigabitEthernet0/2 | 201.191.20.254  | 255.255.255.0  | N/A               |
| PC1        | Fa0               | 15.0.0.1        | 255.0.0.0      | 15.255.255.254    |
| PC2        | Fa0               | 182.98.0.1      | 255.255.0.0    | 182.98.255.254    |
| PC3        | Fa0               | 201.191.20.1    | 255.255.255.0  | 201.191.20.254    |
| SW1        | N/A               | N/A             | N/A            | N/A               |
| SW2        | N/A               | N/A             | N/A            | N/A               |
| SW3        | N/A               | N/A             | N/A            | N/A               |

Note: PC IP addresses are inferred from the topology diagram labels. Switches are Layer 2 and do not require IP configuration.

## Step-by-Step Configuration

### Router (R1) Configuration
Enter privileged EXEC mode and global configuration mode. Verify initial interface status, set the hostname for identification, configure IP addresses on each GigabitEthernet interface, enable them with 'no shutdown', and exit to verify changes. This sets up the router as the default gateway for connected subnets.

```
Router> enable
Router# configure terminal
Router(config)# do show ip interface brief
```

(Output shows all interfaces unassigned and down.)

```
Router(config)# hostname R1
R1(config)# interface gigabitethernet0/0
R1(config-if)# ip address 15.255.255.254 255.0.0.0
R1(config-if)# no shutdown
```

(Link and line protocol come up.)

```
R1(config-if)# interface gigabitethernet0/1
R1(config-if)# ip address 182.98.255.254 255.255.0.0
R1(config-if)# no shutdown
```

(Link and line protocol come up.)

```
R1(config-if)# interface gigabitethernet0/2
R1(config-if)# ip address 201.191.20.254 255.255.255.0
R1(config-if)# no shutdown
```

(Link and line protocol come up.)

```
R1(config-if)# end
R1# show ip interface brief
```

(Output now shows configured IPs with up/up status.)

```
R1# write memory
```

Note: The 'do' keyword allows running show commands from config mode. 'No shutdown' brings interfaces up. Save the config to persist changes after reload.

### PC Configuration
Configure IP addresses, subnet masks, and default gateways on PC1, PC2, and PC3 using the Packet Tracer GUI (as noted in the lab instructions: "Watch the video to learn how to do this in Packet Tracer"). No CLI is needed for PCs; use the desktop configuration menu to assign static IPs matching the table.

## Results & Screenshots
<img width="1898" height="627" alt="image" src="https://github.com/user-attachments/assets/f140b92f-b0c2-4476-a0b1-c5b24be5858c" />

<img width="1811" height="895" alt="image" src="https://github.com/user-attachments/assets/38ac1333-86f4-4428-baff-7779079a979c" />
