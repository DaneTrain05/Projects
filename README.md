# Build a Honeynet and SOC in Azure with Real-World Cyber Attacks
![Azure Honeynet](https://github.com/DaneTrain05/Projects/assets/48533052/1a39287f-1f3e-4d7f-9adb-d091114201be)

## Summary

In this project, a honeynet and Security Operations Center (SOC) were constructed within Azure. The goal was to capture, analyze, and consolidate logs within a Log Analytics Workspace. Microsoft Sentinel was used to read the logs, generate attack maps, trigger alerts, and create incidents. After the honeynet was created, the first phase was using an unsecured version of the environment and exposing it to the internet for a 24-hour period. After the first phase, several security controls were implemented to harden the environment. Once this was done, the second phase started where we again exposed the environment to the internet for 24 hours. The project ended when both 24-hour periods were logged and compared. 

Metrics analyzed were:
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into the honeynet)

## Technologies, Components, and Regulations
- Azure Virtual Network (VNet)
- Azure Network Security Groups (NSG)
- Virtual Machines (2 Windows VMs and 2 Linux VMs)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final) for Security Controls
- [NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling

## Architecture Before Hardening / Controls
![Architecture Before Hardening](https://github.com/DaneTrain05/Projects/assets/48533052/afaf8dc4-564c-4816-965d-4885eb970258)



This phase monitored the attack patterns used by bad actors in a "real-world" setting.

This phase began with the virtual environment being exposed to the public internet with minimal security controls. The lack of controls allowed bad actors to discover the environment and cyber attacks. The environment consisted of 2 Windows 10 Virtual Machines with a SQL database, 2 Linux Virtual Machines, a blob storage account, and a key vault. Having all of these components increased the attack surface and allowed us to monitor the methods used by attackers.

To ensure the environment was discoverable, the virtual machines had firewalls disabled and the network security groups (NSGs) were configured to allow all inbound traffic. The storage account and key vault were set up with public endpoints visible to the internet. Log Analytics Workspace was set up for log aggregation and Sentinel was used for incident creation. Workbooks with custom queries were created in Microsoft Sentinel to plot the malicious activity on a world map, which provided a visual aid. 

The results can be found in the following section.

## Attack Maps Before Hardening / Security Controls

<b>This attack map shows attempts by bad actors to brute force into the Windows Virtual Machine via Remote Desktop Protocol:</b>
![Windows RDP Auth Fail](https://github.com/DaneTrain05/Projects/assets/48533052/24dd6c8b-15b2-497a-8486-f7eac547b653)



<b>This attack map shows the attempts by bad actors to brute force into the Linux Virtual Machine via Secure Shell:</b>
![Linux SSH Auth Fail](https://github.com/DaneTrain05/Projects/assets/48533052/aa9b3482-fb24-4e6a-be76-ff8de9595e6e)



<b>This attack map shows traffic allowed by Network Security Groups configured to allow all inbound traffic:</b>
![Malicious Allowed In](https://github.com/DaneTrain05/Projects/assets/48533052/aa3c004b-903b-45c9-95d0-117d03c18148)



<b>This attack map shows attempts by bad actors to brute force into the SQL Database:</b>
![Mssql auth fail](https://github.com/DaneTrain05/Projects/assets/48533052/cf215957-88ae-4ab5-a3a7-cabb38abdd13)


## Metrics Before Hardening / Security Controls

The following table shows the metrics measured within the unsecured environment over a 24-hour period:


| Metric                                                               | Count
| -------------------------------------------------------------------- | -----
| SecurityEvent (Windows Event Logs)                                   | 24062
| Syslog (Linux Event Logs)                                            | 4527
| SecurityAlert (Log Analytics Alerts Triggered)                       | 2
| AzureNetworkAnalytics_CL (Malicious flows allowed into the honeynet) | 6408

## Architecture After Hardening / Security Controls
![Architecture After Hardening](https://github.com/DaneTrain05/Projects/assets/48533052/38796d18-b6db-4bb7-8bb5-c230f1ef9d3d)

The purpose here was to harden the environment and implement security controls. We based these controls on regulations to ensure compliance with NIST SP 800-53 Revision 5 SC-7(3) Access Points. 

Hardening included:
- Network Security Groups (NSGs)
  - NSGs were hardened by blocking all inbound and outbound traffic, with the exception of certain public IP addresses that required access to the virtual machines. This made sure that only authorized traffic from trusted sources were allowed to access the virtual machines.

- Built-in Firewalls
  - Azure's built-in firewalls were configured on the virtual machines to restrict unauthorized access and protect resources from malicious connections. This involved fine-tuning the firewall rules based on the service and responsibilities of each virtual machine. This shrunk the attack surface of the environment.

- Private Endpoints
  - For the Azure Key Vault and Storage Containers, Public Endpoints were replaced with Private Endpoints. This ensured access to these resources was limited to the virtual network and not the public internet.

## Attack Maps After Hardening / Security Controls

```All map queries returned no results. This is because there was no malicious activity for the 24-hour testing period.```

## Conclusion

For this project, a small honeynet was built in Microsoft Azure and Log Analytics Workspace was used to ingest logs from the environment's resources. Microsoft Sentinel was configured to trigger alerts and create incidents based on the logs. Several metrics were recorded in order to compare the effectiveness of hardening and security controls. The reduction in events is largely due to not simulating regular user activity. In a real network, users would likely generate security events and alerts to some degree, even if these are false positives.


## Reflection
This project had me use my foundational knowledge and put it into practice. Configuring the honeynet and implementing a small-scale SOC gave me hands-on experience with a variety of tools. Going forward, I plan to do this project again with the purpose of doing a deeper dive into Incident Response and implementing additional NIST 800-53 controls. I would also like to keep the virtual machines running for a lot longer to gather more data.
