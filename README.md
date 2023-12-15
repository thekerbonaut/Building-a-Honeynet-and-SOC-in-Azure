# Building a Honeynet and SOC in Azure (With Live Traffic Analysis)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

For this project, I constructed a small honeynet in Azure and collected log data from several sources into a Log Analytics workspace. This workspace is subsequently utilized by Microsoft Sentinel to generate attack maps, activate alerts, and generate incidents. I conducted security metric assessments in an unsecure environment for a duration of 24 hours. Subsequently, I implemented security measures to strengthen the environment and conducted another 24-hour assessment of the metrics. The findings of these assessments are presented below. The metrics we will display are:

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

Initially, to establish "BEFORE" metrics, all resources were installed and made accessible to the internet. The Virtual Machines had their Network Security Groups and built-in firewalls configured with unrestricted access, allowing all traffic. Additionally, all other resources were deployed with public endpoints that are accessible from the Internet. As a result, there is no need for Private Endpoints.

To collect the "AFTER" metrics, Network Security Groups were strengthened by implementing a policy that blocks ALL traffic, save for my admin workstation. Additionally, all other resources were safeguarded by their inherent firewalls and Private Endpoint.


## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](INSERTPICTURE)<br>
![Linux Syslog Auth Failures](INSERTPICTURE)<br>
![Windows RDP/SMB Auth Failures](INSERTPICTURE)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 11/27/2023 9:58:10
Stop Time 11/28/2023 9:58:10

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 39918
| Syslog                   | 5746
| SecurityAlert            | 4
| SecurityIncident         | 281
| NSGInboundMaliciousFlows | 2822

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 12/6/2023 15:35:19
Stop Time  12/7/2023 15:35:19

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8102
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| NSGInboundMaliciousFlows | 0

## Conclusion

A miniature honeynet was built within the Microsoft Azure platform, and several log sources were included into a Log Analytics workspace. Microsoft Sentinel was utilized for triggering alerts and to generate incidents based on the ingested data. In addition, metrics were assessed in the insecure environment before the implementation of security measures, and subsequently reevaluated following the implementation of security controls. The implementation of security measures resulted in a significant decrease in the number of security events and incidents, thereby indicating their efficacy.
It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
