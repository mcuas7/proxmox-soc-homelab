# Proxmox SOC Home Lab

## About This Project

I currently work in IT support and I'm working toward transitioning into a cybersecurity/SOC Analyst role.

I created this home lab to get more hands-on experience with the tools and day-to-day tasks that a SOC Analyst may work with, such as reviewing alerts, analyzing logs, working with Windows security events, and investigating suspicious activity.

I'm using Proxmox as the foundation of the lab so I can create multiple virtual machines and build a small enterprise-style environment without needing several physical computers.

This repository will document the project as I build it, including the setup process, problems I run into, what I learn, and eventually some SOC investigation exercises.

**Current Status:** Work in Progress

---

## Why I'm Building This

I've spent most of my professional IT experience working in technical support. While studying cybersecurity has helped me understand many security concepts, I wanted an environment where I could actually apply them.

My goal with this project is to get more comfortable with things like:

- Investigating security alerts
- Reading and understanding logs
- Windows Event Viewer
- Active Directory
- SIEM tools
- Endpoint monitoring
- Basic network analysis
- Documenting an investigation
- Understanding false positives
- Knowing when an alert should be escalated

I also want to become more comfortable explaining how I approached an investigation instead of simply memorizing cybersecurity concepts.

---

## Lab Hardware

The lab will run on an HP EliteDesk Mini with:

- 32 GB RAM
- 1 TB SSD
- Proxmox VE

I already have a separate Debian server running Nextcloud, so I'm keeping the SOC lab separate from my main storage server.

---

## Planned Lab

My initial plan is to build the following environment:

    Proxmox VE
        |
        +-- AdGuard Home
        |
        +-- Uptime Kuma
        |
        +-- Windows Server
        |      |
        |      +-- Active Directory
        |      +-- DNS
        |
        +-- Windows 11 Workstation
        |
        +-- Linux Server
        |
        +-- Wazuh
               |
               +-- Windows logs
               +-- Linux logs
               +-- Security alerts

The design may change as I learn more and continue building the lab.

---

## Tools and Technologies

Some of the technologies I plan to work with are:

- Proxmox VE
- Windows Server
- Active Directory
- Windows 11
- Linux
- Wazuh
- Sysmon
- PowerShell
- AdGuard Home
- Uptime Kuma
- MITRE ATT&CK

I'm expecting this list to grow as the project develops.

---

## SOC Skills I Want to Practice

One of my main goals is to understand what actually happens after a security alert appears.

Instead of just generating alerts, I want to practice the full investigation process:

    Alert
      |
      v
    Review the alert
      |
      v
    Gather information
      |
      v
    Check logs and endpoint activity
      |
      v
    Build a timeline
      |
      v
    Determine what happened
      |
      v
    Decide if the activity is benign or suspicious
      |
      v
    Document the investigation

I also want to practice writing investigation notes that another analyst could understand.

---

## Planned Investigations

Once the environment is running, I plan to create several safe scenarios inside the lab and investigate the resulting activity.

Some of the scenarios I want to work through include:

- Multiple failed login attempts
- Suspicious PowerShell activity
- User account or group membership changes
- Unusual network connections
- Endpoint security alerts
- File changes
- Authentication activity

For each investigation, I plan to document what triggered the alert, what logs I checked, what evidence I found, and how I reached my conclusion.

---

## Project Progress

### Proxmox Setup

- [ ] Install Proxmox
- [ ] Configure networking
- [ ] Configure storage
- [ ] Create the lab network

### Basic Services

- [ ] Install AdGuard Home
- [ ] Install Uptime Kuma

### Windows / Active Directory Lab

- [ ] Install Windows Server
- [ ] Configure Active Directory
- [ ] Create test users
- [ ] Install Windows 11
- [ ] Join Windows 11 to the domain

### SOC Environment

- [ ] Install Wazuh
- [ ] Install Wazuh agents
- [ ] Install Sysmon
- [ ] Collect Windows logs
- [ ] Collect Linux logs
- [ ] Verify alerts are reaching the SIEM

### SOC Practice

- [ ] Investigation #1
- [ ] Investigation #2
- [ ] Investigation #3
- [ ] Investigation #4
- [ ] Investigation #5

---

## What I'm Learning

I'll update this section throughout the project with things that I learn, problems I encounter, and how I fixed them.

I expect troubleshooting to be a big part of this project, so I want to document the mistakes and fixes instead of only showing the finished environment.

---

## Future Ideas

Once the basic SOC environment is working, I'd like to explore:

- Microsoft Sentinel
- Microsoft Defender
- Vulnerability scanning
- Network segmentation
- Detection rules
- Threat hunting
- Additional Windows endpoints
- Additional Linux systems

---

## Security

This is an isolated home lab used for learning and testing.

I will not include passwords, API keys, authentication tokens, personal information, or other sensitive information in this repository.

Any screenshots added to the project will be reviewed and sanitized before being uploaded.

---

## Disclaimer

Everything in this repository is being performed in my own lab environment for educational and defensive cybersecurity purposes.
