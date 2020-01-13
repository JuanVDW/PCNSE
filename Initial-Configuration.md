# Initial Configuration

### Administrative Controls
* WebUI
* Panorama
* CLI
* XML API

### Initial Access to System
* MGT is out of band, connected to the management plane; default IP it is [192.168.1.1/24](https://192.168.1.1) for physical. VM is DHCP.
* Console port (RJ45) 9600‐8‐N‐1
* admin/admin default login (nag screen until changed)
* MGT can be set for DHCP (although Static is highly recommended)

Initial config

Factory Reset instructions:

'Request system private-data-reset' if you have CLI access

If no CLI access, reboot into MAINT mode (see PAN documentation for further information)

Hostname limited to 31 characters

Configure new IP if needed, hostname, domain name (if wanted), and Gateway

MGT does updates for updates, DNS, NTP, unless done on a data port.

Add Service route(s) if any are needed.

(note from u/jaime_cal): HTTPS, SSH and Ping are enabled by default on the MGT Interface

(note from /u/jaime_cal): Minimum MGT Config are IP Address, Netmask and Default Gateway

(note from /u/jaime_cal): MGT port is used by default to access external management services, such as:

PAN Update Servers

NTP

DNS

Inband port can be set up to for service routes to perform these services which ports to retrieve them from if MGT port is not able to.

Configuration Management

Running config: active config running on the FW - running-config.xml

Candidate config: sandbox configuration; when a commit is done, candidate replaces the running config.

Previous configurations are saved. These can be reverted, exported, saved out, and imported.

Admin-Level commit will commit all changes made by anyone (if commit all changes is selected)

Config changes are logged under the admin logged in for change tracking

Commit locks stop other admins from committing changes

Config locks stop other admins from making any candidate config changes

(Note from /u/jaime_cal): Admin Locks can only be removed by the admin that put the lock in place, or by a super admin.

(note from /u/jaime_cal): Candidate Configuration is stored on control plane memory

(note from /u/jaime_cal): Running configuration is written to both control and dataplane memory

​Licensing and Software Updates

Registration with PAN is first step - support page and register new device. Generally this will send an activation code to your email.

Retrieve License from PAN License server

VM's can be downloaded from the software page after registration

Activate support license needed before activating other optional licenses (URL/threat/Wildfire, etc)

(if licensed) Set the dynamic updates for update/install on specific intervals

Update the Dynamic updates before upgrading the PanOS code. If no subscription, download and install manually.

Update the PanOS software. Steps to upgrade will likely be needed if upgrading between major versions (7.0 ->8.0 for example)

​Account Administration

Administrators can be created with specific access, using Admin Roles.

External auth servers supported are LDAP, Kerberos, RADIUS, TACACS+, SAML, along with 2FA are supported.

For non-local admins, create an admin role profile, server profile, authentication profile. authentication sequence is optional.

2 types of admin role profiles:

predefined dynamic profiles

super user, superuser(read-only), device administrator, DA (read-only), VS Admin, VS Admin (read-only).

administrator defined role based profiles

These can be granularity specified for specifically what they have access to, and functions they can change, update or view.

Predefined local admin accounts are:

super user, superuser(read-only), device administrator, DA (read-only)

local admin accounts can be set for minimum passwords, password aging and password complexity. Not enabled by default.

Creating non-local admins by creating an authentication profile.

Multiple servers can be used. LDAP, then RADIUS would be an example.

Create Server profile, then (optional) auth sequence, then authentication profile.

Allow list can be used for those that will be allowed to use certain auth profiles.

Viewing and Filtering Logs

Clicking any link in the Monitor > Traffic (or other entries) will filter the logs to only show entries with those

Filters can be saved and loaded for quick access
