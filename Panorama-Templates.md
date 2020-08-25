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
  
### Create and Configure Templates
For this example, we are configuring an interface in a template
* Navigate to: Panorama > Templates > Add
  * Add the information for the template (name and description)
* To create a new interface to add, Navigate to: Network > Zones
  * Select the template you created
  * Add the zones for this template
* Templates can be cloned and altered for granular configurations based on the deployment scenarios and network designs
  * Cloned templates will NOT have any devices attached, they will need to be added manually
* To create a stack, navigate to: Panorama > Templates  
  * Click the 'add stack'
  * Create a name, select the VSYS, and add a description
  * Select the templates by clicking add in the templates section
  * Be sure to add them in the hierarchy they will be applied on. Lower listed templates will overwrite lower listed template settings.
  * Select the firewalls on the bottom right the stack will be applied to
* To push the template stack to a device or devices:
  * Navigate to: Commit > Push to Devices > Edit Selections
  * A checkbox on the bottom 'Force Template Values' is unchecked by default; selecting this checkbox will force-overwrite any values that are already configured locally on the firewall with the template/stack's configuration push.
* Local devices can be set to keep local values from being overwritten
  * On the device settings, the green gear on a configuration window can be selected, and locally applied settings put in place. This will superimpose an orange wheel over the green, showing that a local override has been put in place.
    * These settings can be replaced by Panorama, but should be done with care, as local values being overwritten can impact the function of devices.
* Device Server profiles can be configured in templates under Device > Server Profile

### Template Variables
* An example of this will assume:
  * Deploy firewalls that have an Internal IP of 10.0.*.1
  * Asterisk will be set for each FW
  * In Panorama 8.0, 255 templates would be needed
  * In Panorama 8.1, a few templates and 1 stack will do the same function
* Using the above example, using the template variable $IP_Variable
  * Variables can be used to replace device-specific information, such as IP's, IP Ranges, FQDN and interface specific information
  * The values of the variables can be overriden by the value in the template stack
  * Max number of variables per template is 4096
  * Max number of variables per template stack is 8192
  * These new variables also work with PAN firewalls running 8.0 or earlier versions
* To create a variable and a default value, Navigate under Panorama > Templates > Add (interface template example)
  * Create a new template - Confirm the interface template is selected
  * Navigate to Network > Interfaces
  * Select the Template you created
  * Add Interface - Select Slot Interface (slot 1 by default), Interface type (L3 in this example)
  * Click IPv4, and add to add an IP
  * Select Variable (Green X on the bottom of this box)
  * Give it a name (such as: $Inside_IP) and assign it an IP (such as 10.0.1.1/24) and select OK
* To assign device-specific value to a variable, Navigate to Panorama > Managed Devices > summary
  * For the specific firewall, click on the 'create' under the Variables column
  * The 'create device variable definition' window appears
  * Keep the button on 'No' for a cloning option, and click OK
  * The 'Template Variables for Device (name)' is shown
  * Select 'Inside IP' and click 'override'
  * Enter the override IP in the new window that appears for the variables
  * Repeat on other devices as needed
  * Commit and push to devices when completed and ready to push out
