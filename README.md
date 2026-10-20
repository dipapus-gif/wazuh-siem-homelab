# Home Lab: SIEM Deployment & Attack Simulation with Wazuh + Kali Linux

A hands-on home lab project simulating a real-world detection pipeline — from SIEM deployment to live attack simulation and MITRE ATT&CK-mapped alerting.

## Lab Architecture

- Wazuh Manager / Indexer / Dashboard — Ubuntu 26.04 LTS VM (VirtualBox, bridged networking)
- Attacker / Agent machine — Kali Linux VM (also running a Wazuh agent)
- Network — Bridged adapter on both VMs, same home LAN subnet

## What Was Accomplished

### Infrastructure
- Deployed the full Wazuh SIEM stack (manager, indexer, dashboard)
- Resolved service startup ordering issues
- Fixed dashboard timeout/performance issues under indexer load
- Enabled auto-start on boot via systemctl

### Agent Deployment
- Installed the Wazuh agent on Kali via the .deb package
- Resolved a wazuh-agent/wazuh-manager package conflict
- Fixed bridged networking for agent-to-manager communication
- Verified agent enrollment and Active status in the dashboard

### Offensive Simulation
- Used Hydra to run SSH brute-force attacks
- First run uncovered a default credential (kali:kali)
- Second run used a clean wrong-password wordlist (30 attempts) to simulate a sustained attack

### Detection & Analysis
- Observed baseline authentication failure logging (sshd, PAM, unix_chkpwd)
- Watched Wazuh escalate repeated failures into a Brute Force detection at rule level 10
- Confirmed MITRE ATT&CK mapping: Password Guessing, Valid Accounts, SSH, Brute Force, Remote Services
- Reviewed alert evolution, source attribution, and event timelines

## Key Results

| Metric | Value |
|---|---|
| Total alerts generated (24h) | 638+ |
| Brute force alerts (rule level 10) | 9 |
| MITRE techniques identified | 5 |
| Agents monitored | 1 (Kali) |

## Key Takeaway

Raw log noise isn't detection — correlation is. Watching individual low-severity authentication failures escalate in real time into a correctly classified, MITRE-mapped brute-force alert highlighted exactly what makes a SIEM valuable over plain log collection.

## Next Steps

- Stand up a dedicated (non-self-targeting) victim machine
- Test Wazuh Active Response (automatic IP blocking)
- Simulate port scanning (Nmap) detection
- Add a vulnerable web app target and test web attack detection
- Explore File Integrity Monitoring (FIM) alerts

## Tools Used

- Wazuh (SIEM)
- Kali Linux (attacker OS)
- Hydra (brute-force tool)
- Oracle VirtualBox (virtualization)

---

This is a personal home lab project for learning purposes. All attacks were conducted against self-owned/self-hosted systems in an isolated lab environment.
EOF
