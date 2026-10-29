# networkwalks-BO84D-week1-cybersecurity-lab-setup

# 🔐 Cybersecurity Lab Environment Setup
*Building an isolated virtual lab for penetration testing and ethical hacking practice*

## 📌 Project Overview

This project documents the setup of a virtual cybersecurity lab built with VirtualBox and Kali Linux. The lab gives me a controlled, repeatable environment for practicing reconnaissance, network scanning, vulnerability assessment, and other security-testing techniques without putting any real systems at risk.

The lab runs on a private virtual network, fully isolated from my home network and the internet. This makes it safe to run offensive tools, and it can easily grow: intentionally vulnerable machines can be added as targets, and a monitoring system can be added later to practice detection and log analysis from a SOC analyst's perspective.

All testing is performed only on machines I own and control inside this isolated environment.
## 🎯 Objectives

The main objectives of this project are to:

- Install and configure VirtualBox.
- Install or import Kali Linux as a virtual machine.
- Create a private NAT Network for the cybersecurity lab.
- Configure network connectivity for the Kali Linux VM.
- Assign a consistent IP address to the Kali VM.
- Verify network connectivity and DNS resolution.
- Take a clean VM snapshot for recovery.
- Document the complete setup process.
- Prepare the environment for future cybersecurity projects.
## 🛡️ Purpose of the Lab

The lab provides a controlled environment for cybersecurity learning and authorized security testing, kept separate from my home network.

It can be used for activities such as:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Exploitation practice
- Security-tool experimentation
⚠️ Important: This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

🏗️ Lab Architecture
<img width="1875" height="839" alt="VirtualBox Network Lab Topology" src="https://github.com/user-attachments/assets/25704f2c-78d3-4153-96e4-a109f34a86bc" />
Additional target machines can be added to the same virtual network in future projects.
## ⚙️ Lab Configuration

| 🧩 Component       | ⚙️ Configuration              |
|--------------------|-------------------------------|
| 🖥️ Host OS         | [your OS and version]         |
| 🧠 Host RAM        | [your host RAM]               |
| ⚡ Processor       | [your CPU]                    |
| 🧰 Hypervisor      | VirtualBox [your version]     |
| 🐉 Security OS     | Kali Linux [your version]     |
| 🧠 Kali RAM        | [e.g., 2048 MB]               |
| 🌐 Virtual Network | NAT Network                   |
| 📡 Network Address | 10.0.0.0/24                   |
| 🐧 Kali IP Address | 10.0.0.2/24                   |
| 🚪 Default Gateway | 10.0.0.1                      |
| 🌍 DNS Server      | 8.8.8.8                       |
| 🔮 Future VM Range | 10.0.0.3–10.0.0.99            |
# 🪜 Lab Setup Procedure

## Step 1. Install 7-Zip

7-Zip was installed to extract the Kali Linux virtual machine package, which is distributed as a `.7z` archive.

**Tool:** 7-Zip

---

## Step 2. Install VirtualBox

VirtualBox was installed as the hypervisor that runs the lab's virtual machines.

**Tool:** VirtualBox [your version]

---

## Step 3. Create the NAT Network

A dedicated NAT Network was created in VirtualBox with the following configuration:

- **Network Name:** NatNetwork
- **IPv4 Prefix:** 10.0.0.0/24
- **DHCP:** Enabled
- **IPv6:** Disabled

<img width="1914" height="985" alt="Screenshot-NATNETWORK(1)-David_Idiong" src="https://github.com/user-attachments/assets/69dc9d01-56ae-4a47-9089-6fe92340a07f" />


A NAT Network was chosen because VMs attached to the same
## Step 4. Import Kali Linux

The Kali Linux virtual machine was downloaded from the official Kali Linux website, extracted with 7-Zip, and imported into VirtualBox.

**Network adapter configuration:**
The VM network adapter was configured as follows:
Adapter 1
Attached to: NAT Network
Network: NatNetwork
Adapter Type: Intel PRO/1000 MT Desktop
The VM was allocated:

RAM: 2048 MB
<img width="1918" height="1009" alt="Screenshot-kalilinux-David_Idiong" src="https://github.com/user-attachments/assets/d0a0d236-f6fb-4f2e-9bc1-0b12d39f6fe7" />
## Step 5. Configure the Kali Linux Network

The Kali Linux network was checked and configured with a consistent IPv4 address.
Example Configuration

IP Address: 10.0.0.2

Subnet Mask: 255.255.255.0

Gateway: 10.0.0.1

DNS: 8.8.8.8


A consistent IP address makes it easier to document the lab and to reference the Kali machine in future exercises.
<img width="1917" height="1006" alt="kalilinux network settings-David_Idiong" src="https://github.com/user-attachments/assets/cd18b205-7cd2-4f95-b255-b6679c47ecf1" />

