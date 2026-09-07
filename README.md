# 🛡️ Cybersecurity Virtual Lab

## 📖 Overview

This repository documents the creation and configuration of a dedicated virtual environment for cybersecurity experimentation.

The lab is designed to provide a controlled space where security tools, networking concepts, reconnaissance techniques, and penetration-testing exercises can be practiced without affecting the host system or external networks.

The environment is built using virtualization technology and can later be expanded by adding additional machines that will act as testing targets.

---

## 🎯 Goals

The main goals of this setup were to:

* Build a dedicated cybersecurity practice environment.
* Configure a virtualized Kali Linux machine.
* Establish an isolated virtual network.
* Configure the required IP addressing and network parameters.
* Confirm communication between the virtual machine and the network.
* Verify Internet and DNS functionality.
* Create a recovery snapshot after completing the configuration.
* Establish a reliable environment for future security labs.

---

## 🧱 Environment Design

The lab currently consists of a host computer running a virtualization platform with Kali Linux as the primary security-testing machine.

```text
                    Host Computer
                         │
                    VirtualBox
                         │
                  ┌──────┴──────┐
                  │ NAT Network │
                  └──────┬──────┘
                         │
                    ┌────┴────┐
                    │   Kali  │
                    │  Linux  │
                    └─────────┘
```

Additional virtual machines can be connected to the same network later to create attacker/target scenarios.

**[SCREENSHOT: VirtualBox main window showing the Kali VM and its network configuration]**

---

## ⚙️ Lab Specifications

| Item                    | Configuration             |
| ----------------------- | ------------------------- |
| Host Operating System   | `YOUR OS`                 |
| Virtualization Platform | `YOUR VIRTUALBOX VERSION` |
| Security VM             | Kali Linux                |
| Kali RAM                | `YOUR RAM`                |
| CPU Allocation          | `YOUR CPU CORES`          |
| Virtual Network         | NAT Network               |
| Network Range           | `YOUR NETWORK/CIDR`       |
| Kali IP                 | `YOUR KALI IP`            |
| Gateway                 | `YOUR GATEWAY`            |
| DNS                     | `YOUR DNS`                |

> Replace the values above with the actual configuration of your laboratory.

---

# 🔧 Installation & Configuration

## 1. Preparing the Virtualization Environment

The first step was preparing the host system for virtualization.

VirtualBox was installed and configured as the platform responsible for creating and managing the virtual machines.

**[SCREENSHOT: VirtualBox installation or VirtualBox version/about window]**

---

## 2. Creating the Virtual Network

A dedicated NAT Network was configured for the cybersecurity environment.

Using a separate NAT Network allows the virtual machines to communicate with each other while maintaining network isolation from the physical LAN.

### Network Configuration

```text
Network Name : YOUR_NETWORK_NAME
IPv4 Prefix  : YOUR_NETWORK/CIDR
DHCP         : Enabled/Disabled
IPv6         : Enabled/Disabled
```

**[SCREENSHOT: VirtualBox NAT Network configuration showing the network name, CIDR and DHCP settings]**

---

## 3. Preparing the Kali Linux Machine

Kali Linux was added as the primary security-testing machine.

The virtual machine was configured with the required hardware resources and connected to the previously created NAT Network.

### Virtual Machine Settings

```text
Memory       : YOUR RAM
Processors   : YOUR CPU CORES
Network      : NAT Network
Adapter      : YOUR ADAPTER
```

**[SCREENSHOT: Kali VM System settings showing RAM and CPU allocation]**

**[SCREENSHOT: Kali VM Network settings showing NAT Network]**

---

## 4. Kali Linux Network Configuration

After starting Kali Linux, the network configuration was checked to ensure that the virtual machine received the expected addressing information.

The configuration can be inspected using:

```bash
ip addr
```

The routing table can also be checked with:

```bash
ip route
```

The expected configuration should contain:

```text
IP Address : YOUR_KALI_IP
Subnet     : YOUR_SUBNET
Gateway    : YOUR_GATEWAY
DNS        : YOUR_DNS
```

**[SCREENSHOT: Kali terminal showing `ip addr`]**

**[SCREENSHOT: Kali terminal showing `ip route`]**

---

## 5. Testing Network Connectivity

Once the network configuration was complete, connectivity was tested progressively.

### Gateway Test

```bash
ping -c 4 YOUR_GATEWAY
```

This verifies communication between Kali and the virtual network gateway.

**[SCREENSHOT: Successful gateway ping]**

### Internet Connectivity

```bash
ping -c 4 8.8.8.8
```

This checks whether the virtual machine can reach an external IP address.

**[SCREENSHOT: Successful Internet ping]**

### DNS Resolution

```bash
nslookup google.com
```

or:

```bash
dig google.com
```

This confirms that DNS resolution is functioning correctly.

**[SCREENSHOT: Successful DNS lookup]**

---

# 🔍 Security Tool Verification

After establishing network connectivity, the security environment was checked to ensure that the required tools were available.

For example:

```bash
nmap --version
```

Expected result:

