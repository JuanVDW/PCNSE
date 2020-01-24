# High Availability

### Overview
* 2 firewalls can be configured in a High Availability pair
* HA Provides:
    * Redundancy
    * Business Continuity
    * If one firewall fails, the second can continue service with little to no interruption
* HA options can be deployed as:
    * Active/Passive: One active, one standby firewall
    * Active/Active: Both Active, used in specific circumstances, such as asynchronous routing setups
* Items Synchronized include:
    * Networks
    * Objects
    * Policies
    * Certificates
    * Session Tables (not available on the PA-200)
* Items NOT Synchronized:
    * Management Interface configuration
    * HA Settings
    * Logs
    * ACC information
* For a consolidated application and log view, Panorama must be used.
* PA-200 only supports HA-Lite
    * Lite is only available due to the low number of ports available on this model
* A/P Deployment
    * Only one firewall is active
    * One firewall synchronized and ready to process traffic
    * No increase in session capacity or network throughput
    * Supports VWire, Layer 2, and Layer 3 deployments
    * A/P HA has simplistic design to help with implementation
* A/A Deployment
    * Both firewalls are active and processing traffic
    * Both individually maintain routing and session tables, sync'd to the other
    * Is for use in Asyncronous routing deployments
    * No increase in throughput/session tables
    * Supported in V-Wire and L3 deployments
* HA Prerequisites
    * Both firewalls must be running the same hardware or VM model
    * Both firewalls must be running the same version PanOS
    * Starting in 7.0, session syncing is an option when upgrading major and minor releases
    * Updated and current Threat, URL and App DB's
    * Same dedicated HA interfaces
    * Licenses are unique to each FW; each needs matching licenses
    * Matching Slot configurations (for chassis 5000/7000 series)
    * VM's must be on the same hyper-visor, and have same number of CPU Cores

### HA Components and Operations
* HA Control Link is L3 link that requires an IP address
    * Used to exchange heartbeats and hellos and HA state info
    * Used to exchange routing and user ID information
    * Active firewall uses this to exchange config change information
* HA Datalink is a L2 Link, but can be configured in L3 that requires and IP
    * L3 is required if the data links are not on the same subnet
    * In L2 mode, the Datalink uses ethernet type 0x7261
    * The Datalink synchronize sessions, forwarding table, IPSec SA's and ARP tables in the HA Pair
    * Dataflow is unidirectional from the Active to Passive firewall
* Some models have dedicated HA ports, other models will use MGT or other in-band ports
    * Dedicated HA Ports are on 3000, 4000, 5000 and 7000 models
    * HA1/HA2 ports can be directly connected via ethernet cable
    * Recommended to use the MGT port as the control link
        * Any in-band port used must be configured as type HA
* HA Backup Links are recommended for the control link, to prevent the FW's going into 'split-brain' mode
    * Backup links must be on separate physical ports
    * Backup links must be in separate subnets as the primary backup links
* PA-7000 series mandates the use of specific ports on the Switch Management Card (SMC)
    * HA1-A is the control link; connect to same port on the 2nd firewall (or through switch/router)
    * HA1-B is the backup control link; connect to same port on 2nd firewall (or through switch/router)
    * Backup control link cannot be configured on the MGT or NPC Data ports
    * High Speed Chassis Interconnects (HSCI) are used as the Primary and backup Datalinks
        * If distance is beyond the scope of the HSCI ports, inband ports can be used
* HA firewalls can be set with a device priority to indicate a preference for which should be active
    * Enable Pre-empt on both firewalls if you want one firewall to become the active firewall when it is available/brought online
* Failure Detection
    * Hello and Heartbeats to confirm responsiveness and availability
    * Link Groups can be configured to validate interfaces are up
    * Path groups can monitor remote IP's to validate reachability
    * These items can be configured for any/all and the failure conditions
    * Internal Health checks are done to validate hardware is healthy
* HA Timers
    * HA Timers enable the firewall to detect failures and fail over
    * Timer profiles simplify setting HA timer settings
    * Advances enables individual timer modification
* HA Heartbeat on the management port
    * Helps to prevent split-brain
    * Happens when a non-redundant control link goes down

### Active/Passive HA Configuration
* Prepare In-band Interface
    * Set interface type as HA
* Configured under Device > High Availability
    * Each section here can be configured depending on the needs of the deployment
* Enable HA A/P mode under Device > High Availability > General
    * Select Mode (A/A or A/P)
    * Matching Group ID's for the HA Pair
    * Description (useful if configuring multiple HA configurations)
    * Check enable config sync to automatically sync any config changes to the peer
    * Add the Peer IP address
        * HIGHLY recommended to add a backup peer IP Address
* Configure the Control Link
    * Under Device > High Availability > General
    * Select the Control Link (HA1)
        * Select management port or another configured in-band port
        * MGT Port is recommended if a dedicated HA port is not available
        * Add a gateway if the peer is in a different subnet
    * Control link can be encrypted
        * Private keys will need to be exported/imported from the certificate configuration for this to function.
    * Backup link can be configured using an in-band port
* Configure the DataLink
    * If available, configured on the HA2 link
    * If using in-band and the peer is on a different subnet, add a gateway
    * An HA2 keepalive can also be configured
        * To prevent split-brain, use the action 'log only'
    * Select 'session synchronization' to ensure sessions are sync'd
    * A backup datalink can also be configured
* Election Settings
    * Device Priority can be set if one should be preferred to be the Primary
    * Preemptive can be set if a specific firewall should be primary if available. The firewall with the lower numerical value has the higher priority and will be primary if both are active and pre-empt is set.
    * HA Timer can be changed, however leaving at recommended unless a specific reason is needed for change.
* Set the passive link state to auto (optional)
* Link Monitoring (optional)
    * Configured under Device > High Availability > Link and Path Monitoring
        * Different link groups can be configured with different failure conditions
        * Example: Critical links can force a failover if any of the links fail. Other links can be set if all links fail.
* Path Monitoring (optional)
    * Configured under Device > High Availability > Link and Path Monitoring
    * Options for VWire Path, VLAN Path and/or a Virtual Router Path
        * A VWire will need a source and destination IP
        * Virtual Router monitoring does not need a source, as a route lookup will be done to determine the source

### Monitoring HA state
* During Boot, a FW looks for an HA Peer; after 60 seconds, if a peer hasn't been discovered, the FW will boot as Active.
* If a peer is found, it will negotiate with the peer
    * If Preempt is active, determine who has highest priority - this FW becomes active.
* If a FW is in a suspend state, it will not participate in a FW election
* States an A/P FW can be in are:
    * Initial: Transient state when it joins an HA pair
    * Active: normal state, primary and processing traffic
    * Passive: normal traffic is discarded, may process LLDP and LACP traffic
    * Suspended: administratively disabled
    * Non-functional: FW is non-functional and will need to have the issues resolved before it can return to service
* States of the individual members can be added as a widget on the Dashboard
* Add under Dashboard > Widgets > System > High Availability
* This will show at a glance the status
* Green: Good
* Yellow: Warning (normal state for a standby firewall in an A/P pair)
* Red: Error to be resolved
* When an HA Pair is initially formed, a manual sync will need to be done. This screen can initiate a 'sync to peer' push.
* System Log will show the events in an HA Pair negotiation
