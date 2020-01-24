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
