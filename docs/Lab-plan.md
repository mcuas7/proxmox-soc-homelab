# SOC Home Lab Plan

I'm building this lab to practice the work I would do as a SOC analyst: reviewing alerts, checking logs, investigating activity, and writing down what I found.

## Build steps

1. Install Proxmox VE on the HP EliteDesk.
2. Create an isolated network for the lab.
3. Set up Windows systems for Active Directory and endpoint activity.
4. Add a security monitoring platform and connect the systems to it.
5. Generate safe test activity and investigate the resulting alerts.
6. Document each investigation, including the evidence I reviewed and my conclusion.

## What I'll document

As I complete each step, I'll add the configuration, screenshots, problems I ran into, and how I resolved them. I'll keep practice scenarios separate from any real activity on my home network.

## Current status

Proxmox and the lab network are running. OPNsense provides the lab gateway, and the Debian desktop is connected to Wazuh on an Ubuntu server. I completed a controlled sudo authentication-failure test and reviewed the resulting alerts. Windows and Active Directory are still planned.

[Read the first investigation](investigations/01-sudo-authentication.md).

My second investigation covers file integrity monitoring. Creation, modification, and deletion alerts are verified. [Read the completed investigation](investigations/02-file-integrity.md).

I completed my third investigation by comparing multiple sudo authentication-failure alerts with terminal evidence and documenting why the controlled test could be closed without escalation. [Read Investigation 03](investigations/03-failed-sudo-attempts.md).
