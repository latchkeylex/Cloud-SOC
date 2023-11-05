# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://docs.google.com/drawings/d/e/2PACX-1vRtlKH0P7f0eVHMY0arkZKuVXP2ekRCIq6Xb4DQFf5VNVYlC0-VlYjpkIjI6IdAPA6fzdw5taiHA7QH/pub?w=960&h=720)

## Introduction

In this project, I established a small-scale honeynet within the Azure cloud environment. I gathered log data from diverse sources and channeled it into a Log Analytics workspace. Microsoft Sentinel then leveraged these logs to construct attack visualizations, trigger security alerts, and manage security incidents.

To assess the security posture, I conducted a 24-hour measurement of security metrics within the initial, less secure environment. Subsequently, I applied a series of security controls to fortify the environment and performed another 24-hour metric assessment. The security metrics under examination include..

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://docs.google.com/drawings/d/e/2PACX-1vTNDFS8mB600xQscPBKko1Tq8E8sACNRO2T0oPj6CeiY4HSXI5roLc4xC1uFquIbHi2Sv1adJCrqJSm/pub?w=960&h=720)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://docs.google.com/drawings/d/e/2PACX-1vRj1nqSGR_cL78m8iJbB0kGA5emIsScTkkZlEb3vQGTP97XR3ib8_1qSeBRHkoS-dDelEU0gL2HwCVV/pub?w=960&h=720)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 kali linux)
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
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, we established a mini honeynet within the Microsoft Azure environment to enhance security monitoring. We integrated various log sources into a dedicated Log Analytics workspace. Microsoft Sentinel was employed to implement a proactive security approach, utilizing ingested logs to trigger alerts and manage security incidents effectively.

We conducted initial security metrics assessments within an environment that lacked robust security controls, and then subsequently assessed security metrics after implementing enhanced security measures. It's noteworthy that the post-improvement assessment revealed a significant reduction in the number of security events and incidents, underscoring the efficacy of the security controls we implemented.

Additionally, it's important to consider that if the network resources were under heavier regular user utilization, the 24-hour post-security control period might have generated a higher volume of security events and alerts, but our interventions effectively mitigated these potential risks.
