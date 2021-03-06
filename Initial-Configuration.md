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

### Initial config
* Factory Reset instructions:
  * `request system private-data-reset` if you have CLI access
  * If no CLI access, reboot into MAINT mode [(see PAN documentation for further information)](https://live.paloaltonetworks.com/t5/Management-Articles/How-to-Enter-Maintenance-Mode-on-the-Palo-Alto-Networks-Firewall/ta-p/55082)
* Hostname limited to 31 characters
* HTTPS, SSH and Ping are enabled by default on the MGT interface
* Minimum MGT config are IP Address, Netmask and Default Gateway
* MGT port is used by default to access external management services, such as:
  * PAN Update Servers
  * NTP
  * DNS
* Inband port can be set up for service routes to perform these services if MGT port is not able to

### Configuration Management
* Running config
  * Active config running on the FW - running-config.xml
  * Running configuration is written to both control and dataplane memory
* Candidate config
  * Sandbox configuration; when a commit is done, candidate replaces the running config
  * Candidate configuration is stored on control plane memory
* Previous configurations are saved. These can be reverted, exported, saved out, and imported.
* Admin-Level commit will commit all changes made by anyone (if commit all changes is selected)
* Commit locks stop other admins from committing changes and making any candidate config changes
* Admin Locks can only be removed by the admin that put the lock in place, or by a super admin
* Config changes are logged under the admin logged in for change tracking

### Licensing and Software Updates
* Registration with PAN is first step - support page and register new device. Generally this will send an activation code to your email.
* Retrieve License from PAN License server
* VM's can be downloaded from the software page after registration
* Activate support license needed before activating other optional licenses (URL/threat/Wildfire, etc)
* Set the dynamic updates for update/install on specific intervals
* Update the Dynamic updates before upgrading the PanOS code. If no subscription, download and install manually.
* Update the PanOS software. Steps to upgrade will likely be needed if upgrading between major versions (7.0 --> 8.0 for example)

### Account Administration
* Administrators can be created with specific access, using Admin Roles.
* External auth servers supported are LDAP, Kerberos, RADIUS, TACACS+, SAML, along with 2FA are supported.
* For non-local admins, create an admin role profile, server profile, authentication profile. Authentication sequence is optional.
* 2 types of admin role profiles:
  * predefined dynamic profiles
    * super user, superuser(read-only), device administrator, DA (read-only), VS Admin, VS Admin (read-only).
  * administrator defined role based profiles
    * These can be granularity specified for specifically what they have access to, and functions they can change, update or view.
* Local admin accounts can be set for minimum passwords, password aging and password complexity. Not enabled by default.

### Viewing and Filtering Logs
* Clicking any link in the Monitor > Traffic (or other entries) will filter the logs to only show entries with those
* Filters can be saved and loaded for quick access
