# Global Protect

### Overview
* Global Protect: Solution to VPN Issues
  * Extends NGFW to endpoints
  * Delivers full traffic visibility
  * Simplifies Management
  * Unifies policies
  * Stops Advanced Threats
* Components
  * Portal - Provides management functions for GP; every client connecting to GP receives configuration information from the portal
  * Gateways - Provide Security Enforcement for traffic
    * External gateways provide security enforcement and VPN Access
    * Internal gateways apply security policy for access to internal resources
* Connection Sequence
  * GP client connects to the portal for authentication
  * After auth, the portal sends the configuration and list of GP gateways
  * Client will connect to the portal with the best SSL response time
  * If the client is not installed, it will ask to be downloaded and installed
  * When the client is installed, the client will connect to the selected gateway
* GlobalProtect in the Cloud
  * Infrastructure can be extended using AWS VM-series. When a Portal is contacted, it can provide an AWS Gateway as an option.
* Simple Topology
  * Required at least one portal and one gateway
    * In small deployments this can be on the same device
    * If portal and gateway share a single system, only one certificate is needed for the firewall
* Advanced Topology
  * Multiple Gateways can be configured for performance and global deployments
    * Chosen gateway is the fastest responded
    * Only one portal can be configured and active
    * If portal goes down, existing users can log into a cached gateway
    * If portal is down, no new clients can connect, and no new configuration changes can be sent out to existing users
    * If the portal is down, either restore it, or activate a portal at another location
* Determining External or Internal Gateways
  * The portal may provide an IP and DNS to determine if the client is inside or outside the network
    * This should be a hostname that can only be resolved internally
    * If the IP is able to be resolved to a hostname, then the internal gateway is used
    * If the IP is not resolvable, then the external gateway is used
* GP for Internal Users
  * Internal Gateways are useful for enforcing group based policies, or access to restricted or confidential data
  * Examples include: Enforcing access to Engineering to Code and Bug DB's, While blocking access to Finance and HR to that resource
  * Profiles on the gateway can allow only certain LDAP/AD group members
  * Can also enforce HIP checks for AV/OS Patching/etc

### Preparing the firewall for GlobalProtect
* Certificates:
  * Certificate Authority Certificate (Optional)
  * GP Portal certificate
  * GP client certificate (optional)
  * A public CA certificate should be used for external users to provide the correct authority and security for the portal
  * Portal will include the public server certificate, and the client certificate and key.
  * GP users use the client certificate to identify the client
* Authentication Profile
  * Authentication profile are used to authenticate users. An existing Authentication profile can be used.
    * This is done under Device > Authentication Profile
* Agent Software
  * Under Device > GlobalProtect Client
    * Review the currently installed and activated GlobalProtect client version
    * New versions can be downloaded and activated from this page
    * GP Client software only needs to be updated and activated on the portal, not on the gateways
    
### Configuration: GP Portal
* GP Portal
    * Authenticates users using GP
    * Ability to create and store custom client configurations
    * Maintains a list of internal and external gateways
    * Manages CA Certificates for client validation of gateways
* Configuration
    * Configuration is done under Network > GlobalProtect > Portals > General
    * A Portal must be configured on an L3 interface
    * Custom pages can be created and uploaded to the firewall under Device > Response Page
      * Access to the Portal Login page can also be disabled (via browser on 443)
      * This does not impact the GP Client connections, they can still connect
      * Clientless VPN's need portal page to be accessible
* Portal Authentication
  * This is under Network > GlobalProtect > Portals > Add > Authentication
    * Portal Configuration Authentication profile is used to authenticate users
    * Certificate profiles are used if certificates are used for client validation. If not using certificates, select 'none'.
    * Authentication Message is an optional entry of up to 50 characters in length, to provide a message such as what kind of credentials to use
* Agent configuration
  * For certificate logins: A root CA must be specified under the Agent tab. If a gateway gives a certificate that is not from the listed CA, the login is rejected
  * Multiple configurations can be done for different groups. For example, a config for field users, and another for office users.
  * The internal portal can be configured under the Internal tab. These gateways need to be manually defined.
  * On the external gateway, the connection is made by the fastest response time and priority. A checkbox is available to manually select a tunnel.
  * Gateways that are set as 'manual only' are not provided for consideration for the fastest SSL response.
  * Three Types of App connection Methods are supported:
    * On-demand (Manual user initiated connection): Users connect when they need to, and disconnect when completed
    * User-logon (Always On): Automatically connects when the user logs in
    * Pre-logon (Always On): GP connects before the user has entered credentials, to keep the system secured, and updates the user login information when they supply credentials.
    * Pre-logon then On-demand: Unlike the pre-logon connect method, after the user logs in to the endpoint, users must manually launch the app to connect to GlobalProtect if the connection is terminated for any reason.
* Clientless VPN
  * Users can log in through a browser to access specific configured applications. Examples can be web-based email, internal web apps.
  * Applications can be published under Network > GlobalProtect > Portals > Clientless VPN > Application > Add
    * Group Mappings need to be configured prior to this point in order to use group mappings.
    
### Configuration: GP Gateway
* Global Protect Gateway is configured under Network > Global Protect > Gateways
* Select the L3 interface to use with the gateway, and the IP Address (if different from the interface IP)
* The tunnel tab will be needed if you are configuring an external gateway; optional for internal gateways.
* Agent configuration
  * Check the 'Tunnel Mode' to enable tunneling (for external gateways; not needed for internal but can be used)
  * Tunnel settings will include the tunnel interface
    * Enable IPSec, or uncheck to enable SSL. If IPSec is selected but not able to connect, it will fall back to SSL automatically.
  * Timeouts can be set for inactive connections to be disconnected.
  * A group name and password can be used in place of certificates to authenticate third-party VPN clients
* IP Pools Tab 
  * When configured in Tunnel mode, it functions as a DHCP client
  * Pools are only available if tunnel mode is enabled
* Split Tunneling
  * Not recommended; This will allow internal traffic to go up the tunnel, and internet traffic out the local network
  * Can be set to not allow any local network access (no access to home devices/printers/ect)
* Network Services Tab can be used to override the local settings of DNS, WINS and DNS Suffixes with the settings of the interface selected for the 'inheritance source' (aka the interface selected) field.
* User-ID can be used to map users to username to IP. This info is added to the User-ID list to show in the logfiles. Can also be used in internal networks to validate only specific users and authenticated users have access to specific systems.

### Configuration: GP Agents
* Agent runs on Windows, Mac, Linux, and is availabe as apps for IOS and Android (HIP check license required for IOS/Android)
* Agent must be installed on client device. The portal will provide a download link after a successful login to the portal webpage.
* Agent can be open or locked down depending on administrator
  * FQDN or IP, and login/password are minimum requirements if not already configured. Username can be left blank if using SSO.
  * A right-click on the icon will show the option available. Connect to will default to 'auto discovery' to find the fastest gateway. Manual selection of a gateway can be selected.
* X-auth can be configured (only configurable if Tunnel Mode and IPSec enabled)
  * Third party X-auth clients can connect to a GP Gateway such IPSec VPN on IOS/Android and the VPNC client on Linux
  * Provides simplified access
  * If group name and group password are populated, then the group name/password must be entered first, THEN the auth profile credentials are used. If the group name/password is left blank, a certificate must be used for the first authentication.
  * By Default, there is not required to re-authenticate when the IKE rekey timer is up. The check box can be set to skip auth on rekey.
* System Logs show the GP connection logs
  * Available Under Monitor > Logs > System, a log filter of 'subtype eq globalprotect' will show the GP connections
  * The traffic logs are under the standard Monitor > Logs > Traffic
