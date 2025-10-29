# Suricata IDS vs. Nmap Reconnaissance Lab

##  Project Overview

This project established a comprehensive home lab environment to simulate and detect malicious network reconnaissance. Acting as a red team, I used Nmap to perform stealthy TCP and UDP scans on a target machine, mimicking how attackers map out network services and discover open ports.

On the defensive side, I configured Suricata, an open-source Intrusion Detection System (IDS), to monitor network traffic in real-time. The Suricata instance successfully detected and alerted on the reconnaissance scans, flagging suspicious probes to services like MySQL, PostgreSQL, Oracle, and MSSQL.

This exercise served as a full-scope Red Team vs. Blue Team scenario, demonstrating a practical understanding of both offensive tactics and defensive strategies.

## 🛠️ Skills & Technologies

* **Intrusion Detection:** Suricata (IDS)
* **Network Scanning:** Nmap / Zenmap
* **Log Analysis:** Real-time log monitoring and analysis (`fast.log`)
* **Network Protocols:** TCP/UDP, ICMP, MySQL, PostgreSQL, MSSQL
* **Cybersecurity:** Red Team (Offensive Scanning) vs. Blue Team (Defensive Monitoring)
* **Virtualization:** Home lab environment setup

##  Workflow Architecture & Workflow

The diagram below illustrates the complete attack and defense flow, from the Nmap scan initiation to the Suricata IDS analysis and final alert generation for the defender.

<p align="center">
  <img src=".assets/Architectural Layout.jpg" alt="Red Team vs. Blue Team Architecture" width="800"/>
</p>
<p align="center">Figure 1: The architectural layout of the reconnaissance simulation.</p>

##  Live Detection & Output

The following images show the real-time execution from both the attacker's (Red Team) and defender's (Blue Team) perspectives.

### Red Team: Nmap Scan Output

The Nmap command used was a highly customized scan combining multiple techniques. The Zenmap output shows the scan beginning, specifically stating "Initiating SYN Stealth Scan" and "Initiating UDP scan," which are the actions that will be detected by the IDS.

<p align="center">
  <img src=".assets/Zenmap output.jpg" alt="Zenmap scan output" width="800"/>
</p>
<p align="center">Figure 2: Zenmap terminal output showing the initiation of stealthy port scans.</p>

### Blue Team: Suricata Log Analysis

This terminal shows the real-time `tail -f` of Suricata's `fast.log`. As the Nmap scan progressed, Suricata successfully identified and logged multiple "ET SCAN Suspicious inbound" events,
correctly classifying the probes to database ports like MySQL, PostgreSQL, and MSSQL.

<p align="center">
  <img src=".assets/Suricata Logs.jpg" alt="Suricata alert logs" width="800"/>
</p>
<p align="center">Figure 3: Suricata's `fast.log` detecting the Nmap scan in real-time.</p>

##  Key Configuration & Code

This scenario is broken into the Red Team command used to initiate the scan and the Blue Team log file showing the detection.

### Red Team: `run_scan.sh`

This is the advanced Nmap command used, which blends TCP SYN and UDP scans, OS/version detection (`-A`), and script execution.

```bash
# Nmap command combining TCP SYN (-sS) and UDP (-sU) scans with script discovery
nmap -sS -sU -T4 -A -v -PE -PP -PS80,443 -PA3389 -PU40125 -PY -g 53 --script "default or (discovery and safe)" 172.27.135.26
