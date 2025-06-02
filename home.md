# Reverse Shell via .desktop File  
*Cybersecurity Demo Report*

---

## 1. Objective

This demo showcases a phishing-style reverse shell attack using a disguised `.desktop` file on Ubuntu.  
When the victim double-clicks the file, it initiates a reverse shell back to the attacker's Kali machine without opening a terminal or requiring further interaction.

---

## 2. Environment Setup

- **Attacker:** Kali Linux  
- **Victim:** Ubuntu Desktop VM  
- **Listener Tool:** Netcat (nc)  
- **Network:** Bridged Adapter with IP `192.168.1.114` (Kali)  

---

## 3. Tools Used

- Netcat for listening on the attacker’s side  
- Bash reverse shell payload embedded in a `.desktop` file  
- Python HTTP server for file delivery  
- `curl` for file download  

---

## 4. Attack Scenario

A `.desktop` file named `importantFile.pdf.desktop` was created with a malicious `Exec` command that opens a reverse shell to the attacker's machine.  
When the file is made executable and allowed for launching in Ubuntu, double-clicking it initiates a shell session back to the attacker.

---
