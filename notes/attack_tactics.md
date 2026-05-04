* Based on MITRE ATT&CK v19 (https://attack.mitre.org/tactics/enterprise/)
## 15 Enterprise Tactics

### Reconnaissance
This try to getting more information but do not do anything yet but prepare to do something with information that gathering in this.

### Resource Development
This might similar with Reconnaissance but relate with resources not information such as register new domains, acquire some system for the future activity.

### Initial Access
Attacker gets the first foothold into the target network — phishing, exploit, valid credentials.

### Execution
This activity focus in malicious execution.

### Persistence
This try to maintain the ways that they sit in the system.

### Privilege Escalation
It try to esclate more priviledge that can move to more ability to access the system.

### Stealth
hide and conceal their actions, appearing as normal behavior.

### Defense Impairment
It's doing stop or kill security process that protect the system.

### Credential Access
It might similar with Reconnaissance but this focus in steal but need to understand what different and how they steal.

### Discovery
This activity try to discover what in the system so that to plan and prepare.

### Lateral Movement
This might next step of discovery after find out what in the system and environment then we will move to other.

### Collection
Getting information and assess the values of data or information.

### Command and Control
Attacker maintains a communication channel to send commands to compromised systems.

### Exfiltration
This try to export the data out of system without permit.

### Impact
This might close with Defense Impairment but more focus into system not security perimeters. 


## I'm picking 3 tactics.
### Reconnaissance > Initial Access > Lateral Movement
I pick 3 of them as i think it major of attacks. You need to getting information even small or least privilege but it can be step next level to escalation with Initial Access and find something big and movement in the system.
From defender's perspective, you need to give awareness to your users and everyone in org to understand the security to prevent and make it difficults for reconnaissance also, security baseline that not make it too easy to get the information into protected system. With defense in depth the escalation need some layers or approved mechanism to prevent and detection what will happens when escalation occurs. Also, trust nothing for lateral movement then need validate if they need to move somewhere and some separation of duties.


### T1595 — Active Scanning

**Tactic:** Reconnaissance
**Sub-techniques worth knowing:** T1595.001(Scanning IP Block), T1595.002(Vulnerability Scanning), T1595.003(Wordlist Scanning)
**What it is:** It try to probing and/or scanning to getting information the victim system then they understand what we have and how they will get into.
**Data sources to detect it:** Monitor network data for uncommon data flows. Processes utilizing the network that do not normally have network communication or have never been seen before are suspicious.
Monitor and analyze traffic patterns and packet inspection associated to protocol(s) that do not follow the expected protocol standards and traffic flows (e.g extraneous packets that do not belong to established flows, gratuitous or anomalous traffic patterns, anomalous syntax, or structure). Consider correlation with process monitoring and command line to detect anomalous processes execution and command line arguments associated to traffic patterns (e.g. monitor anomalies in use of files that do not normally initiate connections for respective protocol(s)).
**Real-world example:** In the Triton Safety Instrumented System Attack, TEMP.Veles engaged in network reconnaissance against targets of interest.
**My note:** This a bit tricky with today system open to public so that we are reachable everywhere but to mitigate our own with more privacy in the IP blocks information, up to date patches that system reachable to public and protect own network to use detection or prevention of network scanning.

### T1589 — Gather Victim Identity Information

**Tactic:** Reconnaissance
**Sub-techniques worth knowing:** T1589.001(Credentials), T1589.002(Email Addresses), T1589.003(Employee Names)
**What it is:** It getting identify of victim, it can be username, email address, or name of users. Attackers gather identity info (usernames, emails, names) so they can target individuals later for phishing or credential attacks.
**Data sources to detect it:** Monitor for suspicious network traffic that could be indicative of probing for user information, such as large/iterative quantities of authentication requests originating from a single source (especially if the source is known to be associated with an adversary/botnet). Analyzing web metadata may also reveal artifacts that can be attributed to potentially malicious activity, such as referer or user-agent string HTTP/S fields.
**Real-world example:** FIN13 has researched employees to target for social engineering attacks.
**My note:** Give awareness to our users that do not reuse password or share any identify and protected with MFA. Monitor the geo location that user might login and some un usual behaviors.

### T1566 — Phishing

**Tactic:** Initial Access
**Sub-techniques worth knowing:** T1566.001(Spearphishing Attachment), T1566.002(Spearphishing Link), T1566.003(Spearphishing via Service), T1566.004(Spearphishing Voice)
**What it is:** It phishing victim with human feeling, greedy, fear, etc to force victim to do something without second thought. Then with that we could reachout the system with thier identity.
**Data sources to detect it:** Unusual inbound email activity where attachments or embedded URLs are delivered to users followed by execution of new processes or suspicious document behavior. Detection involves correlating email metadata, file creation, and network activity after a phishing message is received.
**Real-world example:** AppleJeus has used spearphishing emails to distribute malicious payloads.
**My note:** In today world, phishing like scam and much more techniques it can be Spearphishing Video not just voice.

### T1190 — Exploit Public-Facing Application

**Tactic:** Initial Access
**Sub-techniques worth knowing:** No Sub Techniques
**What it is:** Attacker using the public exploit information together with system that facing to public with Vulnerability facing then exploit the system
**Data sources to detect it:** Adversary exploits public admin services on routers/firewalls/switches. Chain: anomalous HTTP/SNMP/SmartInstall inputs → device syslog errors/restarts → config changes/CLI spawn → egress to attacker C2.
**Real-world example:** Volt Typhoon has gained initial access through exploitation of multiple vulnerabilities in internet-facing software and appliances such as Fortinet, Ivanti (formerly Pulse Secure), NETGEAR, Citrix, and Cisco.
**My note:** Keep monitoring the provider security advisory and study our system that hit into that advisory or not and how to apply patches or workaround to mitigate the exploit.

### T1021 — Remote Services

**Tactic:** Lateral Movement
**Sub-techniques worth knowing:** T1021.001(Remote Desktop Protocol), T1021.002(SMB/Windows Admin Shares), T1021.003(Distributed Component Object Model), T1021.004(SSH), T1021.005(VNC), T1021.006(Windows Remote Management), T1021.007(Cloud Services), T1021.008(Direct Cloud VM Connections)
**What it is:** Attackers abuse legitimate remote services (RDP, SSH, SMB) to move between systems. Hard to detect because the protocols are legitimate — the question is whether the use is legitimate.
**Data sources to detect it:** SSH session from new source IP followed by interactive shell or privilege escalation (e.g., sudo, su) and outbound lateral connection.
**Real-world example:** Aquatic Panda used remote scheduled tasks to install malicious software on victim systems during lateral movement actions.
**My note:** Identify general/ normal behavior of remote service in system. Network segmentation and monitor the anomaly network behave.

### T1550 — Use Alternate Authentication Material

**Tactic:** Lateral Movement
**Sub-techniques worth knowing:** T1550.001(Application Access Token), T1550.002(Pass the Hash), T1550.003(Pass the Ticket), T1550.004(Web Session Cookie)
**What it is:** Attackers use stolen tokens, tickets, or hashes to authenticate as another user without needing the password. Modern variant: stolen API tokens and OAuth cookies.
**Data sources to detect it:** Use of stolen Kerberos tickets or token impersonation resulting in logon sessions from accounts without expected interactive logon events.
**Real-world example:** FoggyWeb can allow abuse of a compromised AD FS server's SAML token.
**My note:** Follow with provider security advisor which protocol end of life also understand own behavior with network segmentation.


## Day 2 Reflection

**What clicked:**
- Tactics can be more granular more techniques, we need to update and monitor what going on in our today world with more fast changes and update but most of tactics might cover but techniques can be update quickly follow with fast changes.

**What surprised me:**
- I do not surprise in this but interesting with the covering in tactics and techniques mitre have as Framework.

**What I want to learn next:**
- As you suggest sigma rules or rules of compromise that we follow with tactics and techniques identify.