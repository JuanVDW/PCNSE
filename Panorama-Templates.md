# Panorama: Templates

### Purpose of Templates
* Templates are Data Objects to hold settings for network devices
* When working under the Network and Device tabs in Panorama, a specific template must be chosen where settings will be stored
* Template stacks can be formed from individual templates, up to 8 layers
  * Settings in each later are inherited downward
  * If there is a settings conflict, a lower layer will overwrite it
  * Settings most commonly shared are generally put higher in the stack
  * Individual templates can be used in multiple stacks.
  * Firewall settings in a stack must be completed
    * Example: An interface must have all settings configured
* Steps to push common parameters to a device
  * Create a template then store the common settings/configuration parameters there
  * Create a template stack, then add templates to a stack
  * Push the stack to a device
* Stacks are:
  * An ordered list of Templates
  * All settings in a template stack are pushed to a managed device
  * Firewalls are assigned to one template stack
    * Max of 8 templates per stack
  * Panorama 8.0+ supports up to 1024 template stacks
  * When pushing updates to a device, the FW will push the updated template settings to the firewall, and then execute a local commit to update the firewall
* A template stack example:
  * Template one (worldwide)
    * Logging profile settings
    * Administrator accounts
  * Template Two (country)
    * SNMP, Syslog, DNS, Service Routes
  * Template Three (local)
    * Interface Settings
  * The above Templates will be: Worldwide->Country->Region
    * Country values will override the local settings, as it is higher in the template stack.
* Common Template deployment scenarios can include:
  * Function: Sales/Marketing/Support
  * Departments: Dev/QA/Tech
  * Geographic: World/Region/country/state/city
  
  
