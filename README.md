# ⚔️ Active Directory Attack & Detection Lab

[![PL](https://img.shields.io/badge/Język-Polski-red.svg)](README_PL.md)
[![EN](https://img.shields.io/badge/Language-English-blue.svg)](README.md)

## About the Project
This repository documents offensive security testing against a hardened Active Directory environment. The goal was to simulate real-world attack vectors using Kali Linux and verify the corresponding telemetry and detections inside Wazuh SIEM.

## Technologies Used
* **Attacker System:** Kali Linux (Nmap, smbclient, Bash scripting)
* **Target Environment:** Windows Server 2025 (`DC01`), Windows 11 Enterprise
* **SIEM / Monitoring:** Wazuh SIEM, Windows Security Event Log, Sysmon
* **Protocols & Services:** SMB, RDP, PowerShell

---

## Attack Scenarios & Detection Verification

### 1. Reconnaissance & Port Filtering Verification (Nmap)
* **Attack Vector:** Executed port scanning from Kali Linux against the Domain Controller IP to discover exposed services.
* **Observation:** RDP port scan (`nmap -p 3389 192.168.10.20`) returned `3389/tcp filtered`, validating the active GPO firewall rules and network isolation.

### 2. SMB Dictionary & Brute Force Attack
* **Attack Vector:** Simulated an automated password guessing attack using `smbclient` and a dictionary list (`pass.txt`) targeted at the Domain Controller SMB service.
* **SIEM Detection:** Wazuh collected failed authentication events (`Event ID 4625`) and automatically triggered a high-severity alert: `CRITICAL: Brute Force Attack Detected (Level 10)`.

### 3. Obfuscated PowerShell Execution Analysis
* **Attack Vector:** Executed Base64-encoded PowerShell payloads (`powershell.exe -EncodedCommand`) on the client machine to bypass basic string detection.
* **SIEM Detection:** Windows ScriptBlock Logging (`Event ID 4104`) automatically decoded the executed script content and forwarded the raw payload to Wazuh for analysis.

---

## Evidence and Screenshots

### Nmap Reconnaissance & Filtered Port
<img width="1146" height="947" alt="kali" src="https://github.com/user-attachments/assets/43b5659f-204d-4405-9dea-9c9b603d15cc" />

### SMB Brute Force Execution (Kali Linux)
<img width="718" height="209" alt="brute" src="https://github.com/user-attachments/assets/40be2d2a-85f4-4920-984c-7cd9dc17f34e" />

### Wazuh SIEM Brute Force Alert (Level 10)
<img width="1643" height="740" alt="brutewazuh" src="https://github.com/user-attachments/assets/a5169130-45d4-4a00-9876-779dd542f695" />

### PowerShell Decoded Execution Log (Event 4104)
<img width="1642" height="726" alt="wazuh" src="https://github.com/user-attachments/assets/07327f72-d387-4f59-a72d-34156ac51587" />

