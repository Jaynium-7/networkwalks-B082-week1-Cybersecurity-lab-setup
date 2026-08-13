# Week 1 — Cybersecurity & Penetration Testing Lab Setup

## Phase 1: Virtual Lab Environment and Kali Linux Network Configuration

## 1. Project Overview
This project documents the setup of a practical cybersecurity laboratory using Oracle VirtualBox and Kali Linux.

The laboratory provides a controlled environment for practicing cybersecurity, ethical hacking, and penetration-testing activities. A dedicated VirtualBox NAT Network was created to provide a defined virtual network for the laboratory environment.

The main objectives of Phase 1 were to:
- Prepare the VirtualBox environment.
- Create a dedicated NAT Network.
- Connect Kali Linux to the laboratory network.
- Configure a static IP address.
- Troubleshoot network connectivity.
- Verify the network configuration.
- Create a snapshot of the working Kali environment.

# 2. Purpose of the Lab
## Why a Sandbox?
Cybersecurity and penetration-testing activities should be performed in a controlled environment rather than directly against production systems or networks.
A virtual laboratory provides a sandbox where cybersecurity tools, configurations, and future testing activities can be practiced in a controlled environment.
Using virtual machines also makes it possible to modify, reset, and restore the laboratory environment without rebuilding the entire system from scratch.

## Why use a NAT network instead of NAT
A NAT Network was used because this laboratory contains multiple virtual machines that need to operate within the same controlled virtual network. Unlike standard NAT, where each virtual machine is placed behind its own separate NAT environment and cannot normally communicate directly with other VMs, a NAT Network allows multiple VMs to share the same virtual subnet and communicate with one another while still providing internet access through NAT.

Also, a bridged network is not used so it doesn't coincede with my host machine ip network (192.x.x.x)

For this project, a VirtualBox **NAT Network** named `Networklab` was created using the `10.0.0.0/24` network range.

The dedicated network provides a controlled foundation for future penetration-testing and cybersecurity exercises.

# 3. Lab Environment
Host Operating System ---- Windows 10 
Virtualization Platform --- Oracle VirtualBox 7.2.4 
Security Testing VM --- Kali GNU/Linux Rolling 2025.4 
Virtual Network ---- Networklab 
Network Type ---- NAT Network 
Network Range ---- `10.0.0.0/24` 
Kali Network Interface ---- `eth0` 
Kali IP Address ----- `10.0.0.2/24`
Default Gateway ----- `10.0.0.1`
DNS Server ------ `8.8.8.8` 

# 4. Network Architecture
Network:        Networklab
Network Range:  10.0.0.0/24
Interface:      eth0
IP Address:     10.0.0.2/24
Gateway:        10.0.0.1
DNS:            8.8.8.8

# 5. Phase 1 — Step-by-Step Build
5.1 VirtualBox Environment
What was done
Oracle VirtualBox was used as the virtualization platform for the cybersecurity laboratory.
The installed version was VirtualBox 7.2.4 and Kali Linux was already available as a virtual machine. The installed Kali version was verified from inside Kali using:
>>cat /etc/os-release
The system reported:
PRETTY_NAME="Kali GNU/Linux Rolling"
VERSION_ID="2025.4"
VERSION="2025.4"
VERSION_CODENAME=kali-rolling
Therefore, the actual installed operating system documented for this project is Kali GNU/Linux Rolling 2025.4

Why it was done
VirtualBox provides the virtualization environment required to run the cybersecurity laboratory machines independently from the host operating system.

5.2 Creating the Networklab NAT Network
What was done
A dedicated NAT Network named `Networklab` was created in VirtualBox. The network was configured using:
Network Range: 10.0.0.0/24
DHCP was enabled during the initial network configuration.

Why it was done
A dedicated network provides a defined network for the cybersecurity laboratory.
The 10.0.0.0/24 network gives the laboratory a specific private address range in which the virtual machines can be configured.

Screenshot

5.3 Connecting Kali Linux to `Networklab`
What was done
The Kali Linux virtual machine's network adapter was connected to `Networklab`

The adapter was configured as:
Enable Network Adapter:   Enabled
Attached to:              NAT Network
Network Name:             Networklab
Virtual Cable Connected:  Enabled

