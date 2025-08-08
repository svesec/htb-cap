🧠 HTB Machine: Cap (Retired)
Overview
Machine Name: Cap
Difficulty: Easy
Operating System: Linux
Enumeration Focus: PCAP analysis, FTP credentials, local privilege escalation
Initial Foothold: FTP credentials discovered through packet capture
Privilege Escalation: Could not reproduce – documented based on video analysis

🛰️ Reconnaissance
bash

nmap -sC -sV -oA cap_scan 10.10.10.245

Open ports:

21/tcp – FTP (vsftpd 3.0.3)

22/tcp – SSH (OpenSSH 8.x)

80/tcp – HTTP (gunicorn server)

HTTP enumeration revealed a /capture endpoint for downloading .pcap files.

📦 Packet Capture Analysis
Downloaded PCAP files:

http://10.10.10.245/data/1 http://10.10.10.245/data/2 http://10.10.10.245/data/3

Used Wireshark and base64 decoding to attempt manual credential extraction – unsuccessful in current environment.

❗ Note: Credentials for nathan:nathan were not discoverable in this instance due to possible changes in the machine or environment. In the original IppSec video, these credentials are found in clear text inside HTTP payloads within the PCAP.

🔐 Initial Access via FTP
bash

ftp 10.10.10.245 Name: nathan Password: nathan

Successfully authenticated and downloaded user.txt.

🐚 SSH Access
bash

ssh nathan@10.10.10.245

Confirmed shell access:

bash

whoami id

⚠️ Privilege Escalation
Tried common enumeration:

bash

sudo -l find / -perm -4000 -type f 2>/dev/null

However, no working vector for escalation was found in current session.

In the video walkthrough, privilege escalation is achieved through a script at /opt/cap/cap_user.sh, which does not exist in this session – likely removed or altered.

✅ Conclusion
Although the full root flag couldn't be retrieved, this machine still served as valuable practice in:

Web & network traffic enumeration

Packet analysis with Wireshark

FTP-based access

Working with real SSH sessions on HTB
