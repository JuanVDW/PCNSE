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
* When registering a Panorama VM, it will provide a serial # on the registration page.
  * Add this SN to the device under: Panorama > Setup > Management > General Settings
  * Note that physical appliances will already have this present.
* A valid support and capasity license is required.
  * The licences can be retrieved or added under: Panorama > Licenses > License management
  * Licenses can manage up to 25, 100, or 1000, depending on the license purchased.
  * If no path to the license server is available, then the licenses can be activated manually via phone, or generating an authorization code from the support website.
* Panorama supports 3rd party plugins.
  * These can be managed under: Panorama > Plugins
