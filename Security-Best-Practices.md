# Security Best Practices

### Migration Guidelines
* Two Options for deploying application based policies:
1. Migrate the configuration from an existing legacy firewall
    * Do a per-policy conversion migrating from port to applications
        * Migration Tool is an option intended to assist with a migration from a legacy firewall to a PAN FW
        * When a Migration is done with the tool, it moves the configuration from port-based to port-based.
        * Each rule will need to be recreated as an application based, place above the old port-based rule. This will confirm the application based rule is covering the previous port-based rule.
2. Deploy a 'from scratch' firewall as a virtual wire deployment
    * Create an Any/Any rule, and then craft rules based on the traffic in the traffic logs
    * Monitor the traffic as it transverses the firewall
    * Create individual rules based on this traffic, matching the source, destination and applications
        * User-ID should be implemented at this stage if it will be used in the deployment, and added to the new FW policy rules being created.
        * Add the rule above the any/any rule, and check the traffic logs to see that the traffic is being correctly identified and processed by that rule.
    * When all the rules have been converted (done after a 30-day period of in-line vwire data gathering), and all business critical traffic is confirmed to not be hitting the any/any/allow rule, change the any/any/allow to an any/any/deny rule, and validate that no critical traffic is being blocked
* After the build-out of the application-based rules, rule consolidation, customization and risk analysis can be done. Shadow rules, address objects, application groups and unused rules can all be evaluated and combined or removed if not needed.
    * Address object groups, applications groups are examples that can be built/updated to help better organize FW rules.
