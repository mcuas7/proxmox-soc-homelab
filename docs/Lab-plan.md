# SOC Home Lab Plan

I'm building this lab to practice the work I would do as a SOC analyst: reviewing alerts, checking logs, investigating activity, and writing down what I found.

## Build steps

1. Install Proxmox VE on the lab host.
2. Create an isolated network for the lab.
3. Set up Windows systems for Active Directory and endpoint activity.
4. Add a security monitoring platform and connect the systems to it.
5. Generate safe test activity and investigate the resulting alerts.
6. Document each investigation, including the evidence I reviewed and my conclusion.

## What I'll document

As I complete each step, I'll add the configuration, screenshots, problems I ran into, and how I resolved them. I'll keep practice scenarios separate from any real activity on my home network.

## Current status

Proxmox hosts OPNsense, a Debian endpoint, an Ubuntu Wazuh server, and a Windows 11 Enterprise Evaluation endpoint. Both endpoints are enrolled in Wazuh, and Linux and Windows Security events are reaching the SIEM.

## Completed investigations

1. [Failed sudo authentication](investigations/01-sudo-authentication.md)
2. [File integrity monitoring](investigations/02-file-integrity.md)
3. [Multiple failed sudo authentication attempts](investigations/03-failed-sudo-attempts.md)
4. [Linux account creation](investigations/04-linux-account-creation.md)
5. [Linux group membership and custom detection](investigations/05-linux-group-membership.md)
6. [Windows failed authentication](investigations/06-windows-failed-logon.md)
7. [Windows account creation and deletion](investigations/07-windows-account-creation.md)
8. [Windows Administrators group membership](investigations/08-windows-admin-membership.md)

The custom Linux detection includes a verified live alert and positive and negative validation samples. [Windows endpoint setup](setup/windows-endpoint.md) documents enrollment and event collection.

## Next steps

- Add Sysmon and practice process and PowerShell investigations.
- Build Windows Server and Active Directory, then join the Windows endpoint to the domain.

These steps remain pending. My focus is to explain the evidence, authorization, impact, and cleanup for each investigation before adding more tools.
