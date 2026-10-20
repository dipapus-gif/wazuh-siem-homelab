Home Lab: SIEM Deployment & Attack Simulation with Wazuh + Kali Linux
A hands-on home lab project simulating a real-world detection pipeline — from SIEM deployment to live attack simulation and MITRE ATT&CK-mapped alerting.
 Lab Architecture
Wazuh Manager / Indexer / Dashboard — Ubuntu 26.04 LTS VM (VirtualBox, bridged networking)
Attacker / Agent machine — Kali Linux VM (also running a Wazuh agent)
Network — Bridged adapter on both VMs, placing them on the same home LAN subnet for realistic agent-to-manager communication
 What Was Accomplished
Infrastructure
Deployed the full Wazuh SIEM stack (manager, indexer, dashboard) on an Ubuntu VM
Resolved service startup ordering issues (indexer → manager → dashboard)
Diagnosed and fixed dashboard timeout/performance issues under indexer load
Enabled all services to auto-start on boot via systemctl enable
Agent Deployment
Installed the Wazuh agent on a Kali Linux VM via the .deb package
Resolved a package conflict (wazuh-agent cannot coexist with wazuh-manager on the same host)
Fixed bridged networking so the agent VM could reach the manager across the LAN
Verified agent enrollment and confirmed Active status in the dashboard
Offensive Simulation
Used Hydra to run SSH brute-force attacks against the target
First run uncovered a default/weak credential (kali:kali) — a good real-world reminder of why default credentials are a common attack vector
Second run used a clean wrong-password wordlist (30 attempts) to properly simulate a sustained brute-force attempt without early exit
Detection & Analysis
Observed baseline authentication failure logging across multiple sources:
sshd: authentication failed (rule 5760)
PAM: User login failed (rule 5503)
unix_chkpwd: Password check failed (rule 5557)
Watched Wazuh's correlation engine escalate the repeated failures into a proper Brute Force detection at rule level 10
Confirmed automatic MITRE ATT&CK technique mapping, including:
Password Guessing
Valid Accounts
SSH
Brute Force
Remote Services
Reviewed alert evolution over time, source agent attribution, and full event timelines in the Wazuh dashboard
 Key Results
Metric
Value
Total alerts generated (24h)
638+
Brute force alerts (rule level 10)
9
MITRE techniques identified
5 (Password Guessing, Valid Accounts, SSH, Brute Force, Remote Services)
Agents monitored
1 (Kali)
 Key Takeaway
Raw log noise isn't detection — correlation is. Watching individual low-severity authentication failures escalate in real time into a correctly classified, MITRE-mapped brute-force alert highlighted exactly what makes a SIEM valuable over plain log collection: pattern recognition across time and volume, not just event capture.
 Next Steps

Stand up a dedicated (non-self-targeting) victim machine

Test Wazuh Active Response (automatic IP blocking)

Simulate port scanning (Nmap) detection

Add a vulnerable web app target and test web attack detection

Explore File Integrity Monitoring (FIM) alerts
 Tools Used
Wazuh (SIEM)
Kali Linux (attacker OS)
Hydra (brute-force tool)
Oracle VirtualBox (virtualization)
This is a personal home lab project for learning purposes. All attacks were conducted against self-owned/self-hosted systems in an isolated lab environment.
