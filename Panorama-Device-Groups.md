# Panorama: Device Groups

### Device Group Overview
* Similar as templates but for Policies and Objects
  * FWs belong to one device group or device group hierarchy
  * Settings are pushed to FWs when clicking Commit > Push to devices
* Hierarchical Device Groups are a bit different than template stacks
  * The inheritance is specified during the creation of groups
* PAN 8.1 allows max 1024 groups
* Common Device Group Organization Strategies are by firewall functionality, geographic location or other political boundaries

### Configuring Device Groups, Objects and Policies
* To create a device group go to Panorama > Device Groups > Add
  * Give a name 
  * Choose a parent group (default is "Shared")
  * Add Devices
* To move a device group, select Panorama > Devices Groups and open the group, then adapt the Parent Device Group
* Make sure to select the correct Device Group when configuring an object
  * To prevent override of objects, select the "Disable override" check box
    * The green gear wheel appears on the object
    * Local FW admin cannot override objects
  * To move an object to another device group, click the Move button and select the new device group
* To manage policies, select the Policies tab and the select the device group
  * Select the policy type and the order
    * Policy Rules are evaluated from Top to Down
      * Pre-rules are evaluated first
      * Local policy rules second
      * Then the post-rules third 
      * Followed by default rules
    * Pre-rules and post-rules are read-only for the local admin
* Policy rulebase settings can be used to enforce tags, descriptions, etc... when creating policies (since PAN 9.0)
  * Can be set at global level, Panorame > Setup > Management > Policy Rulebase Settings or at template level Device > Setup > Management > Policy Rulebase Settings
* Since PAN 9.0, the "Audit Comment Archive" can be used to track change made to rules
  * The Rules Changes tab allows to view a side-by-side comparaison on two config versions
