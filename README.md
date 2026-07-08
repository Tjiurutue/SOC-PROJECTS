# SOC-PROJECTS
SIEM Proof of Concept — Wazuh Threat Detection Lab

A hands-on Security Information and Event Management (SIEM) deployment built in a virtualized lab environment, simulating a realistic small-business network (Active Directory, mail server, FTP server, and endpoint clients) with live attack detection and incident response.

Overview

This project demonstrates end-to-end SOC analyst skills: standing up a monitored network from scratch, deploying a SIEM platform, generating real attack traffic against it, and walking through detection → investigation → response for each scenario.

Built for: Applied Ethical Hacking coursework (NUST) — extended here as a standalone portfolio project.

Architecture

All systems run on a single 192.168.10.0/24 subnet inside VMware Workstation:

SystemRoleOSWazuh SIEM ServerCentral log collection, indexing, dashboardUbuntu 24.04 LTSDomain ControllerActive Directory, DNS, FTP (IIS)Windows Server 2016Mail ServeriRedMail (Postfix, Dovecot, Roundcube, Nginx)Ubuntu 22.04 LTSWindows ClientDomain-joined endpoint with Wazuh agentWindows 10Linux ClientDomain-adjacent endpoint with Wazuh agentUbuntu 22.04 LTSAttacker MachineSimulated threat actor (Hydra, Metasploit, Nmap)Kali Linux

All endpoints forward logs to the Wazuh Manager for centralized monitoring and real-time alerting.

What Was Built


Active Directory domain (nec.local) with organizational units for IT, HR, and Marketing departments, and domain-joined Windows 10 client
FTP server on IIS with department-segmented directories and access permissions
Mail server (iRedMail) with Postfix, Dovecot, Roundcube webmail, and Fail2Ban intrusion prevention
Wazuh SIEM deployed as the central log aggregator, ingesting logs from Windows Security Event Logs, IIS FTP logs, mail logs (Postfix/Dovecot/Fail2Ban/Nginx), and Linux auth logs across all endpoints


Attack Simulation & Detection

Three attack scenarios were run from the Kali Linux attacker machine to validate detection coverage:

ScenarioToolTargetResultBrute-force credential attackHydra / MetasploitAD Domain Controller (SMB/RDP/LDAP)Detected in real time — multiple failed authentication alerts (Event ID 4625), High severityUnauthorized FTP accessHydra FTP moduleFTP Server (port 21)Detected in real time — FTP authentication failure alerts, Medium severityPhishing email w/ malicious attachmentSimulated payloadEmployee mailboxDetected and quarantined via ClamAV + Amavis, logged and alerted, High severity

Incident Response Actions Taken

For each detected attack, a full response cycle was executed and documented:


Attacker IP identified from Wazuh alert data and blocked at the firewall
Account lockout policy enabled after repeated failed logins
Affected credentials reset
FTP anonymous access disabled and directory permissions tightened
Malicious email sender blacklisted at the mail server


Skills Demonstrated

SIEM deployment & tuning Log source integration Windows Event Log analysis Active Directory administration IIS/FTP configuration Linux mail server administration (Postfix/Dovecot) Offensive tooling (Hydra, Metasploit, Nmap) Incident response workflow Security documentation & reporting

Security Recommendations Delivered

As part of the writeup, the following prioritized recommendations were produced for hardening the simulated environment:


Critical: Multi-Factor Authentication, TLS encryption for all email traffic
High: Regular patch management, network segmentation (VLANs), IDS deployment
Medium: Security awareness training, backup/disaster recovery testing
Low: Data Loss Prevention (DLP)


Notes on This Repository

This is a defensive security lab exercise conducted in an isolated virtual environment for educational purposes. All credentials shown during setup were lab-only defaults and have been redacted from this repository. No real production systems, external targets, or public infrastructure were involved.