Why it was done
Kali needed to be connected to the dedicated laboratory network before its IP configuration and network connectivity could be tested.

The Virtual Cable Connected option was enabled so that the virtual network interface had an active connection.

5.4 Configuring the Kali Static IP
Initial Configuration
When Kali was first connected to the network, it received an IP address automatically through DHCP `10.0.0.3/24`

The connection was then changed to a manual IPv4 configuration.

What was done
The IPv4 connection settings were edited and configured as follows:

IPv4 Method:    Manual
IP Address:     10.0.0.2
Netmask:        /24
Gateway:        10.0.0.1
DNS Server:     8.8.8.8
Why it was done
A static IP provides Kali with a predictable address within the laboratory network.

This will make it easier to identify the Kali machine during future cybersecurity exercises.

Final Configuration
Kali IP:        10.0.0.2/24
Gateway:        10.0.0.1
DNS:            8.8.8.8

5.5 Network Troubleshooting
Problem encountered
A network connectivity problem was encountered during the Kali network configuration.

What was done
The following NetworkManager command from the supplied laboratory material was used:
`>>sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0`
After applying the configuration, the network connection was tested again.

Why it was done
The command was used to troubleshoot the network connectivity problem and allow the Kali network configuration to function correctly.


5.6 Verifying the Kali Network Configuration
After the network configuration and troubleshooting, the final configuration was verified from the Kali terminal.

IP Address Verification
The following command was executed:

`>>ip a`
The eth0 interface showed:
`inet 10.0.0.2/24`
This confirmed that Kali was using the intended static IP address.

Routing Verification
The following command was executed:
`ip route`
The routing table showed:
`default via 10.0.0.1 dev eth0 proto static metric 100`
`10.0.0.0/24 dev eth0 proto kernel scope link src 10.0.0.2 metric 100`
This confirmed that 10.0.0.1 was configured as the default gateway.

Gateway Connectivity Test
The gateway was also tested using:
`>>ping -c 4 10.0.0.1`
The result was:
4 packets transmitted
4 packets received
0% packet loss
This confirmed successful communication between Kali and the VirtualBox NAT Network gateway.

5.7 Creating the Kali Snapshot
What was done
After completing and verifying the Kali network configuration, a VirtualBox snapshot was created.

Why it was done
The snapshot provides a recovery point for the laboratory. If future cybersecurity exercises change the Kali environment or cause an unwanted configuration change, the virtual machine can be restored to this verified Phase 1 state.


# 6. Problems Faced and How They Were Solved
Network Connectivity Issue
Problem
A network connectivity problem occurred during the Kali network configuration.

Solution
The following command was used to fix the network connectivity issue
`>>sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0`

# 7. What I Learned This Week
This week's laboratory setup provided practical experience with virtual cybersecurity environments and network configuration.
I learned how to:
1.Create a custom NAT Network in VirtualBox.
2.Connect a virtual machine to a specific VirtualBox network.
3.Configure a static IPv4 address in Kali Linux.
4.The difference between a NAT and NAT network
5.Configure a default gateway and DNS server.
6.Use ip a to inspect Linux network interfaces.
7.Use ip route to inspect routing information.
8.Use ping to test network connectivity.
9.Use nmcli to troubleshoot NetworkManager configuration.
10.Create VirtualBox snapshots for recovery.

# 8. Tools and Links Used
Tools
1.Oracle VirtualBox 7.2.4
2.Kali GNU/Linux Rolling 2025.4

Lab Reference
The supplied Practical Lab Environment Setup for Pentesting, Ethical Hacking & Cybersecurity material was used as the primary guide for the Phase 1 configuration.

The material covered the VirtualBox environment, NAT Network configuration, Kali Linux setup, IP configuration, network troubleshooting, and snapshot process.

# 9. Project Demonstration Video
A short demonstration video of the completed Phase 1 laboratory setup will be included in the repository.

video/week1-lab-demo.mkv
The demonstration will show:
*VirtualBox environment
*Networklab configuration
*Kali network adapter
*Kali static IP configuration
*ip a verification
*Gateway ping

# 10. Author
Joseph Victor Ese-Osa
Role: networkwalks Cybersecurity Intern
Batch: B082

LinkedIn:


