# Active-Directory-Brute-Force-Detection-Lab
# Executive Summary
This project simulates a real-world brute-force attack against a Windows Active Directory environment and demonstrates how such activity can be detected and investigated using native Windows Security Event logs.
A Domain Controller and domain-joined Windows 10 client were deployed in a controlled lab environment. An account lockout policy was configured to enforce defensive controls. Multiple failed Kerberos authentication attempts were generated to simulate a brute-force attack, leading to account lockout.
Security events were analyzed to reconstruct the attack timeline and identify the source system.
This project demonstrates hands-on blue team skills in authentication monitoring, log analysis, and incident correlation within an enterprise Active Directory environment.
# The objective was to:
    1. Deploy a Windows Server Domain Controller
    2. Join a Windows 10 client to the domain
    3. Configure Account Lockout Policy
    4. Simulate failed authentication attempts
    5. Investigate security events using Windows Event Viewer
    6. Analyze Kerberos authentication failures
    7. Correlate lockout events
This lab simulates real-world SOC investigation procedures.
# Lab Architecture
Environment: Hyper-V (On-Prem Lab)
   # Machine    Role                      IP Address
    DC01      Domain Controller          192.168.100.10
    WIN10     Domain-Joined Client       192.168.100.20
Domain Name: corp.local
<img width="1366" height="768" alt="Domain Controller promoted" src="https://github.com/user-attachments/assets/15826a9d-fe6b-4bb3-b20c-49190435027f" />

# Active Directory Configuration
    .Installed Active Directory Domain Services (AD DS)
    .Promoted DC01 to Domain Controller
    .Created Organizational Units (OUs)
    .Created domain users (e.g., john.admin)
    .Configured static IP addressing
    .Verified domain connectivity
# Security Hardening Implemented
Account Lockout Policy
# Configured via Default Domain Policy:
    .Lockout Threshold: 5 failed attempts
    .Lockout Duration: 15 minutes
    .Reset Counter After: 15 minutes
<img width="1366" height="768" alt="Account Lockout Policy" src="https://github.com/user-attachments/assets/954dab1c-8f52-4def-b900-cdd4e0a9e344" />

# Verified using:
    net accounts
<img width="1366" height="768" alt="net accounts output" src="https://github.com/user-attachments/assets/0a60ada9-5f9c-499a-9fd7-2e84c10de127" />

# Attack Simulation
## Simulated brute-force attack from Windows 10 client:
    .Attempted login with incorrect password 5 times
    .Triggered account lockout
    .Generated security events on Domain Controller
# Event Log Analysis
## Event ID 4771 — Kerberos Pre-Authentication Failed
    .Status 0x18 → Bad password
    .Status 0x12 → Account disabled/locked
    .Captured source IP address of attacking machine
<img width="1366" height="768" alt="Event ID 4771" src="https://github.com/user-attachments/assets/71ba80a1-3cdc-46b4-90b3-9742644a4399" />

Event ID 4740 — Account Locked Out
Confirmed lockout triggered after threshold was reached.
<img width="1366" height="768" alt="Event ID 4740" src="https://github.com/user-attachments/assets/898f785d-99f4-4ff5-82f0-b5fe403c134c" />

# Investigation Process
    .Filtered Security logs for Event ID 4771
    .Identified repeated failed attempts
    .Correlated timestamp with Event ID 4740
    .Extracted source IP address
    .Reconstructed attack timeline
# Skills Demonstrated
    .Active Directory deployment
    .Group Policy configuration
    .Windows Security Event analysis
    .Kerberos authentication investigation
    .Account lockout enforcement
    .SOC-style incident correlation
# Future Improvements
    .Forward logs to Microsoft Sentinel
    .Create KQL detection rules
    .Simulate password spraying
    .Simulate lateral movement
    .Develop automated alerting
# Key Takeaways
## This project demonstrates hands-on experience with:
    .Windows Server administration
    .Defensive security controls
    .Authentication monitoring
    .Blue team investigation techniques
# Incident Report
## Multiple Kerberos Authentication Failures Leading to Account Lockout
### Severity
Medium (Credential Attack Simulation)
### Summary
A series of failed Kerberos pre-authentication attempts were detected against the domain account john.admin. After five consecutive failed login attempts, the domain account lockout policy was triggered, generating a lockout event on the Domain Controller.
### Detection Method
    .Event ID 4771 — Kerberos Pre-Authentication Failed (Status 0x18 – Bad Password)
    .Event ID 4740 — Account Locked Out
### Source of Activity
    .Client IP: 192.168.100.20
    .Machine: Windows 10 Domain-Joined Workstation
### Root Cause
Simulated brute-force authentication attempts using invalid credentials.
### Containment
Account automatically locked after 5 failed attempts per configured domain policy.
### Lessons Learned
    .Account lockout policies effectively mitigate brute-force attempts.
    .Kerberos Event ID 4771 provides valuable insight into authentication failures.
    .Correlating 4771 and 4740 events enables clear reconstruction of attack timelines.
    
