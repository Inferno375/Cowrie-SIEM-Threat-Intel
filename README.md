# Cowrie-SIEM-Threat-Intel
Live Threat Intelligence dashboard capturing real-world brute-force attacks via an AWS-hosted Cowrie SSH honeypot and New Relic SIEM.

# Cowrie-SIEM-Threat-Intel

## Project Objective
The goal of this project was to deploy an interactive SSH honeypot to capture, log, and visualize real-world brute-force attacks and malware deployment attempts in a live cloud environment.

## Architecture & Tech Stack
* **Cloud Infrastructure:** AWS EC2 (`t3.micro`) running Ubuntu
* **Honeypot:** Cowrie (running in a Python virtual environment)
* **SIEM & Visualization:** New Relic Infrastructure Agent with custom JSON log integration
* **Networking:** `iptables` for internal port routing

## Implementation Steps
1. **Server Hardening:** Shifted the actual administrative SSH daemon to port `22222` to secure the host.
2. **Honeypot Deployment:** Installed Cowrie under a strictly isolated non-root `cowrie` user to prevent privilege escalation.
3. **Traffic Routing:** Configured `iptables` to silently forward all incoming traffic on the default SSH port (`22`) to the Cowrie listening port (`2222`).
4. **SIEM Integration:** Installed the New Relic Infrastructure Agent and created a custom logging pipeline (`cowrie.yml`) to stream raw `cowrie.json` event logs into the New Relic cloud dashboard in real-time.
5. **Data Visualization:** Built NRQL (New Relic Query Language) widgets to parse the JSON attributes and visualize the attack data.

## Threat Analysis & Results
Within 48 hours of exposing the server, the honeypot successfully trapped multiple automated botnets. Key intelligence gathered:
* **Credential Harvesting:** Captured automated dictionaries attempting to breach default Linux accounts like `root`, `ubuntu`, and `debian`.
* **Command Execution:** Recorded bots running system reconnaissance (`uname -a`, `cat /proc/cpuinfo`) and network discovery (`ifconfig`, `netstat`).
* **Attacker Origins:** Mapped global source IP addresses actively scanning and targeting the server.

<img width="1917" height="806" alt="Screenshot 2026-10-26 104625" src="https://github.com/user-attachments/assets/bb1b317e-bc22-430a-8407-abee083fcb3b" />
