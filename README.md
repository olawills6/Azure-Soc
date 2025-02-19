# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I created a mini honeynet in Microsoft Azure and ingest log sources from various resources and Diagnostic Setting was used to forward them to Log Analytics workspace. Then, Microsoft Sentinel was used to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, then applied some security controls to harden the environment. I measured the metrics for another 24 hours and show the results below. The metrics shown are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

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
![NSG Allowed Inbound Malicious Flows]![image](https://github.com/olawills6/Azure-Soc/assets/145821831/ac716c82-eabc-4cde-a048-9966532aa6b6)<br>
![Linux Syslog Auth Failures]![image](https://github.com/olawills6/Azure-Soc/assets/145821831/6db7031a-580e-4b37-893c-e1c212eed6e2)<br>
![Windows RDP/SMB Auth Failures]![image](https://github.com/olawills6/Azure-Soc/assets/145821831/95cabed4-05f8-421a-8808-0a4fc9979985)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-03-13 14:33:02
Stop Time 2024-03-14 14:33:02

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 25480
| Syslog                   | 1983
| SecurityAlert            | 6
| SecurityIncident         | 121
| AzureNetworkAnalytics_CL | 1319

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-03-15 14:49:09
Stop Time	2024-03-16 14:49:09

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 858
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was created in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on the ingested logs. Also, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is worth mentioning that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth mentioning that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
