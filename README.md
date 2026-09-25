# SBT-DF203 — Lab 9: WEP40 Wireless Packet Decryption and Aircrack Forensics

Offline forensic analysis of a historical WEP40 wireless capture — evidence preservation, key recovery, packet decryption, endpoint inventory, and object carving.

---

## Author

| Field | Detail |
| :--- | :--- |
| **Student** | Ibrahim Diseh Garba |
| **Registration No.** | `2025/FWSD/11521` |
| **Programme** | Fellowship in Web Application Security & Digital Forensics |
| **Institution** | International Cybersecurity and Digital Forensics Academy (ICDFA) |
| **Course** | SBT-DF203 — Basic Networking Skills for Digital Forensics |
| **Instructor** | Aminu Idris, AMCPN |
| **Delivery Block** | 3/3 of 3 |
| **Scheduled Dates** | 19–25 September 2026 |
| **Submission Date** | 25 September 2026 |

---

## Overview

This repository contains the offline forensic analysis of a historical wireless capture protected by the deprecated **WEP40** encryption standard. The capture originates from the **CodeGate CTF 2015 "Good Crypto"** challenge and was supplied by ICDFA as an authorised training artefact.

The investigation preserved the compressed evidence, recovered the 40-bit WEP key using `aircrack-ng`, decrypted the capture offline with `airdecap-ng`, and inventoried endpoints, protocols, and transferred objects. Every artefact is hashed with SHA-256 for chain-of-custody. No live wireless network was captured, cracked, or connected to.

---

## Objectives

1. Identify management, control, and data frames in an 802.11 capture.
2. Explain WEP40 structure, RC4 keystream use, IV reuse, and integrity limitations.
3. Use Aircrack-ng on a supplied historical capture only.
4. Decrypt WEP traffic using `airdecap-ng` and verify output files.
5. Extract IP/MAC endpoints, protocols, images, and HTML from decrypted traffic.
6. Distinguish a WEP key from a human passphrase and document optional CTF password analysis.
7. Recommend modern wireless security controls.

---

## Environment

| Component | Value |
| :--- | :--- |
| Operating System | Kali Linux VM (ICDFA lab) |
| Wireless Tools | `aircrack-ng`, `airdecap-ng` |
| Packet Tools | TShark, Wireshark |
| Carving Tools | `foremost`, `binwalk`, `file`, `strings` |
| Compression | `xz-utils` |
| Evidence | CodeGate CTF 2015 capture (`file.xz`) |
| Analysis Mode | Offline only — no live wireless capture |

---

## Methodology

| Phase | Description | Output |
| :---: | :--- | :--- |
| 1 | Folder setup and tool installation | `SBT-DF203-Lab9/` tree |
| 2 | Preserve, hash, decompress the evidence | `file_working`, `working_hashes.txt` |
| 3 | Inventory 802.11 frame categories | `wlan_frame_sample.tsv` |
| 4 | Confirm WEP protection and IV evidence | `wep_protected_frames.tsv`, `repeated_iv_summary.txt` |
| 5 | Recover/validate WEP40 key | `aircrack_output.txt` |
| 6 | Decrypt capture offline | `file_working-dec`, `airdecap_output.txt` |
| 7 | Extract endpoints and conversations | `ethernet_endpoints.txt`, `ip_endpoints.txt`, `tcp_conversations.txt` |
| 8 | Export HTTP objects and carve files | `exported/http_objects/`, `exported/foremost/` |

---

## Key Findings

| Indicator | Value |
| :--- | :--- |
| Capture format | IEEE 802.11 (tcpdump) |
| Total packets | 45,169 |
| WEP-protected data frames | 15,477 |
| BSSID | `00:26:66:55:97:D6` |
| ESSID | `cgnetwork` |
| Recovered WEP40 key | `A4:3D:F6:F3:74` |
| Decryption rate | 100% (0 corrupted frames) |
| Decrypted output | `working/file_working-dec` |
| Protocols visible post-decryption | ARP, DHCP, ICMPv6, TCP, HTTP |
| Recovered objects | HTML, JPEG, PNG, GIF |
| Evidence integrity | SHA-256 recorded for all artefacts |

### Verdict

WEP40's 24-bit IV space and RC4 keystream-reuse property allowed the key to be recovered from ~15,477 IVs. The entire capture decrypted with **zero corruption** — proving WEP offers no meaningful confidentiality. This is a design-level failure, not a configuration issue: **WEP should be retired, not strengthened**.

---

License
Submitted as academic coursework for SBT-DF203 Lab 9 at ICDFA. Contents may not be redistributed, reused, or reproduced without written permission from the author and ICDFA.

© 2026 Ibrahim Diseh Garba. All rights reserved.