## Step 6. Create a Clean VM Snapshot

After completing the initial configuration, a VirtualBox snapshot was created.

Snapshot name: Clean Kali - Network Setup
The snapshot represents the clean baseline of the laboratory.

If a future exercise changes or damages the VM configuration, the machine can be restored to this baseline.

# 🔎 Lab Verification

| ✅ Test                       | 🧾 Command                       | 🎯 Expected Result               |
|------------------------------|---------------------------------|---------------------------------|
| 🌐 Check IP address           | `ip a`                          | Correct Kali IP displayed       |
| 📡 Test gateway               | `ping 10.0.0.1`                 | Successful replies              |
| 🌍 Test Internet connectivity | `ping 8.8.8.8`                  | Successful replies              |
| 🔎 Test DNS resolution        | `nslookup example.com`          | Domain resolves                 |
| 🧰 Verify Nmap                | `nmap --version`                | Nmap version displayed          |
| 🔄 Verify snapshot            | Restore snapshot and run `ip a` | Baseline configuration restored |

### Example Results

```
IP Address:
10.0.0.2/24

Gateway:
10.0.0.1

DNS:
8.8.8.8
```
# 🐞 Problems Encountered & Solutions

Documenting problems is an important part of the project.

Problem 1. Internet Connectivity After Static IP Configuration
After manually configuring the IPv4 settings, Internet connectivity may fail depending on the Kali/NetworkManager configuration.

One workaround used during this lab was:

sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
The network connection was then restarted/rebooted and connectivity was tested again.

Important: Network interface and connection names may differ between systems. Students should first identify their actual connection name before running an nmcli command.

## Problem 2. VirtualBox VT-x / Virtualization Error

**Symptom:** The Kali VM would not start. VirtualBox reported a hardware virtualization (VT-x) error.

**Cause:** Hardware virtualization was turned off in the computer's BIOS/UEFI settings, so VirtualBox could not use it.

**Solution:**

1. Restart the computer and open the BIOS/UEFI setup.
2. Find the virtualization setting (Intel VT-x, or AMD-V on AMD processors) and enable it.
3. Save the changes and let the computer restart.
4. Launch VirtualBox and start the Kali VM again.

**Result:** The VM booted normally once virtualization was enabled.

# 💡 What I Learned

This project taught me how to build and configure a virtual environment for cybersecurity practice. The main takeaways were:

### 1. NAT vs NAT Network

A standard VirtualBox NAT gives each VM its own isolated connection to the outside world, so VMs can't see each other. A NAT Network puts multiple VMs on one shared virtual network where they can talk to each other and still reach the internet through address translation. That makes it the right choice for a lab with both attacker and target machines.

### 2. Virtual Machine Networking

I learned how VirtualBox network adapters attach VMs to different network types, and how that choice decides which machines can communicate with each other and with the outside world.

### 3. Static IP Configuration

I practiced setting and verifying an IPv4 address, subnet mask, gateway, and DNS server in Kali Linux, and testing each one with commands like `ip a`, `ping`, and `nslookup`.

### 4. VM Snapshots

A clean snapshot gives me a known-good state to return to. I should take one before any risky or experimental work, so a mistake never means rebuilding the lab from scratch.

### 5. Documentation

Recording commands, settings, screenshots, problems, and fixes is part of doing the work properly. Good notes make a project repeatable and show how I think, which matters in security roles.

# 🔐 Security & Ethical Use

This lab is built for education and skills practice only.

- All testing is done on virtual machines that I own and control, inside this private lab network.
- I will not use these tools or techniques against any system, network, or service without the owner's explicit written permission.
- Intentionally vulnerable machines added later will be kept inside the lab and never exposed to my home network or the public internet.
- Anything I learn here is for building defensive and analytical skills, in line with legal and professional ethical standards.

- # 🔗 Tools & Resources

- **7-Zip:** [https://7-zip.org/download.html](https://7-zip.org/download.html)
- **VirtualBox:** [https://virtualbox.org/wiki/Downloads](https://virtualbox.org/wiki/Downloads)
- **Kali Linux:** [https://kali.org/get-kali](https://kali.org/get-kali)

- # 👤 Author

**David Idiong**
Aspiring SOC Analyst | Cybersecurity Training  |Intern at networkwalks|BO84D

LinkedIn: [https://www.linkedin.com/in/david-idiong-71476a326](https://www.linkedin.com/in/david-idiong-71476a326)

## 📌 Project Information

**Program:** Cybersecurity at Networkwalks | **Project:** Cybersecurity & Pentesting Lab Setup | **Platform:** VirtualBox + Kali Linux | **Repository:** GitHub
