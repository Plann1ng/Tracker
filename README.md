# Network Troubleshooting & Operational Changes

This repository contains a curated list of real-world networking, security, and infrastructure issues I have worked on, documented in a **Problem / Solution** format.
**NOTE**: All operations mentioned below have been documented with RCA and Solution and preversed following the company's policy as future guide and reference.

---

## VPN Certificate Prompt During Windows 11 OOBE

**Problem:**
During Windows 11 OOBE enrollment via Intune, Cisco Secure Client was preinstalled but on first VPN connection users were prompted to manually select a certificate. The VPN client was using a default local profile instead of the intended custom XML profile, and profile changes on the XDR side appeared to have no effect on newly enrolled devices.

**Solution:**
The issue was caused by a static XDR deployment package being converted to a .intune file and reused in Intune. Changes to the XDR deployment were not applied until the package was rebuilt, reconverted, and replaced in Intune. After correcting this, VPN connectivity still failed due to Secure Endpoint lacking access to the device certificate store, likely because of incorrect local permissions during enrollment. As a workaround, authentication was switched from device certificate validation to cloud-based verification using Umbrella device data, restoring VPN connectivity for new devices.


## Cisco Secure Mail – CLI Access

**Problem:**
Administrative troubleshooting and automation were limited due to missing CLI access on the cloud-based Cisco Secure Mail environment.

**Solution:**
Enabled and validated CLI access on the cloud-based Cisco Secure Mail instance to allow advanced troubleshooting and administrative operations in cooperation with Cisco TAC using PKI components.

---

## Catalyst Center Not Joining Network

**Problem:**
A newly deployed Catalyst Center appliance could not join the network. Routing, ARP, firewall rules, packet captures, MAC learning, re-imaging, and version upgrades did not resolve the issue.

**Solution:**
After exhausting all software and network troubleshooting steps, the appliance was replaced via RMA. The issue was confirmed to be a hardware failure despite the device being brand new.

---

## VLAN Migration Automation for Dell Devices

**Problem:**
During network segmentation, only switch ports connected to Dell laptops needed to be migrated from VLAN 3 to VLAN 3010, including cases where devices were not connected at execution time.

**Solution:**
Developed automation that logs into switches, checks MAC address tables, validates vendor, VLAN, and port mode, then changes the VLAN and logs the previous configuration. A second workflow listens for syslog interface-up traps to handle devices connecting after execution.

