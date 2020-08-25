# Panorama: Adding Firewalls to Panorama

### Adding new Firewalls to Panorama
* Configure the new firewall to connect to Panorama
  * On the firewall's web interface, navigate to: Device > Setup > Management > Panorama Settings
  * Enter the MGT IP of the primary Panorama appliance (and it's HA Peer)
  * Options on this page can be set to allow/disallow Panorama to manage policy and objects, and network templates
* Add the FW's serial number to Panorama
  * On the Panorama interface, navigate to: Panorama > Managed Devices > Summary > Add
  * Add the Device(s) Serial Numbers, and OK
    * On the Summary page after adding, the 'Group HA Peers' button can be selected to group HA firewalls. If this is unchecked, each firewall is individually displayed.
  * Device Tagging can be used to help identify specific firewalls in large managements. Navigate under: Panorama > Manage devices > Summary > Tag
  * Communication between devices can be secured. Navigate to: Panorama > Setup > Management > Secure Communications Settings
    * Communication will be handled by either a predefined or local certificate
* Commit All Changes
  * Changes must be committed on both the local firewalls, and on the Panorama device
* Panorama can manage all licences on managed devices. This can be viewed under: Panorama > Device Deployment > Licenses
  * License status and expiration dates can be seen
  * New licenses can be added with the 'activate' option, and an activation code
  * A license can be deactivated on one device, and activated on another (limited on some depending on the license type)

### Transition a firewall to Panorama management
* Prior to migrating firewall, the following options must be done:
  * Determine the OS version both on the Firewall and on Panorama
  * Panorama must be running the same or later version of PanOS that is on the firewall
  * Plan out the device group hierarchy and template deployment
    * Reduced redundancy
    * Streamline management of shared settings
  * Identify andy configuration that needs to be managed locally
  * Normalize Zone Names
* High Level sequence to add firewalls to Panorama:
  * Add a new device
  * Import the configuration
  * Fine-tune the configurations
  * Push the device state (config bundle)
  * Commit the device groups and templates.
    * Note: This process replaces some or all of the firewall's configuration with the config managed by Panorama
* In an HA Pair, further considerations are needed:
  * Disable the Config Sync under Device > High Availability > General > Setup
  * Add each firewall into Panorama
  * After the import and configuration within Panorama, add both firewalls to the same Device Group and Templates stack
* Steps to add:
  * Join the firewall to Panorama as a managed device
    * Do not add the FW to any device groups or templates yet
  * Import the device configuration to Panorama - this is done under Panorama > Setup > Operations > Configuration management
    * The import does not impact the config, it makes a copy of the configuration to Panorama
    * Update the device group and template configurations as needed or desired for standardization
  * Local configuration is removed
  * Zone names are updated (if needed)
  * Configuration data may be moved to different device groups or templates
  * Shared object names changed where conflicts exist
  * Push the configuration to the firewall; this will remove all policy rules and objects from local configuration
    * Export or push device config bundle
    * Note: the firewall cannot be added to a device group or template before the export/push device, as Panorama would error with problems of duplicate object names
    
### Upgrade PanOS Software and perform dynamic updates
* Panorama can manage software upgrades from a central location
* To see the options available, navigate to: Panorama > Device Deployment > Software
  * In this section, the software can be downloaded to Panorama, and then can be pushed to firewalls
  * The options include:
    * Upload Only (do not install)
    * Install, and reboot after install
* The application and content-ID updates can also be centrally managed and distributed with Panorama
* The options and configuration are available under: Panorama > Device Deployments > Dynamic Updates
  * A manual update/push can be done
  * A scheduled download/push can be done
  * Updates can only be done one at a time; stagger the updates to ensure that they will complete
* Global Protect can be centrally managed and updated in Panorama
* The options and configuration are avilable under: Panorama > Device Deployment > GlobalProtect Client
  * Select the version to download to Panorama
  * When downloaded, this version can be activated, and then specific firewalls can be selected to push the update to

### Manage Panorama and Firewall configuration backups
* Under Panorama > Setup > Operations, the export options for the configuration of Panorama are listed
  * Export Named Panorama configuration snapshot exports the current running config, the candidate config, or a previously imported config
  * Export Panorama configuration version exports a version that is specified
  * Export Panorama and Devices config bundle exports Panorama and all firewall configurations
  * Export or Push device config bundle (see the transition section above in this chapter for details)
* A scheduled export can be configured for automatic backups
  * Navigate under Panorama > Scheduled Config Export
  * Export can be scheduled once per day
  * FTP and SCP options are supported
    * FTP Passive mode can be selected from the checkbox, if Active mode is having issues
  * If using anonymous for username, do not specify a password.
* When a commit is done on a local firewall, a backup is sent to Panorama automatically
* By default, Panorama stores up to 100 previous configurations.
  * These can be viewed under: Panorama > Managed Devices > Summary
