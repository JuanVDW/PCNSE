# User-ID

### User-ID Overview
* Identify users by username and user group
* Creates policies and view logs/reports based on user/group name
* Used in combination with App-ID allows for very granular control
* Can be used to profile identified vs non-identified users for policy control
* Prior to being ready for use, the FW needs to know the group mapping to match user to IP
* Components for User-ID include:
    * PAN Firewall
    * PAN OS Integrated User ID agent
    * Windows Based User-ID Agent
    * Terminal Services Agent
* Integrated Vs Windows-Based Agents
    * Windows Agent uses Windows RPC to read the full security logs
        * Recommended for local deployments with the Windows Servers and Firewalls in the same physical network
        * Suitable for large remote sites
    * Integrated Agent uses Windows WMI or WinRM to read security logs to map Username to IP
        * Uses much less bandwidth
        * Uses more of the FW CPU
        * Better for remote deployments of firewalls in small offices, labs, etc.
        
### User Mapping Methods Overview
* Multiple Methods available, which will depend on the OS's, apps and infrastructure
    * Can monitor Windows DC, Exchange servers, or Novell eDirectory for user auth session tables
    * Probes windows clients for file/printer mappings
    * Captive Portal/GP logins
    * Terminal Services Agents for Windows RDP/Citrix
    * Syslog login/logout for NAC, 802.1x and Wireless AC's
    * Pan-OS XML API for devices that can send XML to the firewall
* For User-ID to function, it must be enabled on the zone
* User-ID can monitor Syslog server for actions to map users, when syslog messages are received from systems such as:
    * Unix/Linux Authentication
    * 802.1x Authentication
    * Windows and the User-ID agent can parse the Syslogs to help mapping users to IP's
    * Multiple profiles can be configured to read from different sources
* Domain Controller Monitoring
    * Monitors the Security Log of DC's
    * Continuously monitors logs for all login/logout events
        * DC must be configurd to log successful logon events
        * All DC's must be configured
    * An agent can only monitor one domain; for multiple domains, multiple agents would be needed.
    * Anyone who accesses file and printer shares also have their connections in the log read to map to their user ID
* User-ID can be configured to use WMI to probe windows system
    * This is useful for laptops and devices that may change IP's semi-frequently
    * NetBIOS is option and supported
    * WMI Probes are performed every 20 minutes (default)
* Global Protect
    * GP will provide User-ID with username/IP when they log into the gateway
* User ID Mapping Recommendations
    * User ID Agent is used for DC, Exchange, eDirectory, Windows file/print shares, Client probing and Syslog Monitoring
    * Terminal Services agent is used for multiuser systems for MS Terminal Server, Citrix Metaframe/Xenapp
    * Captive Portal maps usernames to IP's for users that do not login to a windows domain
    * GlobalProtect maps usernames/IP's for remote users
    * XML API is for non-User-ID devices and systems that can expore XML data

### Configuring User-ID
* Enable User-ID by the zone
    * Check the 'Enable User Identification' on the Network > Zones > (zone name)
    * Only enable on inside-facing zones, or it will attempt to identify any user on the internet if added on an outside facing zone.
    * By default, all subnets in the source zone are mapped; the include/exclude list can be added/modified to include or exclude custom subnets.
    * If WMI probing is enabled, it will only probe RFC1918 IP ranges (10/8, 172.16/12, 192.168/16); to add external IP's, they must be added to the include list.
* Configure user mapping methods (under Device > User Identification > User Mapping)
* Configure group mapping (optional, under Device > User Identification > Group Mapping Settings)
* Modify FW Policies for user/group matching

### PAN-OS Integrated Agent configuration
* On the DC, create a service account with the required permissions
* Define the addresses of the servers on the Firewall
    * An autodiscover option for Windows DC based on domain name (under device > setup > management > general settings) is also an option
* Add the service account to monitor the server(s)
    * Added under Device > User Identification > User Mapping; username should be entered as domain\account
    * Consult the Administrators guide for specific groups needed for your version of Windows server
* Configure session monitoring (optional)
    * Enable session monitoring under Device > User Identification > User Mapping > Server Monitor Tab
    * This option enables the File/Print Sharing mapping to account and IP address
* Configure WMI Probing (optional)
    * Enable WMI Client Probing under Device > User Identification > User Mapping > Client Probing tab
    * This will enable a probing of the clients every 20 minutes, to validate the same user is still logged into the same IP address
    * When an IP is found with no User-ID account, it sends it to the Agent for an immediate probe
    * WMI doesn't probe any IP's outside of RFC1918; to enable any non-routable IP's, add them to the include list in the zone.
    * File and Print sharing must be enabled on the client for this to function
