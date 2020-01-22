# Monitoring and Reporting

### Dashboard, ACC and Monitor
* Dashboard
    * On the dashboard, individual widgets can be added and removed to have a customized display
    * A custom refresh counter can be set in the upper right hand corner
* ACC
    * Interactive graph of traffic and applications going through the firewall
    * Threat graph shows the risk of traffic going through
    * Custom Tabs can be added, with custom widgets to be added with information specific to your network and security concerns
    * Filters
        * Applied by using the funnel shaped icon in the top right corner of the widget
        * Can be applied to a specific widget to set custom displays
        * Persistent between reboots
        * A global filter can be applied to all graphs in the ACC to help troubleshooting or trends
        * Global Filters on the ACC are not persistent
        * Global filters can be applied in three methods:
            * Select an attribute from a table in any widget, apply it as a global filter
            * Promote a local filter and elevate it to a global filter
            * Use the global filters pane in the ACC
* Session Browser
    * To see active sessions on the firewall, go under Monitor > Session Browser
* Reports
    * Various reports can be accessed under Monitor tab
    * Predefined reports are included with the FW, and can be run to generate reports on commonly requested information
    * Custom reports can be created using the Query Builder
    * User or Group activity reports can show what users and groups access (must have User-ID enabled and configured)
    * Botnet reports can show systems that display behaviors noted with known botnets
    * PDF Summary reports can help aggregate reports and export to PDF format for reports and presentations
    * Report Groups combine reports into a single emailed PDF document
    * SaaS Reports can be generated on all data over a specified timeframe, or based on a certain group or application
    * Reports can be scheduled to run at a specific schedule

### Log Forwarding
* Under the logs, a CSV file can be exported with a maximum of up to 65,535 rows
    * Limit can be changed by updating the Max Rows field in Device > Setup > Management > Logging and Reporting Settings
* In Scheduled Log Export, the logs exported will be up to the last scheduled export
* Logs can be forwarded with:
    * Panorama
    * HTTP
    * Syslog SIEM
    * SNMP Manager
    * Email
* Panorama can be a log aggregator to generate reports based on all firewall traffic, push updated policies, and monitor usage and security incidents
* Panorama comes in applicances or VM:
    * M100 - supports 8 terabytes
    * M500 - supports 24 terabytes
    * VM - supports ?? petabytes
* Logs can be configured to be sent to an external archive system (syslog/SIEM server)
    * Define the remote logging destination
    * Enable log forwarding for each type
* SNMP Trap servers, Syslog and Email log forwarding can be configured under: Device > Server Profiles
* System log will contain information about changes to the device, failed logins and config commits.
* Log forwarding is configured under Objects > Log forwarding
    * The log objects that can be forwarded are broken down into categories: Traffic, Threat, Wildfire, URL, Data, GTP, Tunnel and Authentication.
* Each security policy rule can have a log forwarding profile applied to each rule. Under the actions tab, the rule can be set to log at start, end, and a log forwarding profile set.
* The Log forwarding profiles can be viewed under Device > Log Settings. These profiles are only visible to the security policy rules for log forwarding

### Syslog
* Allows the aggregation of logs from different sources to be combined, compiled, analyzed and reports generated from
* Syslog can be sent over:
    * UDP (unsecured and unreliable)
    * TCP (more overhead, reliable but unsecured)
    * SSL (highest overhead, secured auth is required)
* Syslog Profiles can be created under Device > Server Profiles > Syslog
    * Specify IP
    * Transport type
    * Set port defaults are:
        * UDP: 514
        * TCP: must be manually specified.
        * SSL: 6514
    * Format: BSD, Default, IETF
    * Facility: Level of logging to send
* Syslog over TCP/SSL
    * If the syslog server uses client auth:
        * A local certificate is required
        * The private key must also be available
        * This cannot be stored in an Hardware Security Module (HSM)
        * Import an existing certificate (or)
        * Create a self-signed certificate (or)
        * Create a cert using a windows cert server on your network
* Syslog custom format
    * Under Device > Server Profiles > Syslog
    * Create a custom log format based on criteria from your syslog server or custom needs
    
### Configuring SNMP
* If the SNMP Manager is not on the MGT interface, then SNMP must be enabled on the management profile where it is, and a service route added.
* Configure under Device > Setup > Operations > Miscellaneous > SNMP Setup
    * Enter and IOD and a mast to determine which parts of the MIB can be seen.
        * 1.3.6.1 mast 0xf0 to see everything
        * .1 mask 0x80 to see more information
    * Users select View for User, user name auth and privilege password should match in SNMP Manager
    * The Community string needs to match the string in the SNMP manager
* For SNMP Traps Profile:
    * V2c
        * IP of Manager
        * Community Name
    * V3
        * Username
        * IP Address
        * EngineID (can get using OID 1.3.6.1.6.3.10.2.1.1.0)
        * Auth Password (SHA)
        * Private Password (AES)
