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

Proxmox and the lab network are running. OPNsense provides the lab gateway, and the Debian desktop is connected to Wazuh on an Ubuntu server. I completed a controlled sudo authentication-failure test and reviewed the resulting alerts. Windows 11 is now connected to Wazuh and Windows Security event collection is verified. Windows Server and Active Directory are still planned.

[Read the first investigation](investigations/01-sudo-authentication.md).

My second investigation covers file integrity monitoring. Creation, modification, and deletion alerts are verified. [Read the completed investigation](investigations/02-file-integrity.md).

I completed my third investigation by comparing multiple sudo authentication-failure alerts with terminal evidence and documenting why the controlled test could be closed without escalation. [Read Investigation 03](investigations/03-failed-sudo-attempts.md).

I completed Investigation 04 by creating a temporary Linux account, reviewing the user and group alerts, and verifying account cleanup. [Read the investigation](investigations/04-linux-account-creation.md).

I completed Investigation 05 by investigating a Linux group membership change, adding and validating a custom Wazuh detection, testing file access, and verifying cleanup. [Read the investigation](investigations/05-linux-group-membership.md).

I also validated the custom group-membership detection with an alternate-user positive sample and two negative samples. All three tests behaved as expected.


I completed Investigation 06 by comparing controlled Windows authentication failures with event 4625 and Wazuh rule 60122, checking distinct event records, and documenting the limits of the correlation. [Read the investigation](investigations/06-windows-failed-logon.md).

[Windows endpoint setup and evidence](setup/windows-endpoint.md).

I completed Investigation 07 by reviewing Windows account creation and deletion events, identifying the acting and affected accounts, and verifying removal with an endpoint lookup. [Read the investigation](investigations/07-windows-account-creation.md).