* Commit the configuration and validate agent connectivity
    * After commit, each server specified under Device > User Identification should show as connected. If not, troubleshoot the connection from the agent to the DC, check service account rights, and confirm network connectivity

### Windows-based agent configuration
* Installation information:
    * Can be installed on 32 and 64-bit systems, XP SP3 or later
    * Should be installed in the the same physical network as the servers to optimize bandwidth
    * Should be installed on at least 2 domain members for redundancy
    * Recommended that it should NOT be installed on the domain controller itself (best practice)
* Download the agent software from PAN's support site.
    * Check the Release notes for details on supported OS's for the version you are downloading
    * MSI can also be used in SCCM to push to multiple locations
* In the Agent Application after installation:
    * Click Setup on the left-side to change any of the settings
        * Save will save but not activate
        * Commit will implement all changes
    * TCP Port 5007 is the default port
* Should run with a service account with proper rights
    * For specifics, check the Administrators guide or the support website
* Server Monitoring tab can be used to enable the security sessions reader
* Client Probing tab can be set to enable WMI probing
    * Sends a probe to each known IP to validate the same user is logged in. Each is probed once per interval (20 minutes is default)
    * NetBIOS can be enabled; is used for backwards compatibility with XP and earlier versions of windows. Needs to have port 139 open for communication.
* Clicking the Discovery on the left side, you can use the 'Auto-Discover' button to try to automagically add the DC's, or manually add the servers you want to probe.
* The firewall must be configured for each agent. This is done under Device > User Identification > User-ID Agents > Add
    * For Panorama setups that will gather the User-ID info, select 'serial number'
    * For Windows Agents, select 'host port' If you change the Port the agent uses, this is where it can be updated on the PAN side.
* Validate connectivity both on the agent and the firewall. Both should be green showing connection is working.
* The Monitoring section on the left side of the agent will show a list of current IP to User-ID mapping
* On the firewall CLI, you can see the mappings are:
    * `show user user-id-agent statistics`
    * `show users user-ids`
    * `show user ip-user-mappings all`
    * `show user ip-user-mappings` (ip/netmask)

### Configuring Group Mapping
* Server profiles with LDAP servers will be contacted, which order, and where to search the directory tree.
    * Defaults to port 389; if SSL is configured on the server, then 636 is available.
    * Type is the type of LDAP Server
    * Base DN should auto-populate when you click the drop-down menu
        * To check the Base DN manually, on the server open active directory domains and trust > Microsoft Console Snap-In - look at the name of the Top-level domain
    * Bind DN and Password will be used to auth users and read the LDAP directory. The Bind DN will depend on your DC configuration
        * If Universal Groups are used, the GC must be used to capture group memberships, and the LDAP port must be set to 3268
    * Bind, Search and Retry timeouts can be changed
* To configure Group Mapping, open Device > User Identification > Group Mapping Settings > Add
    * Select the server profile for your AD/LDAP server profile
    * The domain setting is generally blank; only enter a name if NetBIOS needs to override.
    * Groups objects should be dynamically populated by the LDAP server; these can be manually changed to look in specific locations.
* Group include list will allow you to filter specific groups to be included. If no groups are added to the 'included groups' section, then all groups are added.
    * It is recommended if you have a large/complex tree/forest, to specify groups. This will reduce search time and CPU utilization.
* Custom Groups allow you to set certain filters so that a filter will match certain critera, but are not in a specific LDAP/AD user group.
    * Examples could be: Department=Sales, City=Dallas, etc
    * Can help without the need for an AD Admin to create or modify existing structure.
    * User-ID also logs custom groups.

### User-ID and Security Policy
* In the security policy rules, the options under the Users section are:
    * any: Any user if they match the rest of the rule criteria
    * pre-logon: used with certain GP configurations and implementations
    * known-user: a known/mapped user
    * unknown: an unmapped/unmatched user/ip address
    * select: a specific user or group specified
* Note: The source IP and the source user are processed with a logical AND condition. So the user ID and the source IP range must match.
    * This can be used in places to allow access only if someone is connected to a network segment that is physically on-site at an office, and block access if someone is connected via GP or other VPN.
* Small office can use Users, however in larger environments, groups are best to base rules on.
