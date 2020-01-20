# User-ID

### User-ID Overview
* Identify users by username and user group
* Creates Policies and view logs/reports based on user/group name
* Used in combination with App-ID allows for very granular control
* Can be used to profile identified vs non-identified users for policy control
* Prior to being ready for use, the FW needs to know the group mapping to match user to IP
* Components for User-ID include:
    * PAN Firewall
    * PAN OS Integrated User ID agent
    * Windows Based User-ID Agent
    * Terminal Services Agent
    * (other options - see below)
* Integrated Vs Windows-Based Agents
    * Windows Agent uses Windows RPC to read the full security logs
        * Recommended for local deployments with the Windows Servers and Firewalls in the same physical network
    * Integrated Agent uses Windows WMI to read security logs to map Username to IP
        * Uses much less bandwidth
        * Uses more of the FW CPU
        * Better for remote deployments of firewalls in small offices, labs, etc.
        
### User Mapping Methods Overview
* Multiple Methods available, which will depend on the OS's, apps and infrastructure
    * Can monitor Windows DC, Exchange servers, or Novell eDirectory for user auth session tables
    * Probes windows clients for file/printer mappings
    * Captive Portal/GP Logins
    * Terminal Services Agents for Windows RDP/Citrix
    * Syslog login/logout for NAC, 802.1x and Wireless AC's
    * Pan-OS XML API for devices that can send XML to the firewall
* For User-ID to function, it must be enabled on the zone
* User-ID can monitor Syslog server for actions to map users, when syslog messages are received from systems such as:
    * Unix/Linux Authentication
    * 802.1x Authentication
    * Windows and the User-ID agent can parse the Syslogs to help mapping users to IP's
    * Multiple Profiles can be configured to read from different sources
* Domain Controller Monitoring
    * Monitors the Security Log of DC's
    * Continuously monitors logs for all login/logout events
        * DC must be configurd to log successful logon events
        * All DC's must be configured
    * An agent can only monitor one domain; for multiple domains, multiple agents would be needed.
    * Anyone who accesses file and printer shares also have their connections in the log read to map to their user ID
* User-ID can be configured to use WMI to probe windows system
    * This is useful for laptops and devices that may change IP's semi-frequently.
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
