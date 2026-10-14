# Lab Ataków i Detekcji w Active Directory

[![PL](https://img.shields.io/badge/Język-Polski-red.svg)](README_PL.md)
[![EN](https://img.shields.io/badge/Language-English-blue.svg)](README.md)

## O Projekcie
To repozytorium dokumentuje testy bezpieczeństwa ofensywnego przeprowadzone w utwardzonym środowisku Active Directory. Celem była symulacja realnych wektorów ataku z wykorzystaniem Kali Linux oraz weryfikacja odpowiadającej im telemetrii i detekcji w systemie Wazuh SIEM.

## Wykorzystane Technologie
- **System Atakujący:** Kali Linux (Nmap, smbclient, skrypty Bash)
- **Środowisko Docelowe:** Windows Server 2025 (`DC01`), Windows 11 Enterprise
- **SIEM / Monitoring:** Wazuh SIEM, Dziennik Zdarzeń Windows Security, Sysmon
- **Protokoły i Usługi:** SMB, RDP, PowerShell

---

## Scenariusze Ataków i Weryfikacja Detekcji

### 1. Zwiad (Reconnaissance) i Weryfikacja Filtrowania Portów (Nmap)
- **Wektor Ataku:** Przeprowadzono skanowanie portów z systemu Kali Linux na adres IP Kontrolera Domeny w celu wykrycia wystawionych usług.
- **Obserwacja:** Skanowanie portu RDP (`nmap -p 3389 192.168.10.20`) zwróciło wynik `3389/tcp filtered`, co potwierdziło poprawne działanie reguł zapory wdrożonych przez GPO oraz izolację sieciową.

### 2. Atak Słownikowy i Brute Force na SMB
- **Wektor Ataku:** Przeprowadzono symulację automatycznego ataku zgadywania haseł przy użyciu `smbclient` oraz listy słownikowej (`pass.txt`), skierowanego na usługę SMB Kontrolera Domeny.
- **Detekcja w SIEM:** Wazuh zebrał zdarzenia nieudanej autentykacji (`Event ID 4625`) i automatycznie wygenerował alert o wysokim priorytecie: `CRITICAL: Brute Force Attack Detected (Level 10)`.

### 3. Analiza Wykonania Zaciemnionego Kodu PowerShell (Obfuskacja)
- **Wektor Ataku:** Uruchomiono zakodowane w Base64 ładunki PowerShell (`powershell.exe -EncodedCommand`) na maszynie klienckiej w celu obejścia podstawowej detekcji ciągów znaków.
- **Detekcja w SIEM:** Mechanizm Windows ScriptBlock Logging (`Event ID 4104`) automatycznie zdekodował treść wykonanego skryptu i przekazał czysty ładunek do systemu Wazuh w celu analizy.

---

## Dowody i Zrzuty Ekranu

### Zwiad Nmap i Przefiltrowany Port
<img width="1146" height="947" alt="kali" src="https://github.com/user-attachments/assets/43b5659f-204d-4405-9dea-9c9b603d15cc" />

### Wykonanie Ataku SMB Brute Force (Kali Linux)
<img width="718" height="209" alt="brute" src="https://github.com/user-attachments/assets/40be2d2a-85f4-4920-984c-7cd9dc17f34e" />

### Alert Wazuh SIEM o Ataku Brute Force (Poziom 10)
<img width="1643" height="740" alt="brutewazuh" src="https://github.com/user-attachments/assets/a5169130-45d4-4a00-9876-779dd542f695" />

### Zdekodowany Log Wykonania PowerShell (Event 4104)
<img width="1642" height="726" alt="wazuh" src="https://github.com/user-attachments/assets/07327f72-d387-4f59-a72d-34156ac51587" />
