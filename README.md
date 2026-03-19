# 🛡️ Project: Fileless PowerShell Attack & Remediation

## 🔍 Incident Overview
This project demonstrates the detection, investigation, and containment of a **fileless PowerShell attack** targeting a Windows endpoint. By leveraging **Microsoft Defender for Endpoint (MDE)** and **KQL**, I successfully identified obfuscated adversary tactics and performed remote remediation to secure the environment.

---

## 1. The Attack Landscape
* **Technique:** Fileless PowerShell Execution
* **Methodology:** The attacker utilized a **Base64-encoded payload** combined with **XOR decoding**. 
* **Adversary Tactic:** [MITRE ATT&CK T1059.001](https://attack.mitre.org/techniques/T1059.001/) (Command and Scripting Interpreter: PowerShell).
* **Detection:** **Microsoft Defender for Endpoint** triggered a "Behavioral Blocking" alert, flagging a suspicious `powershell.exe` process with high-entropy (encoded) command-line arguments.

---

## 2. Investigation Breakdown
I followed a structured Incident Response (IR) workflow to analyze the scope of the breach:

1.  **Alert Triage:** Confirmed the incident on host `cyberpotentialh` within the Microsoft Defender portal.
2.  **Process Tree Analysis:** Inspected parent-child relationships to identify the origin of the malicious PowerShell session.
3.  **Payload Analysis:** Manually decoded the Base64 and XOR-obfuscated payload using PowerShell, uncovering the hidden command-and-control (C2) intent.
<img width="911" height="448" alt="1" src="https://github.com/user-attachments/assets/7dca0b0f-cb33-429c-b0fe-a67882bb4da4" />
4.  **Evidence Collection:** Identified the source artifact: `AttackScript.ps1` located in the `Downloads` directory.

---

## 3. Containment & Remediation
To neutralize the threat, I pivoted to **Live Response** for direct interaction with the infected host:

* **Live Response Session:** Manually verified the artifact on the disk.
* **Execution:** Ran the `remediate` command to successfully **quarantine** the malicious script.
* **Verification:** Confirmed 100% containment through verified **JSON remediation logs**, ensuring no persistent backdoors remained.

---

## 🛠️ Tools & Technologies Used
* **SIEM/XDR:** Microsoft Defender XDR
* **Endpoint Security:** Microsoft Defender for Endpoint (MDE)
* **Query Language:** KQL (Kusto Query Language) for telemetry hunting
* **Forensics:** PowerShell Live Response Console

---

## 🚩 Key Indicators of Compromise (IoCs)
* **Process:** `powershell.exe` execution with `-EncodedCommand`.
* **Artifact:** `AttackScript.ps1`
* **Status:** Successful Quarantine via Live Response Remediation.

---

### **Growth Reflection**
This lab allowed me to move beyond just "seeing" an alert. I practiced the full lifecycle of a security incident—from understanding how attackers hide their code with encoding to using professional-grade tools to remotely clean a machine.

---
## 📖 Technical Glossary

* **Attack Vector:** The path or means by which an attacker gains access to a computer or network server in order to deliver a malicious outcome.
* **Base64 Encoding:** A binary-to-text encoding scheme used to transport data. Adversaries use it to hide command strings from simple pattern-matching security tools.
* **Behavioral Blocking:** A modern security capability (like in MDE) that stops a process not based on its "name," but because its *behavior* (like spawning a hidden PowerShell window) is known to be malicious.
* **C2 (Command and Control):** The infrastructure (usually an IP or Domain) used by an attacker to send commands to a compromised system and receive stolen data.
* **Fileless Malicious Script:** An attack that runs entirely in the computer's RAM (memory) rather than saving a file to the hard drive, making it harder for traditional antivirus to "scan" it.
* **High-Entropy:** In security, this refers to data that looks "random" or heavily scrambled. A command line full of random letters and numbers is "high-entropy" and is a major red flag for encoded malware.
* **XOR (Exclusive Or):** A simple mathematical bitwise operation. In this attack, it was used as a lightweight layer of encryption to further disguise the PowerShell payload.
