# NETWORKWALKS-BATCH-083A-CYBERSECURITY-LAB-SETUP

This repository details the re-configuration and customisation of a multi-OS virtual lab built on Oracle VirtualBox for cybersecurity and ethical hacking practice.



## Cybersecurity Lab Environmental Setup

Building an isolated virtual lab using VirtualBox and Kali Linux for cybersecurity testing and penetration testing.

## Project Overview

This project focuses on setting up a virtual cybersecurity and penetration-testing laboratory using VirtualBox and Kali Linux.
The aim of the lab is to create a sandbox environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security activities can be performed safely and frequently.
The lab is configured on a private virtual network to allow additional machines to be added later on and also used as targets for authorized security testing.

## Objectives

The major aim of this project is to:

- Install and configure VirtualBox
- Install Kali Linux as a virtual machine
- Select a private NAT Network for the cybersecurity lab
- Configure network and ensure connectivity for Kali Linux
- Assign a static IP address to the Kali VM
- Verify network connectivity and DNS resolution
- Take a snapshot of the VM for easy data recovery
- Document the entire setup process
- Prepare the environment for future cybersecurity projects

## Purpose of the Lab

The lab supports an isolated and controlled environment for cybersecurity learning and authorized security testing.
Also, it can be used for activities like:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Exploitation practice
- Security-tool experimentation

⚠️ It is important to note that this laboratory must only be used for systems that you own and have proper permission to test. Ensure not to use the lab or its tools to attack unauthorized systems.

## ⚙️ Lab Configuration

| 🧩 Component       | ⚙️ Configuration     |
| ----------------- | -------------------- |
| 💻 Host OS         | Windows 10            |
| 🧠 Host RAM        | 8 GB                  |
| 🧰 Hypervisor      | VirtualBox            |
| 🐉 Security OS     | Kali Linux 2025.4     |
| 🌎 Virtual Network | NAT Network (LabNetwork) |
| 📡 Network Address | 10.0.2.0/24           |
| 🦤 Kali IP Address | 10.0.2.2/24 (pending — see Lab Verification below) |
| 🚪 Default Gateway | 10.0.2.1              |
| 🌎 DNS Server      | 8.8.8.8               |

# Lab Setup Procedure

## Step 1. Download Kali Linux

The official pre-built VirtualBox image, `kali-linux-2025.4-virtualbox-amd64`, was downloaded from the Kali Linux website.

## Step 2. Install VirtualBox

VirtualBox was installed as the hypervisor on the Windows 10 host.

## Step 3. Create the NAT Network

A NAT Network was created in VirtualBox:

```
Network Name: LabNetwork
IPv4 Prefix: 10.0.2.0/24
DHCP: Enabled
```

A NAT Network was selected because it allows more than one virtual machine connected to the same NAT Network to communicate with each other while also having outbound internet connection. This makes room for future attacker and target VMs to communicate within the lab.

## Step 4. Import Kali Linux

The Kali Linux virtual machine was imported into VirtualBox, and its network adapter was attached to the `LabNetwork` NAT Network.

## Step 5. Boot and Check Network Assignment

On boot, Kali displayed: *"Requesting an ethernet network address for 'Wired connection 1'…"* while waiting on a DHCP lease from LabNetwork.

Running `ip a` inside Kali showed `eth0` as `UP`, but with only a link-local IPv6 address — no IPv4 lease had been received yet, and the system tray showed "No network connection."

## Step 6. Create a Clean VM Snapshot

*(To be done once networking is verified working — see Lab Verification.)*
Planned snapshot name: `Clean Kali LabNetwork Setup`

This snapshot will represent the clean baseline of the lab, so the machine can be restored to this state if a future exercise alters its configuration.

## Lab Verification

| Test                          | Command                          | Expected Result                   | Result |
| ------------------------------ | --------------------------------- | ---------------------------------- | ------ |
| 🌎 Check IP Address            | `ip a`                             | IPv4 address in `10.0.2.0/24` shown | ❌ Fail — only link-local IPv6 present |
| 📡 Test Gateway                | `ping 10.0.2.1`                    | Successful replies                 | ⬜ Pending |
| 🌎 Test Internet Connectivity  | `ping 8.8.8.8`                     | Successful replies                 | ⬜ Pending |
| 🔎 Test DNS Resolution         | `nslookup networkwalks.com`        | Domain resolves                    | ⬜ Pending |
| 🧰 Verify Nmap                 | `nmap --version`                   | Nmap version displayed             | ⬜ Pending |
| 🔄 Verify Snapshot             | Restore snapshot and run `ip a`    | Baseline configuration restored    | ⬜ Not yet created |

