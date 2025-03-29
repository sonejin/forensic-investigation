Forensic Investigation of Malicious Web Shell Attack on Company Web Server
Overview
In this repository, we document the forensic analysis of a suspicious activity detected on a company web server. The attack involved the unauthorized upload of a malicious web shell, followed by command execution and an attempt to exfiltrate sensitive data. This lab walks through the process of analyzing a PCAP file captured during the incident, using Wireshark to identify key aspects of the attack.

Lab Objective
The goal of this lab is to perform a forensic analysis of a security incident involving a web shell and to understand the methodologies used by the attacker to gain unauthorized access to the server. The analysis includes identifying the geographical origin of the attack, determining the malicious web shell’s name, uncovering the communication channels used by the attacker, and identifying sensitive files targeted for exfiltration.

Table of Contents
Analysis Overview

Step-by-Step Analysis

Q1: Identifying the Geographical Origin

Q2: Attacker's User-Agent

Q3: Malicious Web Shell Identification

Q4: Directory for Uploaded Files

Q5: Port Used for Outbound Communication

Q6: Identifying the Exfiltrated File

Tools Used

Conclusion

Further Reading

Analysis Overview
In this lab, the captured PCAP file is analyzed using Wireshark to trace the steps of the attack, including identifying the attacker’s origin, the methods used to exploit vulnerabilities, and the attacker’s actions within the compromised server. By following the steps in this lab, we gain valuable insights into web application vulnerabilities and the forensic techniques used to identify and mitigate similar attacks.

Step-by-Step Analysis
Q1: Identifying the Geographical Origin
Objective: To determine the geographical location of the attacker's IP address.

Open the provided PCAP file in Wireshark.

Navigate to Statistics -> Endpoints.

Under the IPv4 tab, identify the source IP address (e.g., 117.11.88.124) and the destination IP (24.49.63.79).

Use an IP geolocation service like ipgeolocation.io to find the geographical location of the attacker's IP.

Result: The attack originated from Tianjin, China.

Q2: Attacker's User-Agent
Objective: To identify the attacker’s User-Agent for building filtering rules.

Follow the HTTP stream between the attacker and the server.

In Wireshark, right-click on a relevant HTTP packet and select Follow -> HTTP Stream.

Extract the User-Agent string from the captured HTTP request.

Result: The attacker's User-Agent is identified as Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0.

Q3: Malicious Web Shell Identification
Objective: To identify the malicious web shell uploaded by the attacker.

Apply the filter ip.src == 117.11.88.124 and http.request.method=="POST" in Wireshark.

Locate the POST requests where the attacker uploads files.

Review the uploaded file and its content.

Result: The attacker successfully uploaded a web shell named image.jpg.php by bypassing the server's upload validation mechanism.

Q4: Directory for Uploaded Files
Objective: To determine where uploaded files are stored on the server.

Review the HTTP response after the upload request.

Follow the redirection to the directory where the file is stored.

Result: The uploaded files are stored in the /reviews/uploads/ directory.

Q5: Port Used for Outbound Communication
Objective: To identify the port used by the attacker for reverse shell communication.

Inspect the content of the uploaded web shell.

Identify the netcat (nc) command within the PHP script.

Result: The web shell targets port 8080 on the attacker's machine for outbound communication.

Q6: Identifying the Exfiltrated File
Objective: To determine which file the attacker attempted to exfiltrate.

Follow the TCP stream associated with the reverse shell.

Look for commands attempting to access or send sensitive files.

Result: The attacker attempted to exfiltrate the /etc/passwd file, a critical file for system reconnaissance.

Tools Used
Wireshark: A network protocol analyzer used to examine and analyze the captured traffic (PCAP file).

IP Geolocation: For identifying the geographical origin of the attack.

PHP/Netcat: To analyze the malicious web shell code.

Conclusion
This analysis highlights key forensic techniques used to trace an attacker’s actions within a compromised web server. By identifying the geographical origin, attack vectors, and specific files targeted for exfiltration, we can build a more robust defense strategy. Understanding these steps helps strengthen incident response efforts and mitigates future risks associated with similar attacks.

Further Reading
Wireshark Documentation

PCAP Analysis Tutorials

OWASP Web Security Testing Guide

This repository provides a practical approach to incident response and highlights important steps in network forensics. Feel free to fork, contribute, or ask any questions via Issues or Pull Requests.
