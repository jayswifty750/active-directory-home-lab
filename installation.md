# Windows Server 2022 Installation

This document describes the base server used for the lab and how it was set up. Everything after the installation (Active Directory, DNS, DHCP, users) is covered in the other docs in this folder.

> **Note:** I did not capture screenshots of the installer itself, so the environment details below come from Server Manager and the file system after installation. Items marked **TODO** should be filled in from memory before publishing.

## Environment

| Item | Value |
|---|---|
| Operating system | Windows Server 2022 Standard Evaluation (build 20348) |
| Experience | Desktop Experience (full GUI, Server Manager) |
| Platform | Virtual machine, `QEMU Standard PC (Q35 + ICH9, 2009)` |
| Memory | 3 GB |
| Disk | ~31.4 GB |
| Time zone | UTC-08:00 (Pacific) |
| Computer name | `WIN-J6BC254EIMC` (auto-generated, not renamed) |
| Evaluation license | "Valid for 136 days" as of 4/13/2026 |
| Approx. install date | 2/28/2026 (based on `C:\Users` and `C:\Program Files` timestamps) |

> **TODO:** hypervisor/host (e.g. UTM, QEMU, other), ISO source and version, and CPU count.

## Installation steps

> **TODO:** confirm each step against what you actually did.

1. Created a new virtual machine with the specs above and attached the Windows Server 2022 evaluation ISO.
2. Booted the VM and started Windows Setup.
3. Chose **Windows Server 2022 Standard Evaluation (Desktop Experience)** so the GUI tools were available.
4. Selected a custom install onto the virtual disk and let Setup finish and reboot.
5. Set the local `Administrator` password on first boot and signed in.
6. Server Manager opened automatically and became the main management tool for the rest of the lab.

## Post-install state

After the first login I checked the server in **Server Manager > Local Server**:

![Server Manager Local Server and Tools menu](../screenshots/02_Server_Manager_Tools_Menu_lab.local.png)

- Microsoft Defender Firewall: **Public: On**
- Remote management, Remote Desktop and NIC Teaming: **Disabled**
- Ethernet: IPv4 address assigned by DHCP, IPv6 enabled
- The OS and hardware lines confirm Windows Server 2022 Standard Evaluation on a QEMU virtual machine

The network adapter received its address from the hypervisor's DHCP service:

![ipconfig renew result](../screenshots/34_ipconfig_renew_result.png)

| Setting | Value |
|---|---|
| IPv4 address | `192.168.64.4` |
| Subnet mask | `255.255.255.0` |
| Default gateway | `192.168.64.1` |
| DHCP lease | 1 hour (obtained 4:04 PM, expires 5:04 PM on 4/26/2026) |

Full output: [`ipconfig /all`](../screenshots/35_ipconfig_all.png)

## Adding roles

Roles and features are added with **Manage > Add Roles and Features** in Server Manager. The wizard lists the local server in the server pool:

![Add Roles and Features Wizard, Server Selection](../screenshots/04_Add_Roles_Features_Server_Selection.png)

Active Directory Domain Services, DNS and DHCP were installed this way. See [active-directory.md](active-directory.md).