### Troubleshooting Note

`eth0` came up (`UP`, `LOWER_UP`) but never received an IPv4 lease from LabNetwork's DHCP server. Next steps to resolve:

```bash
nmcli connection show
sudo dhclient -v eth0
sudo systemctl restart NetworkManager
```

If the issue persists, double-check in VirtualBox (VM powered off) that the adapter is attached to **NAT Network → LabNetwork** specifically, and that the adapter is enabled.

# What I Learned

## 1. NAT vs NAT Network

A NAT configuration allows internet access but does not let machines on the VM communicate with each other. NAT Network allows more than one machine to communicate with each other while also allowing internet access — an integral part of building a multi-machine cybersecurity laboratory.

## 2. Virtual Machine Networking

How VirtualBox virtual network adapters connect virtual machines to different types of networks, and how that configuration affects how machines communicate with each other.

## 3. Static IP Configuration

How to configure and verify a static IPv4 address, subnet mask, gateway, and DNS settings in Kali Linux — and how to diagnose a failed DHCP lease using `ip a` and `nmcli`.

## 4. VM Snapshots

The importance of creating a clean snapshot before performing risky activities, to make recovery easy for future tasks.

## 5. Documentation

Documenting commands, configuration, screenshots, problems, and solutions is a very important part of a professional cybersecurity project.

# Security & Ethical Use

This laboratory is for educational purposes only.

# Tools & Resources

- VirtualBox: https://virtualbox.org/wiki/Downloads
- Kali Linux: https://kali.org/get-kali

# Author

Oyeru Rachael Emalohi

# Project Information

Program Name: Cybersecurity Lab Setup | Week: 01 | Project: Cybersecurity & Pentesting Lab Setup | Repository: GitHub

# Acknowledgement

This project's structure and documentation format was inspired by NetworkWalks Academy-style lab documentation.

## 📌 Project Metadata

| Field | Value |
|---|---|
| Program | Cybersecurity Internship / Self-Study (NetworkWalks-style Lab) |
| Author | [Oyeru Racheal Emalohi] |
| Host OS | Windows 10 |
| Hypervisor | Oracle VirtualBox |
| Guest OS | Kali Linux 2025.4 (kali-linux-2025.4-virtualbox-amd64) |
| Networking Mode | NAT Network — "LabNetwork" |
| Subnet | 10.0.2.0/24 |
| DHCP | Enabled (network-level) |
| Repository | GitHub (public) |
| Version | 1.0 |
| Last Updated | 2026-09-11 |

## 🎯 Objective

Build and document a personal, isolated virtual lab environment on Windows 10 for practicing ethical hacking, penetration testing, and network security fundamentals using Oracle VirtualBox and Kali Linux.

> ⚠️ **Disclaimer:** This repository is for educational purposes only. All techniques and tools referenced here are intended strictly for learning in a fully isolated, personal virtual lab environment — not for use against systems you don't own or have explicit permission to test.

## 🛠️ Tools Used

- Oracle VirtualBox
- Kali Linux 2025.4 (VM)
  
  

## 🖥️ Lab Architecture

