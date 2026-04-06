# Active-Directory-Brute-Force-Detection-Lab
# Project Overview
This project demonstrates the configuration, simulation, and investigation of a brute-force attack in a Windows Active Directory environment.
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
Verified using:
    net accounts
# Attack Simulation
# Simulated brute-force attack from Windows 10 client:
    .Attempted login with incorrect password 5 times
    .Triggered account lockout
    .Generated security events on Domain Controller
# Event Log Analysis
# Event ID 4771 — Kerberos Pre-Authentication Failed
    .Status 0x18 → Bad password
    .Status 0x12 → Account disabled/locked
    .Captured source IP address of attacking machine
Event ID 4740 — Account Locked Out
Confirmed lockout triggered after threshold was reached.
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
# This project demonstrates hands-on experience with:
    .Windows Server administration
    .Defensive security controls
    .Authentication monitoring
    .Blue team investigation techniques
