# cybersecurity-setup-lab-environment
I built a virtual cybersecurity lab on Windows 11 using VirtualBox and Kali Linux 2026.2. Kali runs on a private NAT Network with a static IP, so I can safely practice scanning, reconnaissance, and exploitation, and later add vulnerable target machines. The setup includes updates, connectivity checks, clean snapshot, and documented troubleshooting.
<img width="960" height="504" alt="kali-linux-setup png" src="https://github.com/user-attachments/assets/9306511b-92ec-482a-a409-0a7e126b5c94" />
<img width="960" height="504" alt="virtualbox-network-setup png" src="https://github.com/user-attachments/assets/0b12bf4e-1d07-47e4-925b-33c6bdb7fa14" />

---


## 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox and Kali Linux.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

The lab is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing.

---


## 🎯 Objectives

The main objectives of this project are to:

- Install and configure VirtualBox.
- Install/import Kali Linux as a virtual machine.
- Create a private **NAT Network** for the cybersecurity lab.
- Configure network connectivity for Kali Linux.
- Assign a consistent IP address to the Kali VM.
- Verify network connectivity and DNS resolution.
- Take a clean VM snapshot for recovery.
- Document the complete setup process.
- Prepare the environment for future cybersecurity projects.

---

## 🛡️ Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing.

It can be used for activities such as:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Exploitation practice
- Security-tool experimentation

⚠️ **Important:** This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

---

## ⚙️ Lab Configuration

| 🧩 Component          | ⚙️ Configuration      |
| --------------------- | --------------------- |
| 🖥️ Host OS            | [Windows 11]          |
| 🧠 Host RAM           | [8 G]               |
| ⚡ Processor          | [Intel Core i5]       |
| 🧰 Hypervisor         | VirtualBox [7.2]      |
| 🐉 Security VM        | Kali Linux [2026.2]   |
| 🧠 Kali RAM           | [2048 MB]             |
| 🌐 Virtual Network    | NAT Network           |
| 📡 Network Address    | 10.0.0.0/24           |
| 🐉 Kali IP            | 10.0.0.2/24           |
| 🎯 Metasploitable IP  | [10.0.0.3/24]         |
| 🚪 Default Gateway    | 10.0.0.1              |
| 🌍 DNS Server         | 8.8.8.8               |

---

# 🪜 Lab Setup Procedure

## Step 1. Install 7-Zip

I installed 7-Zip to extract the Kali Linux VM package, which comes as a `.7z` archive.

---

## Step 2. Install VirtualBox

I installed VirtualBox as the hypervisor to run all lab machines.

---

## Step 3. Create the NAT Network

I created a dedicated NAT Network in VirtualBox:

```text
Network Name: NatNetwork
IPv4 Prefix:  10.0.0.0/24
DHCP:         Enabled
IPv6:         Disabled
```

![NAT Network Settings]<img width="960" height="504" alt="virtualbox-network-setup png" src="https://github.com/user-attachments/assets/fc126f28-51ab-45fe-b4ed-192482b19eff" />

---

## Step 4. Import Kali Linux

I downloaded the official Kali Linux VirtualBox image from kali.org and imported it.

```text
Adapter 1
Attached to:  NAT Network
Network:      NatNetwork
Adapter Type: Intel PRO/1000 MT Desktop
RAM:          [2048 MB]
```
![Kali Linux Running]<img width="950" height="499" alt="importing-kali-linux" src="https://github.com/user-attachments/assets/4c814afa-e579-49e2-ac8a-dc8f279f66cc" />

---

## Step 5. Configure the Kali Network

I gave Kali a consistent IPv4 address so it is easy to reference in future exercises:

```text
IP Address:  10.0.0.2
Subnet Mask: 255.255.255.0
Gateway:     10.0.0.1
DNS:         8.8.8.8
```

![Kali Network Settings]<img width="960" height="504" alt="kali-linux-network-configuration" src="https://github.com/user-attachments/assets/89ebe573-a895-4164-986f-e9b2c47ce974" />

---

## Step 7. Create a Clean Snapshot

After finishing the setup, I took a snapshot of each VM:

```text
Clean Kali - Network Setup
```

If an exercise breaks something, I can restore the VM to this clean state.

---
# 🔎 Lab Verification

| ✅ Test                    | 🧾 Command                    | 🎯 Expected Result         |
| ------------------------- | ----------------------------- | -------------------------- |
| 🌐 Check IP address       | `ip a`                        | 10.0.0.2 shown             |
| 📡 Test gateway           | `ping 10.0.0.1`               | Successful replies         |
| 🌍 Test internet          | `ping 8.8.8.8`                | Successful replies         |
| 🔎 Test DNS               | `nslookup networkwalks.com`   | Domain resolves            |
| 🎯 Reach target           | `ping [10.0.0.3]`             | Successful replies         |
| 🧰 Verify Nmap            | `nmap --version`              | Version displayed          |
| 🔄 Verify snapshot        | Restore, then run `ip a`      | Baseline config restored   |

---

# 🐞 Problems Encountered & Solutions

## Problem 1. Kali Linux Had No Network Connection

After importing Kali Linux and attaching it to the NAT Network, the VM wasn't getting the right IP address, so it couldn't reach the gateway or the internet.

**How I found the issue:**

```bash
ip a
ping -c 4 10.0.0.1
```

The IP address shown was not the one planned for the lab, and the ping to the gateway failed.

**Solution:**

1. Checked VirtualBox **Settings → Network**: Adapter 1 was set to **NAT Network** with `NatNetwork` selected.
2. In Kali, right-clicked the network icon and opened **Edit Connections**.
3. Selected **Wired connection 1**, then opened the **IPv4 Settings** tab.
4. Changed the method from **Automatic (DHCP)** to **Manual** and added:

```text
Address: 10.0.0.2
Netmask: 24
Gateway: 10.0.0.1
DNS:     8.8.8.8
```
# 💡 What I Learned

### 1. NAT vs NAT Network
Plain NAT isolates each VM. A NAT Network lets several VMs talk to each other while still sharing internet access, which makes it the right choice for a multi-machine lab.

### 2. VM Networking
How VirtualBox network adapters connect VMs, and how adapter settings decide which machines can see each other.

### 3. IP Configuration
Setting and checking IP addresses, subnet masks, gateways and DNS in Linux.

### 4. Snapshots
Taking a clean snapshot **before** risky work gives me a safe point to roll back to.

### 5. Documentation
Recording commands, screenshots, problems and fixes is part of doing security work professionally.

---
# 🔗 Tools & Resources

- **7-Zip:** [7-zip.org](https://7-zip.org/download.html)
- **VirtualBox:** [virtualbox.org](https://virtualbox.org/wiki/Downloads)
- **Kali Linux:** [kali.org](https://kali.org/get-kali)
- **Metasploitable:** [sourceforge.net/projects/metasploitable](https://sourceforge.net/projects/metasploitable/)

---

# 👤 Author

**Sabine**\
Cybersecurity Intern – Networkwalks

GitHub: [quadmi](https://github.com/quadmi)\
LinkedIn: [https://www.linkedin.com/in/sabine-gharib-765b77346]
