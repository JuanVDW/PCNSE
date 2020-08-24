# Panorama: Initial Configuration

### Register and License
* Register the devices under the 'Assets' tab on the support website, under 'devices'
* Provide the serial number from the fulfillment paperwork, or from the dashboard of the panorama device
  * For VM series, select the 'VM-Series' option, and enter the VM Auth-Code
* After licensing, connect to the console to perform initial management interface configuration
or
* Connect to the web interface at the default IP of 192.168.1.252
* A self-signed cert will be used on SSL connections
  * This can be replaced with a certificate issued from a CA
* When registering a Panorama VM, it will provide a serial # on the registration page
  * Add this SN to the device under: Panorama > Setup > Management > General Settings
  * Note that physical appliances will already have this present
* A valid support and capasity license is required.
  * The licences can be retrieved or added under: Panorama > Licenses > License management
  * Licenses can manage up to 25, 100, or 1000, depending on the license purchased
  * If no path to the license server is available, then the licenses can be activated manually via phone, or generating an authorization code from the support website
* Panorama supports 3rd party plugins
  * These can be managed under: Panorama > Plugins

### Perform initial config of interfaces and services
* Services need to be configured for accurate function
  * DNS and NTP servers should be set and validate they can be reached
  * Panorama can be configured with a proxy server for outbound internet access
* SNMP (Option) can also be configured
  * MIB's from PAN are required for an SNMP manager
  * These can be downloaded from the PAN website
* Legacy Mode (VM Mode) provides a single interface enabled, including managments
  * This mode is generally not recommended, unless needed for a specific deployment
  * The management profile should be configured for this interface to allow management connections
  * This is configured under Panorama > Setup > Interfaces > Management
  * If using SNMP, be sure to select SNMP in the management profile
* Network Segmentation can improve security and reduce congestion
  * Multiple interface (non-legacy) can be used for different network segments
  * Device Management and Device Log Collection can be set on different interfaces; This is set under the interface configuration at:
    * Panorama > Setup > Interfaces > Ethernet1/x
  * An example configuration for multiple interface could be:
    * E1/1: Managment, Log and Query (perimeter firewalls)
    * E1/2: Managment traffic, for managing, updating and configuration (perimeter firewalls)
    * E1/3: Device updates for content, software upgrades, etc (all firewall)
    * E1/4: Managment, Log and Query (DC/Colo firewalls)
    * E1/5: Managment traffic, for managing, updating and configuration (DC/Colo firewalls)
    * This is useful for helping to balance the traffic so interfaces aren't tapped

### Describe the Commit Process
* Commit process starts with clicking 'Commit' in the top right corner of the page:
  * Commit to Panorama will save the candidate configuration changes to the Panorama config.
  * Push to Device will push the configuration to managed devices
  * Commit and Push will perform both functions at once.
* In the commit to Panorama window, the option to commit all changes, or to commit changes made by the specific administrator
  * A preview changes option is available to see what the changes will be
  * A Change summary will provide a summary of changes
  * A Validate Commit will validate the full configuration with changes, but not write the new config
  * An optional (but recommended) description field is available. Putting brief notes stating what changes are being done is recommended
* In the Push to devices window:
  * A list of all devices and the location of the changes is listed.
    * Validate device group push will do a full validation on all device configuration to validate the config
    * Validate Template Push will validate that the templates are valid and no errors are present
    * The 'Edit selections' will let you edit the changes being pushed to which devices. Useful if pushing to a single location, test device, all but a single device
    * Edit selections will allow to be very detailed in what is pushed where
    * Options include to 'merge with device candidate config', 'include device and network templates', and 'force template values'
    * The 'force template values' will override any locally corrected values on the target
  * When a push is started, a status window will display showing the status
  * Selecting the link under 'Type' will show the status of the commit function
  * A Panorama server will commit one at a time, and while multiple can be made, they will be queued and run in series
