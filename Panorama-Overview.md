# Panorama Overview

### Describe the Panorama Solution
* Provides Central Management for multiple PAN NGFW's
* Features of Panorama include:
  * Device administration
    * Allows Role-based access control (RBAC) for specific control, or specific firewalls
  * Template and Device Groups
    * Streamline Configuration
    * Prevent Duplicating items (and work)
  * Software / Content / License Updates
    * Schedules updates to push updates and software upgrades to all PAN managed devices
  * Local Policy and config
    * Allows local firewall admins to manage their own firewalls
  * User-ID Data
    * Panorama can provide a centralized place for User-ID collection and distribution to PAN Firewalls
  * Report Data
    * Summary data is gathered and sent to the Panorama server every 60 minutes

### Providing overview of the deployment design of Panorama
* Flexible Deployment Options
  * Selecting an Appliance to your environment
* Options Available include:
  * Basic Deployment
    * Direct Log collection (known as panorama mode)
    * Performs both device management and log collection
  * Distributed Deployment
    * Log collection is directed to Log collectors
    * Panorama maintains management of the firewalls (policy management)
    * Panorama will query the log collectors to gather data for centralized log views
    * Log Collector will respond to the query with a report of the data requested, not the data itself
    * The log data stays on the collector, is not transferred to the Panorama server in this setup
    * This can also be done with the cloud logging service that Panorama can query
* Panorama devices deployed in HA pairs must be the same version and type
  * Configured as HA
  * Sync'd automatically
  * Active platform is responsible for communication to devices
* Panorama can be run as a cloud service to manage their cloud-based VM systems
  * VM systems include:
    * VM Series firewall
    * Panorama Virtual Appliance
    * Virtual Dedicated Log Server
    * 8.1 VM systems are supported on Azure and AWS

### Describe the Panorama Platforms
* These devices can be deployed in different ways depending on the environment:
  * Management Appliance
    * Configuration
    * Device Management
  * Log Collector
    * Aggregated collection
    * Query servicing
  * Can be deployed as both in single system to scale distributed environments
* Models:
  * M100
    * Intel Xeon 4-core CPU
    * 32gb DDR3/120GB SSD
    * Up to 8TB storage; default is 2TB
    * Can be deployed as a Management Appliance or Log Collector
  * M200
    * Intel Xeon 8-core CPU
    * 128GB DDR4/240GB SSD
    * Standard 16TB RAID storage for logs
    * This device is generally deployed as a dedicated log collector, with a VM Panorama management device to administer.
  * M500
    * Intel Xeon 6-core CPU
    * 128GB DDR4/240GB SSD
    * Up to 24TB storage for logs. Default is 4TB
    * Deployed as a pair, this can service a busy PA-7000 in HSFM (high speed forwarding mode).
    * This device is generally deployed as a dedicated log collector, with a VM Panorama management device to administer.
  * M600
    * 2 Intel Xeon 14-core CPU
    * 256GB DDR4 Ram/240GB SSD
    * Up to 48TB storage for logs; default is 16TB
    * This device is used for large enterprises, or can be dedicated to a single location that processes a large amount of log entries.
* Panorama Modes (Physical)
  * Panorama Mode
    * Manage devices
    * Collect Logs
  * Log Collector Mode
    * Collects logs
    * Responds to query's from Panorama servers
  * Management Only Mode
    * Only functions as a device manager
    * No log collection
* Panorama Modes (VM)
  * Legacy mode
    * Manage Devices
    * Collects Logs
    * Legacy does not support logging/reporting enhancements made in 8.0+
    * Mode is available only when a Panorama VM is upgraded to 8.1
    * If a legacy device is changed to another mode, it cannot be changed back to Legacy.
    * Fresh install of 8.1, this mode is not available.
    * 8.1 Supports only the 3 modes below
  * Panorama Mode
    * Manage devices
    * Collect Logs
  * Log Collector Mode
    * Collects logs
    * Responds to query's from Panorama servers
  * Management Only Mode
    * Only functions as a device manager
    * No log collection
* Requirements for each Mode in a VM are:
  * Legacy:
    * 4 CPU's, 4GB RAM, Max of 8TB of Storage
  * Panorama:
    * 8 CPU's, 32GB RAM, Max of 24TB of Storage
  * Log Collector
    * 16 CPU's, 32gb RAM, max of 24TB of storage
  * Management only
    * 4 CPU's, 8gb RAM, no storage needed beyond OS Disk.
