# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/VincentlBrooks/SEC-Analyst/assets/158189609/bbfb4c51-4492-48ab-b78d-81ce1abf0960)





## Introduction

In this project, a mini honeynet was developed within Azure, integrating log sources from diverse resources into a Log Analytics workspace. This setup feeds into Microsoft Sentinel, enabling the construction of attack maps, activation of alerts, and generation of incidents. Over an initial 24-hour period, key security metrics were monitored in an unsecured environment. Following the application of various security controls to enhance the environment's robustness, these metrics were recorded again for another 24 hours. The comparative results are presented below, focusing on several critical metrics:

SecurityEvent: Capturing Windows Event Logs.
Syslog: Documenting Linux Event Logs.
SecurityAlert: Tracking alerts triggered in Log Analytics.
SecurityIncident: Detailing incidents created by Sentinel.
AzureNetworkAnalytics_CL: Analyzing malicious network flows permitted into the honeynet.

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
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 20470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-01-18 15:37
Stop Time	2024-01-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 5693
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was established within Microsoft Azure, with log sources seamlessly integrated into a Log Analytics workspace. Utilizing Microsoft Sentinel, the system was set to generate alerts and incidents in response to the incoming log data. A comparative analysis of security metrics was conducted before and after the implementation of enhanced security controls. The results clearly indicated a significant reduction in the number of security events and incidents post-implementation, underscoring the efficacy of these measures.

It should be noted that under conditions of high resource utilization by regular network users, the likelihood of observing an increased volume of security events and alerts within the 24-hour window after applying the security controls could be higher.
