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
