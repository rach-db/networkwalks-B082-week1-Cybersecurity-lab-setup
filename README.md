# Cybersecurity Virtual Lab Environment

*networkwalks-B082-week1-Cybersecurity-lab-setup*

A practical cybersecurity lab environment built using VirtualBox for learning and practicing cybersecurity, networking, ethical hacking, and penetration-testing concepts in a controlled virtual environment.

The lab consists of multiple virtual machines connected through a custom VirtualBox NAT Network. This allows the virtual machines to communicate with each other through a dedicated virtual network while providing a controlled environment for future cybersecurity experiments.

---

## Purpose of the Lab

The purpose of this lab is to create a controlled cybersecurity practice environment using virtual machines.

A sandboxed environment is useful for cybersecurity learning because it allows security-related experiments, network testing, and troubleshooting to be performed inside virtual machines rather than directly on the host system.

The lab uses a custom **VirtualBox NAT Network** so that the virtual machines can communicate with each other through a dedicated virtual network.

### Objectives

- Set up multiple virtual machines using VirtualBox.
- Configure a shared NAT Network.
- Configure IP addresses for the VMs.
- Test communication between all machines.
- Troubleshoot network connectivity issues.
- Take snapshots of the working VMs.

---

## Lab Environment Details

### Host Machine

| Component | Details |
|---|---|
| Host Operating System | Windows |
| RAM | 16 GB |
| GPU | NVIDIA GeForce RTX 3050 |
| Virtualization Software | Oracle VirtualBox |

### Virtual Machines

| VM | Operating System | IP Address | Purpose |
|---|---|---|---|
| VM 1 | Kali Linux | `10.0.0.2/24` | Cybersecurity testing and network analysis |
| VM 2 | Windows 10 | `10.0.0.10/24` | Windows testing environment |
| VM 3 | Android 9 | `10.0.0.9/24` | Android testing environment |

### Network Configuration

```text
Network Type:      NAT Network
Network Name:      NatNetwork
Network Range:     10.0.0.0/24
IP Range:          10.0.0.2 – 10.0.0.99
Gateway:           10.0.0.1
DNS:               8.8.8.8
```

---

## Difficulties Faced and Solutions

During the lab setup, I encountered several configuration and connectivity issues. Troubleshooting these problems helped me understand the practical aspects of virtual networking and systematic problem-solving.

### 1. Kali Linux Network Was Unreachable

**Problem**

While testing the Kali Linux network connection, the following error appeared:

```text
ping: connect: Network is unreachable
```

**Investigation**

The network interface was checked using:

```bash
nmcli device status
```

The `eth0` interface was detected but appeared as:

```text
eth0    ethernet    disconnected
```

The interface was also checked using:

```bash
ip addr show eth0
```

Although the interface was physically up, it did not have an IPv4 address assigned.

**Solution**

The NetworkManager configuration was checked and the recommended fix for Kali Linux 2026.1 or higher was applied:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

The connection was then reactivated.

**Result**

The Kali network connection was successfully established and the gateway became reachable.

**What I Learned**

An Ethernet interface being detected does not necessarily mean that the network connection is fully configured or active. NetworkManager status, IP addressing, and routing all need to be checked.

---

### 2. Kali Connection Activation Failed

**Problem**

When attempting to activate the network connection:

```bash
nmcli connection up "Wired connection 1"
```

NetworkManager returned:

```text
Connection activation failed:
IP configuration could not be reserved
```

**Investigation**

The network device status was checked:

```bash
nmcli device status
```

The interface configuration was also checked:

```bash
ip addr show eth0
```

This helped identify that the interface was available but the connection profile was not being successfully activated.

**Solution**

The NetworkManager configuration was investigated and the `ipv4.dad-timeout` setting was applied.

**Result**

The connection was successfully activated.

---

### 3. Kali Could Not Ping Windows

**Problem**

After configuring both VMs, Kali attempted to communicate with Windows:

```bash
ping -c 4 10.0.0.10
```

Initially, the result was:

```text
4 packets transmitted
0 received
100% packet loss
```

**Investigation**

The Windows IP configuration was verified using:

```powershell
ipconfig
```

Windows had the expected address `10.0.0.10`. The gateway was also reachable, indicating that the basic VirtualBox network configuration was working.

**Solution**

The Windows firewall configuration was checked and the required rule for ICMP/network communication was enabled.

**Result**

Kali was able to successfully communicate with Windows.

**What I Learned**

Correct IP configuration does not automatically guarantee connectivity between systems. Firewall rules can prevent ICMP traffic even when both systems are connected to the same network.

---

## VM Configurations

### Kali Linux

- IP: `10.0.0.2`
- Gateway: `10.0.0.1`
- DNS: `8.8.8.8`

Network configuration was checked using:

```bash
ip a
nmcli device status
ip route
```

### Windows 10

- IP: `10.0.0.10`
- Subnet Mask: `255.255.255.0`
- Gateway: `10.0.0.1`
- DNS: `8.8.8.8`

Configuration was verified using:

```powershell
ipconfig
```

### Android 9

Android-x86 9.0-r2 was installed and connected to the same NAT Network.

- IP: `10.0.0.9`
- Prefix: `/24`
- Gateway: `10.0.0.1`

---

## Tools and References

- Oracle VirtualBox
- Kali Linux
- Android-x86
- Windows 10

---

## What I Learned This Week

This lab provided practical experience with virtualization, networking, and troubleshooting.

- Learned how to create and configure VMs using VirtualBox.
- Learned the difference between NAT and NAT Network.
- Learned how multiple VMs communicate through a shared virtual network.
- Practiced configuring and verifying IP addresses.
- Used `ip`, `nmcli`, `ipconfig`, and `ping` for network troubleshooting.
- Learned how firewall settings can affect network connectivity.
- Learned how to troubleshoot a network systematically instead of changing settings randomly.
- Gained practical experience working with Kali Linux, Windows, and Android in the same virtual environment.

---

## Author

Rachel Debbarma
