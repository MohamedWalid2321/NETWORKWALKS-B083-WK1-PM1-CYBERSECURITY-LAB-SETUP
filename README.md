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

<img width="1280" height="758" alt="image" src="https://github.com/user-attachments/assets/a06c6126-88f7-4334-8372-c270f7259ada" />


---

## ⚙️ Lab Specifications

| Item                    | Configuration             |
| ----------------------- | ------------------------- |
| Host Operating System   | `Win10`                   |
| Virtualization Platform | `VirtualBox`              |
| Security VM             | Kali Linux                |
| Kali RAM                | `2GB`                     |
| CPU Allocation          | `2 vCPU`                  |
| Virtual Network         | NAT Network               |
| Network Range           | `10.0.0.0/24`             |
| Kali IP                 | `10.0.0.2`                |
| Gateway                 | `10.0.0.1`                |
| DNS                     | `8.8.8.8`                 |


---

# 🔧 Installation & Configuration

## 1. Preparing the Virtualization Environment

The first step was preparing the host system for virtualization.

VirtualBox was installed and configured as the platform responsible for creating and managing the virtual machines.


---

## 2. Creating the Virtual Network

A dedicated NAT Network was configured for the cybersecurity environment.

Using a separate NAT Network allows the virtual machines to communicate with each other while maintaining network isolation from the physical LAN.

### Network Configuration

```text
Network Name : NATNetwork
IPv4 Prefix  : 10.0.0.0/24
DHCP         : Enabled
IPv6         : Disabled
```

<img width="1280" height="762" alt="image" src="https://github.com/user-attachments/assets/6c3651bc-5ba3-42d4-ba54-0c14c73e70db" />


---

## 3. Preparing the Kali Linux Machine

Kali Linux was added as the primary security-testing machine.

The virtual machine was configured with the required hardware resources and connected to the previously created NAT Network.

### Virtual Machine Settings

```text
Memory       : 2GB
Processors   : 2 vCPU
Network      : NAT Network
Adapter      : Intel PRO/1000 MT Desktop
```

<img width="784" height="518" alt="image" src="https://github.com/user-attachments/assets/858c6bea-8c24-4a54-b149-9f29a657d8e6" />
<img width="786" height="519" alt="image" src="https://github.com/user-attachments/assets/64191821-bd8e-4c88-a61e-fb00f0946a12" />



<img width="1280" height="760" alt="image" src="https://github.com/user-attachments/assets/9bdce755-68fa-4a1b-8a82-bff4a7a619af" />


---

## 4. Kali Linux Network Configuration

After starting Kali Linux, the network configuration was checked to ensure that the virtual machine received the expected addressing information.
We will assign a Manual IP to the interface
<img width="1280" height="759" alt="image" src="https://github.com/user-attachments/assets/24b44167-b8ce-42d0-95d1-ea5c2ba983ea" />


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
IP Address : 10.0.0.2
Subnet     : /24 or 255.255.255.0
Gateway    : 10.0.0.1
DNS        : 8.8.8.8
```

<img width="1280" height="761" alt="image" src="https://github.com/user-attachments/assets/2f8267b4-19dd-4ee1-aa26-d2d353b0ce71" />


---

## 5. Testing Network Connectivity

Once the network configuration was complete, connectivity was tested progressively.

### Gateway Test

```bash
ping -c 4 10.0.0.1
```

This verifies communication between Kali and the virtual network gateway.

<img width="1280" height="761" alt="image" src="https://github.com/user-attachments/assets/5fbd17fc-4991-4ffb-9c73-1de6832d494a" />


### Internet Connectivity

```bash
ping -c 4 8.8.8.8
```

This checks whether the virtual machine can reach an external IP address.

<img width="1280" height="760" alt="image" src="https://github.com/user-attachments/assets/2dbcc8b4-d7af-46f0-9468-cac7a25fa452" />


### DNS Resolution

```bash
nslookup google.com
```

or:

```bash
dig google.com
```

This confirms that DNS resolution is functioning correctly.

<img width="1280" height="762" alt="image" src="https://github.com/user-attachments/assets/7bb5e8f1-28b5-4b7b-9ee6-7f81f3fce282" />


---


# 💾 Creating a Recovery Point

After completing the initial configuration, a clean snapshot of the Kali virtual machine was created.

The snapshot provides a known-good starting point that can be restored if a future experiment modifies or damages the VM.

### Recommended Snapshot

```text
Name: Clean-Lab-Baseline
```

<img width="1280" height="761" alt="image" src="https://github.com/user-attachments/assets/cbf4c322-3299-4dd2-85f5-791f641e6e37" />


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

---

## Issue 2: Internet Connectivity After Static IP Configuration

After manually configuring the IPv4 settings, Internet connectivity may stop working depending on the Kali Linux and NetworkManager configuration.

During the lab, the following command was used as a workaround:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

After applying the configuration, the network connection was restarted and the system was rebooted if necessary.

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