- **Host:** Windows 10 machine running Oracle VirtualBox
- **Network:** Custom NAT Network named **LabNetwork**, on the **10.0.2.0/24** subnet, with **DHCP enabled** at the network level
- **Guest VM:** Kali Linux 2025.4, network adapter attached to LabNetwork (rather than the VM's default NAT)
- **Isolation:** NAT Network allows VM-to-VM and VM-to-internet communication while keeping the lab isolated from the host's main network

## ⚙️ Setup Steps

1. Downloaded the official pre-built VM image: `kali-linux-2025.4-virtualbox-amd64`.
2. Installed **Oracle VirtualBox** on the Windows 10 host.
3. Created a **NAT Network** named `LabNetwork` (File → Preferences → Network → NAT Networks), using subnet `10.0.2.0/24` with DHCP enabled.
4. Imported the Kali Linux VM and attached its network adapter to `LabNetwork`.
5. Booted the VM — Kali's boot screen showed *"Requesting an ethernet network address for 'Wired connection 1'…"* while it waited for a DHCP lease.
6. Ran `ip a` inside Kali to confirm the assigned address.

## 🔎 Lab Verification Results

| Test | Command | Expected Output | Result |
|---|---|---|---|
| VM boots and reaches desktop | — | Kali desktop loads | ✅ Pass |
| VM requests DHCP lease | Boot-time notification | "Requesting an ethernet network address..." shown | ✅ Pass |
| IP Address assigned | `ip a show eth0` | IPv4 address in `10.0.2.0/24` range shown | ❌ **Fail** — `eth0` shows `UP` but only a link-local IPv6 address (`fe80::...`); no IPv4 lease received. Status bar shows **"No network connection."** |
| Gateway Reachability | `ping -c 4 10.0.2.1` | 4/4 packets received | ⬜ Blocked by above |
| Internet Connectivity | `ping -c 4 8.8.8.8` | 4/4 packets received | ⬜ Blocked by above |
| DNS Resolution | `nslookup google.com` | Non-authoritative answer returned | ⬜ Blocked by above |
| Nmap Installed | `nmap --version` | Version 7.9x+ displayed | ⬜ Not yet tested |

## 🩹 Troubleshooting Notes

- **Symptom:** After booting Kali on the `LabNetwork` NAT Network, `eth0` comes up (`UP`, `LOWER_UP`) but never receives an IPv4 address — only a link-local IPv6 (`fe80::.../64`) is present, and the taskbar reports "No network connection."
  **Likely cause:** DHCP request to the NAT Network's built-in DHCP server isn't completing — possibly a stalled `dhclient`/NetworkManager request, or the network adapter type/attachment needs to be re-checked in the VM's settings.
  **Next steps to try:**
  ```bash<img width="1919" height="1080" alt="machines" src="https://github.com/user-attachments/assets/a108bda0-2cb5-41fb-ad72-cc8bc1822b9f" />
<img width="1669" height="1061" alt="kali" src="https://github.com/user-attachments/assets/aa315727-858b-4aa6-a25c-3f586b76d65b" />
<img width="1920" height="1037" alt="virtualbox" src="https://github.com/user-attachments/assets/d7f5e934-a0e6-469e-a14d-2c1f99339d9c" />
<img width="1920" height="997" alt="ipv4" src="https://github.com/user-attachments/assets/49748194-5d1b-438d-be5a-370ed9480955" />

  nmcli connection show
  sudo dhclient -v eth0
  # or restart networking:
  sudo systemctl restart NetworkManager
  ```<img width="1932" height="2576" alt="05-natnetwork-labnetwork-settings-2" src="https://github.com/user-attachments/assets/2a8cfca3-0598-4ffe-ab11-7d78757c035f" /><img width="1449" height="2576" alt="01-kali-iso-file" src="https://github.com/user-attachments/assets/0de90407-64e0-4951-9f24-2edc4ebe6ca1" />
<img width="1932" height="2576" alt="02-kali-boot-requesting-address" src="https://github.com/user-attachments/assets/da650fe3-ea7d-450e-97d9-8aa0db52c5df" />
<img width="2576" height="1932" alt="03-ip-a-no-ipv4-assigned" src="https://github.com/user-attachments/assets/39f3e72c-0fbc-49e3-a520-6218d3ad1747" />
<img width="1932" height="2576" alt="04-natnetwork-labnetwork-settings" src="https://github.com/user-attachments/assets/532ebecf-b115-4fa2-95e8-a3c36e2434eb" />


- **NAT vs. NAT Network:** NAT Network enables multi-VM communication plus internet access; standard NAT isolates VMs from each other.
- A VM interface can show `UP` at the link layer while still failing to obtain an IPv4 lease — always check for an actual `inet` line in `ip a`, not just interface state.

## 📷 Screenshots

See the `/screenshots` folder:
- `01-kali-iso-file.jpeg` — Kali Linux 2025.4 VirtualBox image file
- `02-kali-boot-requesting-address.jpeg` — Boot screen requesting a DHCP lease
- `03-ip-a-no-ipv4-assigned.jpeg` — `ip a` output showing no IPv4 on `eth0`
- `04-natnetwork-labnetwork-settings.jpeg` / `05-natnetwork-labnetwork-settings-2.jpeg` — VirtualBox NAT Network config for `LabNetwork` (10.0.2.0/24, DHCP enabled)

## 🙏 Acknowledgments

Built as a self-guided cybersecurity lab exercise, inspired by NetworkWalks Academy-style lab documentation.

[README (1).md](https://github.com/user-attachments/files/32173717/README.1.md)
