# Building a SOC + Honeynet in Azure (Live Traffic)
![Honeynet](https://github.com/JordanDanielWest/Cloud-SOC/assets/96628562/e35e6ea8-5f1e-47ea-8afd-f3b995c84a27)



## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Before Hardening](https://github.com/JordanDanielWest/Cloud-SOC/assets/96628562/c4031d7d-d492-4480-a7cf-7f02ff28ce82)



## Architecture After Hardening / Security Controls
![Architecture After Hardening](https://github.com/JordanDanielWest/Cloud-SOC/assets/96628562/1ec7ebda-dcd3-49b8-8560-e65700579315)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
[NSG Allowed Inbound Malicious Flows]![nsg-malicious-allowd-in_unsecured](https://github.com/JordanDanielWest/Cloud-SOC/assets/96628562/79cc536d-03e5-4a7e-a8c9-7a923ea5e391)
<br>
[Linux Syslog Auth Failures]![linux-ssh-auth-fail_unsecured](https://github.com/JordanDanielWest/Cloud-SOC/assets/96628562/0a54e93e-359e-425f-888d-a5800c64b723)
<br>
[Windows RDP/SMB Auth Failures]![windows-rdp-auth-fail_unsecured](https://github.com/JordanDanielWest/Cloud-SOC/assets/96628562/1d8d85c9-f138-420c-86f9-0814b9fb8c6f)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
- Start Time 2024-02-22 15:24:33
- Stop Time 2024-02-23 15:24:33

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10857
| Syslog                   | 29993
| SecurityAlert            | 0
| SecurityIncident         | 238
| AzureNetworkAnalytics_CL | 0

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
- Start Time 2024-02-24 13:46:57
- Stop Time	2024-02-25 13:46:57

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9129
| Syslog                   | 7
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