**NOTE**:Code can be found at [Network Automation Scripts](https://github.com/Plann1ng/Dynamic-VLAN-Automation/blob/main/README.md)


---

## Site-to-Site VPN Modification

**Problem:**
A municipality site-to-site VPN required modification to support an additional jump host.

**Solution:**
Corrected and extended the site-to-site tunnel configuration to support the new jump host while maintaining existing connectivity.

---

## PDART Report for Catalyst Center Integration

**Problem:**
Catalyst Center integration required a PDART report generated from Prime.

**Solution:**
Generated and validated the PDART report from Prime to complete Catalyst Center integration requirements.

---

## Wi-Fi Drops After Controller Migration

**Problem:**
After migrating from Aire-OS to a 9800 controller, Wi-Fi clients experienced random disconnections.

**Solution:**
Identified a VLAN name mismatch between ISE authorization rules and the controller. Aire-OS tolerated the mismatch, while the 9800 enforced strict validation. Corrected the ISE configuration.

---

## FMC Automation for Microsoft IP Ranges

**Problem:**
Firewall access rules relying on Microsoft IP ranges required frequent manual updates.

**Solution:**
Created a dedicated AD service account and a PowerShell automation that retrieves Microsoft IP ranges weekly and updates FMC access lists automatically.

---

## Firewall Policy Cleanup

**Problem:**
Firewall access control policies contained redundant and outdated rules.

**Solution:**
Cleaned up and optimized firewall access control policies while implementing necessary rule updates.

---

## Daily XDR / Secure Mail Operations

**Problem:**
Ongoing security operations required continuous monitoring and incident handling.

**Solution:**
Performed daily monitoring of XDR, EDR, and Secure Mail, investigated incidents, and handled false positives and suspicious activity.

---

## VPN Profile Automation and Hardening

**Problem:**
VPN users had to manually launch Secure Endpoint and connect when moving to unsecured networks, with limited OS coverage.

**Solution:**
Automated VPN behavior and enforced stricter profile rules across multiple operating systems, eliminating manual user interaction which was causing wider attack surface for the organization due to human errors, or simply forgetting to connect to VPN.

---

## Microsoft Teams VPN Call Drops

**Problem:**
Direct Teams calls dropped after approximately 10 seconds when one user was on VPN and the other was on-site.

**Solution:**
After packet captures and protocol analysis, identified non–VPN-friendly direct call behavior. Implemented VPN split tunneling for required traffic, resolving the issue.

---

## Umbrella Site and Application Cleanup

**Problem:**
Umbrella policies included unnecessary applications and incomplete site coverage.

**Solution:**
Added all network sites to Umbrella and excluded unnecessary applications based on user roles.

---

## Secure Endpoint Service Control for Technicians

**Problem:**
System technicians could not stop Secure Endpoint services for maintenance, and documentation alone posed a security risk.

**Solution:**
Provided a controlled automation script allowing service control only for users with the required permissions, aligned with zero-trust principles.

---

## Guest Wi-Fi Internet Access Issue

**Problem:**
Guest users had no internet access on first connection even after accepting terms of service.

**Solution:**
Identified inconsistent configuration between the ISE and WLC and missing NAC State and Av-pair reauthentication enforced on both sides causing an authentication preventing VLAN enforcement. Adjusting the configuration resolved the issue without requiring reconnects.

---

## AD to Entra Authorization Integration

**Problem:**
Network authorization rules required migration from on-prem Active Directory to Entra-based identity.

**Solution:**
Assisted with architecture and integration, implementing certificate injection during OOBE and ISE validation with cached-certificate fallback for resilience.

---

## Windows 11 Upgrade Timeouts

**Problem:**
Windows 11 upgrades failed due to timeouts caused by intensive EDR file inspections.

**Solution:**
Created a separate EDR policy for upgrade scenarios and excluded required upgrade processes, resolving timeouts.

---

## Management VLAN Segmentation

**Problem:**
Switch management traffic was using client VLANs.

**Solution:**
Created a dedicated management VLAN and migrated all switch management interfaces to it.

---

## Access Point Replacement

**Problem:**
A large number of access points required replacement and reconfiguration.

**Solution:**
Replaced over 200 access points and corrected their configurations post-deployment.

---

## Firewall Patching

**Problem:**
Firewall software required security patching.

**Solution:**
Patched firewall systems in accordance with maintenance procedures.

---

## Physical Switch Replacement

**Problem:**
Legacy switches required hardware replacement with configuration preservation.

**Solution:**
Replaced physical switches and migrated configurations successfully.

---

## PKI Certificate Expiry

**Problem:**
Expired PKI components affected multiple network and security platforms.

**Solution:**
Replaced expired PKI certificates across WLC, ISE, FMC, Prime, and Catalyst Center.

---

## Guest Network Outage During ISE Certificate Replacement

**Problem:**
During a guest certificate replacement on Cisco ISE, external consultants replaced the certificate without awareness of a known Cisco bug that requires an ISE service restart. Because an ISE restart was not possible during production hours, AAA Override and NAC state were disabled on only one of the two independent WLCs (Catalyst 9800). The second WLC (AireOS) continued enforcing AUP/NAC, causing guest authentication failures and a large number of urgent service requests.

**Solution:**
The issue was quickly identified as an inconsistent workaround across the two independent WLCs. Disabling AAA Override and NAC state on the AireOS WLC as well immediately restored guest network connectivity. This bypassed the AUP and guest certificate dependency until a proper ISE restart could be scheduled outside production hours.

**Bug:**
https://bst.cloudapps.cisco.com/bugsearch/bug/CSCwc64480


---

## Automatic SSL Key Renew Automation

**Problem:**
SSL Cert expiry is currently painful for our environment, there are many of them that needs to be changed and keeping them in mind or relying on logs is no reliable. Human errors happen which causes service interruptions, double work, unnecesary stress, consuming time from the employees as well, therefore I am automating this. 

**Solution:**
The solution works with protocol ACME and some custom scripts that are inh help for reading files, shell script to be added for the ISE, FPR, WLC. (Currently in progress, to be updated.)

