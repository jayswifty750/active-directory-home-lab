# Active Directory Configuration

This document covers the AD DS deployment, the DNS and DHCP services that support it, and the first Group Policy and OU setup.

## Overview

| Item | Value |
|---|---|
| Forest / domain | `lab.local` |
| Deployment type | New forest (single domain controller) |
| Domain controller | `WIN-J6BC254EIMC` |
| NetBIOS domain | `LAB` |
| Domain controller IP | `192.168.64.4` |

## Promoting the server to a domain controller

After installing the **Active Directory Domain Services** role, I ran the AD DS Configuration Wizard from the Server Manager notification flag.

On the **Deployment Configuration** page I chose **Add a new forest** and entered the root domain name `lab.local`.

![AD DS Configuration Wizard, Deployment Configuration](screenshots/01_ADDS_Deployment_Configuration_New_Forest.png)

> **TODO:** note the forest/domain functional level, that DNS server was selected on the Domain Controller Options page, and that a DSRM password was set. These pages were not captured.

The wizard then ran the prerequisites check and installed, and the server restarted as a domain controller.

## Verifying the deployment

- **Server Manager > Local Server** shows the domain as `lab.local`.
- The **Tools** menu now lists the AD tools: Active Directory Users and Computers, Domains and Trusts, Sites and Services, Administrative Center, ADSI Edit and Group Policy Management.

![Tools menu with AD tools](screenshots/02_Server_Manager_Tools_Menu_lab.local.png)

- **DNS Manager** opens and shows the server:

![DNS Manager](screenshots/03_DNS_Manager.png)

- `ipconfig /all` reports `lab.local` as the primary DNS suffix, and the server uses itself (`::1` and `127.0.0.1`) as its DNS server, which is expected for a DC that hosts DNS.

![ipconfig /all](screenshots/35_ipconfig_all.png)

## DHCP

I added the **DHCP Server** role and then created scopes from the DHCP console. Server Manager gets a DHCP page once the role is installed.

![DHCP in Server Manager](screenshots/05_Server_Manager_DHCP_Page.png)

### First scope (3/30/2026)

| Setting | Value |
|---|---|
| Name | `LabScope` |
| Range | `192.168.1.100` to `192.168.1.200` |
| Mask | `255.255.255.0` (/24) |

![Scope name](screenshots/06_DHCP_New_Scope_Wizard_Scope_Name.png)
![IP range](screenshots/07_DHCP_New_Scope_Wizard_IP_Range.png)

### Second scope (4/26/2026)

| Setting | Value |
|---|---|
| Range | `10.0.2.100` to `10.0.2.200` |
| Mask | `255.255.255.0` |
| Router (default gateway) | `10.0.2.2` |

![New Scope Wizard welcome](screenshots/29_DHCP_New_Scope_Wizard_Welcome.png)
![IP range 10.0.2.x](screenshots/30_DHCP_New_Scope_IP_Range_10.0.2.x.png)
![Router option](screenshots/31_DHCP_New_Scope_Router_Default_Gateway.png)

> **TODO:** confirm whether the scope was activated and whether the DHCP server was authorized in AD. See [troubleshooting.md](troubleshooting.md) for the issues I hit with DHCP.

## Organizational Unit and Group Policy

I created an OU named **CallCenter** at the domain root, with *Protect container from accidental deletion* left checked.

![New OU CallCenter](screenshots/42_ADUC_New_OU_CallCenter.png)

In **Group Policy Management** I created a new GPO and linked it to the CallCenter OU. The link is enabled, not enforced, and the GPO status is Enabled.

![Group Policy Management, CallCenter](screenshots/45_GPMC_CallCenter_Linked_GPO.png)

> **TODO:** list the settings configured in the GPO, if any.

User accounts in this OU are covered in [user-management.md](user-management.md).
