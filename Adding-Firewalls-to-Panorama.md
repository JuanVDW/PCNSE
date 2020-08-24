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
