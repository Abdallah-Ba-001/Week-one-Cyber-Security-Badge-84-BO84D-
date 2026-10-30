 /># 🔐 Cybersecurity Lab Environment Setup

**Building an isolated virtual lab for penetration testing and ethical hacking practice**

---

## 📌 Project Overview

This project sets up a **virtual cybersecurity and penetration-testing lab** using VirtualBox and Kali Linux.

The lab gives me a controlled environment where I can practice cybersecurity tools safely and repeatedly. It runs on a private virtual network, so more machines can be added later as targets for authorized security testing.

---

## 🎯 Objectives

- Install and configure VirtualBox
- Import Kali Linux as a virtual machine
- Create a private NAT Network (10.0.0.0/24)
- Assign the static IP 10.0.0.2/24 to Kali Linux
- Verify network connectivity
- Take a clean VM snapshot for recovery
- Document the full setup, problems and solutions

---

## ⚠️ Ethical Use

This lab is for education only. It must only be used on systems I own or have written permission to test.

---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
|---|---|
| 🖥️ Host OS | Windows 11 |
| 🧠 Host RAM | 16 GB |
| ⚡ Processor | 1.60 GHz (1.80 GHz) |
| 🧰 Hypervisor | VirtualBox 7.2.22 |
| 🐉 Security OS | Kali Linux 2026.3 |
| 🧠 Kali RAM | 4096 MB |
| 🌐 Virtual Network | NAT Network (NatNetwork) |
| 📡 Network Address | 10.0.0.0/24 |
| 🐧 Kali IP Address | 10.0.0.2/24 |
| 🚪 Default Gateway | 10.0.0.1 |
| 🌍 DNS Server | 8.8.8.8 |

---

## 🪜 Lab Setup Procedure

### Step 1. Install 7-Zip
7-Zip was installed to extract the Kali Linux image, which comes as a `.7z` archive.

### Step 2. Install VirtualBox
VirtualBox 7.2.22 was installed as the hypervisor.

### Step 3. Create the NAT Network
In VirtualBox Expert mode, I opened **Tools > Network > NAT Networks** and edited the default network:

- Name: NatNetwork
- IPv4 Prefix: 10.0.0.0/24
- DHCP: Enabled
- IPv6: Disabled

![NAT Network settings](<img width="652" height="409" alt="2-screenshot-network-settings-1 png)" src="https://github.com/user-attachments/assets/7ed0301a-5128-48a6-b0e4-4a6877c66e3e" />
)

A NAT Network lets several virtual machines talk to each other while also having outbound internet access, so future attacker and target machines can share the same lab.

### Step 4. Import Kali Linux
I downloaded the pre-built Kali Linux VirtualBox image (64-bit) from the official Kali website, extracted it with 7-Zip, and added the `.vbox` file to VirtualBox.

VM network adapter:

```
Adapter 1
Attached to: NAT Network
Name:        NatNetwork
Promiscuous Mode: Allow All
```

I also enabled bidirectional clipboard and drag-and-drop, and shared the Windows Downloads folder with the VM.

![Kali Linux running](3-<img width="652" height="409" alt="Screenshot 2026-10-30 145321" src="https://github.com/user-attachments/assets/d26ea760-e067-4137-ba70-54ccdfc9187e" />
 )

### Step 5. Configure the Kali Linux Network
Kali first received a dynamic address (10.0.0.3) from DHCP. I set a static IP from the terminal:

```
sudo nmcli connection modify "Wired connection 1" ipv4.method manual ipv4.addresses 10.0.0.2/24 ipv4.gateway 10.0.0.1 ipv4.dns 8.8.8.8
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
ip a
```

Final configuration:

```
IP Address:  10.0.0.2
Subnet Mask: 255.255.255.0 (/24)
Gateway:     10.0.0.1
DNS:         8.8.8.8
```

![Kali network settings](4-screenshot-kali-network-settings.png)

### Step 6. Create a Clean VM Snapshot
After the network worked, I took a VirtualBox snapshot named **Clean Kali - Network Setup**. If a future exercise breaks the VM, I can restore this baseline.

---

## 🔎 Lab Verification

| ✅ Test | 🧾 Command | 🎯 Result |
|---|---|---|
| Check IP address | `ip a` | eth0 shows 10.0.0.2/24 |
| Test gateway | `ping -c 3 10.0.0.1` | 3 of 3 replies, 0% loss |
| Test internet | `ping -c 3 8.8.8.8` | 3 of 3 replies, 0% loss |

![Ping test results](5-screenshot-ping-test.png)

---

## 🐞 Problems Encountered & Solutions

### Problem 1. Network option missing in VirtualBox
The Network tool was not visible under Tools, and the guide's screenshots showed it. VirtualBox was in Basic mode, which hides some tools. I switched to Expert mode and the Network tool appeared. I then changed the default NatNetwork from 10.0.2.0/24 to 10.0.0.0/24.

### Problem 2. Wrong Kali file
The first Kali file I received was the installer ISO, not the pre-built VirtualBox image, so there was no `.vbox` file to import. I downloaded the VirtualBox 64-bit image from the official Kali website and imported its `.vbox` file instead.

### Problem 3. Kali network showed "disconnected"
The network icon showed disconnected, even though `nmcli device status` showed eth0 as connected with a dynamic address. Setting the static IP from the terminal with `nmcli connection modify`, then restarting the connection, fixed it. `ip a` showed 10.0.0.2/24, and pings to the gateway and the internet worked.

### Problem 4. Network broke after restarting the VM
After I shut the VM down and reopened it, the network stopped working. Re-checking the adapter settings and re-applying the static IP with `nmcli` restored it.

---

## 💡 What I Learned

This was my first hands-on experience with virtualization and networking, because we had not practiced this before.

- **NAT Network:** it keeps the lab separate from my main network while letting virtual machines talk to each other and reach the internet.
- **Static IP configuration:** I learned to set the IP, subnet mask, gateway and DNS in Kali Linux from the terminal.
- **Checking my files:** I learned to confirm I have the right Kali file (pre-built VirtualBox image, not the installer) before importing.
- **Tools differ from guides:** VirtualBox menus can look different between modes and versions.
- **Snapshots:** taking a clean snapshot before experimenting gives me a safe restore point.
- **Troubleshooting:** every error I solved taught me more than the steps that worked first time.

---

## 🔗 Tools & Resources

- 7-Zip: https://7-zip.org/download.html
- VirtualBox: https://virtualbox.org/wiki/Downloads
- Kali Linux: https://kali.org/get-kali

---

## 👤 Author

**Abdallah Bah**
Cybersecurity Intern, Batch B084D
LinkedIn URL:https://www.linkedin.com/public-profile/settings/

## 📌 Project Information

**Program:** Cybersecurity at Networkwalks | **Week:** 01 | **Project:** Cybersecurity & Pentesting Lab Setup | **Batch:** B084D
