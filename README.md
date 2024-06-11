# Building a SOC + Honeynet in Azure (With Live Traffic)

## Introduction

In this project, I created a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace. This data was utilized by Microsoft Sentinel to construct attack maps, trigger alerts, and generate incidents. I measured several security metrics in the insecure environment over a 24-hour period, implemented security controls to harden the environment, and then measured the metrics again for another 24 hours. The results are presented below. The metrics we will display include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/XFrTKek.png)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/kHxCOee.png)

The architecture of the mini honeynet in Azure includes the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were initially deployed and exposed to the internet. The Virtual Machines had unrestricted Network Security Groups and built-in firewalls, and all other resources were deployed with public endpoints accessible from the internet, without utilizing Private Endpoints.

For the "AFTER" metrics, the Network Security Groups were hardened by blocking all traffic except from my admin workstation. Additionally, all other resources were secured using their built-in firewalls and Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/jLQeDn7.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/ZlhOZKR.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/k0Sxxee.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics measured in the insecure environment for 24 hours:

Start Time 2024-06-08 18:01:02

Stop Time 2024-06-09 18:01:02

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 25587
| Syslog                   | 1751
| SecurityAlert            | 0
| SecurityIncident         | 244
| AzureNetworkAnalytics_CL | 2091

## Attack Maps Before Hardening / Security Controls

```All map queries returned no results due to the absence of malicious activity during the 24-hour period following the hardening process.```

## Metrics After Hardening / Security Controls

The following table shows the metrics that were measured in the environment for another 24 hours, but after Security Controls were applied:

Start Time 2024-06-10 13:35

Stop Time	2024-06-11 13:36

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9100
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to trigger alerts and create incidents based on the ingested logs. Metrics were measured in the insecure environment before applying security controls, and then again after implementing security measures. The results demonstrated a significant reduction in security events and incidents following the application of these controls, highlighting their effectiveness.

It is important to note that if the network resources had been heavily utilized by regular users, more security events and alerts might have been generated within the 24-hour period after the implementation of the security controls.
