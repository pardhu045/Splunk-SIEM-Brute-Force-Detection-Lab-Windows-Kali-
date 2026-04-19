# Splunk-SIEM-Brute-Force-Detection-Lab-Windows-Kali-


##  Project Overview

This project demonstrates a real-world SIEM (Security Information and Event Management) setup using Splunk.

The lab simulates brute-force login attacks from a Kali Linux machine targeting a Windows 10 system, with logs collected and analyzed in Splunk for detection and alerting.

---

##  Architecture

* **Attacker:** Kali Linux
* **Target:** Windows 10 VM
* **SIEM:** Splunk Enterprise (Windows Host)
* **Log Forwarding:** Splunk Universal Forwarder

---

## Setup Steps

### 1. Splunk Setup

* Installed Splunk Enterprise on Windows host
* Enabled receiving on port `9997`

### 2. Linux Forwarder Setup

* Installed Splunk Universal Forwarder on Kali Linux
* Added monitor:

```
/var/log
```

### 3. Windows Forwarder Setup

* Installed Splunk Universal Forwarder on Windows 10 VM
* Configured `inputs.conf`:

```
[WinEventLog://Security]
disabled = 0
```

---

##  Attack Simulation

Simulated brute-force attack using Hydra:

```
hydra -l <username> -P rockyou.txt smb://<target-ip>
```

---

##  Detection in Splunk

### Failed Login Detection (Event ID 4625)

```
index="main" sourcetype="WinEventLog:Security" EventCode=4625
```

### Brute Force Detection

```
index="main" sourcetype="WinEventLog:Security" EventCode=4625
| stats count by Account_Name, host
| where count > 3
```

---

##  Alert Configuration

* Real-time alert
* Trigger when failed login attempts exceed threshold
* Action: Add to Triggered Alerts

---

##  Results

* Successfully detected failed login attempts
* Real-time alerts generated for brute force activity
* Logs collected from both Linux and Windows systems

---

Pardhasaradhi
