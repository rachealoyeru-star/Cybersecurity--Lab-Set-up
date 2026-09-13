# Cybersecurity--Lab-Set-up
This repository details the re-configuration and customisation of a multi-OS virtual lab built on Oracle VirtualBox for cybersecurity and ethical hacking practice.

# Cybersecurity & Penetration Testing Lab Setup

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
- Nmap
- (add any other tools you install, e.g. Wireshark, Metasploit, Burp Suite)

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