```text
Nmap version: YOUR_VERSION
```

**[SCREENSHOT: Nmap version displayed in Kali terminal]**

Other tools can be checked in the same way as they are introduced during future labs.

---

# 💾 Creating a Recovery Point

After completing the initial configuration, a clean snapshot of the Kali virtual machine was created.

The snapshot provides a known-good starting point that can be restored if a future experiment modifies or damages the VM.

### Recommended Snapshot

```text
Name: Clean-Lab-Baseline
```

**[SCREENSHOT: VirtualBox snapshot manager showing the created snapshot]**

---

# 🧪 Verification Summary

| Test             | Command               | Purpose                    | Result    |
| ---------------- | --------------------- | -------------------------- | --------- |
| IP configuration | `ip addr`             | Verify assigned address    | ✅ Passed  |
| Routing          | `ip route`            | Verify default route       | ✅ Passed  |
| Gateway          | `ping -c 4 GATEWAY`   | Test local connectivity    | ✅ Passed  |
| Internet         | `ping -c 4 8.8.8.8`   | Test external connectivity | ✅ Passed  |
| DNS              | `nslookup google.com` | Verify name resolution     | ✅ Passed  |
| Nmap             | `nmap --version`      | Verify security tooling    | ✅ Passed  |
| Snapshot         | VirtualBox            | Verify recovery point      | ✅ Created |

---

# 🐛 Troubleshooting

## Issue 1 — No Network Connectivity

If Kali does not have network access, the first step is to inspect the interface configuration:

```bash
ip addr
```

Then verify the routing table:

```bash
ip route
```

If the default gateway is missing, the virtual network or Kali network configuration should be checked.

**[SCREENSHOT: The network problem/error before fixing it]**

**[SCREENSHOT: Correct configuration after fixing the issue]**

---

## Issue 2 — DNS Resolution Failure

If IP connectivity works but domain names cannot be resolved, DNS configuration should be inspected.

For example:

```bash
cat /etc/resolv.conf
```

A DNS server can then be configured according to the network setup.

The issue was verified by comparing:

```bash
ping -c 4 8.8.8.8
```

with:

```bash
nslookup google.com
```

**[SCREENSHOT: DNS failure]**

**[SCREENSHOT: Successful DNS resolution after the fix]**

---

## Issue 3 — Virtual Machine Startup Problem

If the VM fails to start, virtualization support should be checked on the host system.

Possible causes include:

* Hardware virtualization disabled.
* Incorrect VM configuration.
* Resource allocation problems.
* Conflicts with another virtualization platform.

**[SCREENSHOT: VirtualBox error message, if encountered]**

**[SCREENSHOT: BIOS/UEFI virtualization setting, if this was the actual solution]**

---

# 📚 Key Takeaways

This setup provided practical experience with several important cybersecurity and infrastructure concepts:

### Virtualization

Understanding how virtual machines can be used to create isolated environments for security experimentation.

### Network Segmentation

Using a dedicated virtual network provides a controlled environment where multiple machines can later communicate without directly exposing the host environment.

### Linux Networking

Working with:

```bash
ip addr
ip route
ping
nslookup
```

helped verify different layers of network connectivity.

### Troubleshooting

Rather than changing configurations randomly, network problems can be investigated step-by-step:

```text
Interface
   ↓
IP Address
   ↓
Routing
   ↓
Gateway
   ↓
Internet
   ↓
DNS
```

### Snapshots

A clean snapshot provides a reliable recovery point before performing experiments that could change the system.

---

# 🚀 Future Expansion

The current environment provides the foundation for future cybersecurity exercises.

Possible additions include:

* Windows target machine
* Additional Linux systems
* Vulnerable applications
* Web application targets
* Active Directory environment
* Network monitoring tools
* Security testing tools

The lab can therefore evolve from a single Kali machine into a complete multi-machine cybersecurity testing environment.

---

# ⚠️ Responsible Use

This environment is intended for **authorized cybersecurity training and experimentation only**.

Security testing should only be performed against systems that you own or systems for which you have explicit permission to test.

---

## 📸 Screenshot Checklist

Before publishing the repository, the following screenshots should ideally be included:

1. VirtualBox installed/version
2. VirtualBox NAT Network configuration
3. Kali VM hardware configuration
4. Kali VM network adapter configuration
5. Kali `ip addr`
6. Kali `ip route`
7. Successful gateway ping
8. Successful Internet ping
9. Successful DNS resolution
10. Nmap version
11. VirtualBox snapshot
12. Any actual troubleshooting problem you encountered
13. The configuration after fixing the problem

A clean repository structure could look like:

```text
/
├── README.md
└── screenshots/
    ├── 01-virtualbox.png
    ├── 02-nat-network.png
    ├── 03-kali-hardware.png
    ├── 04-kali-network.png
    ├── 05-ip-address.png
    ├── 06-routing.png
    ├── 07-gateway-ping.png
    ├── 08-internet-ping.png
    ├── 09-dns.png
    ├── 10-nmap.png
    ├── 11-snapshot.png
    └── 12-troubleshooting.png
```
