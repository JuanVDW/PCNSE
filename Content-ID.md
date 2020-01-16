# Content ID

### Overview
* Scans traffic for/offers protection against/can do:
    * Software Vulnerability exploits: detects attempts to exploit known software vulnerabilities
    * Viruses: detects infected files crossing the firewall
    * Spyware: detects spyware downloads and already infected system traffic
    * Malicious URL's: blocks URL's known to be locations that host or assist any of the content scanned with these profiles.
    * Restricted Files: tracks/blocks uploads/downloads based on application and/or file types
    * Data Filtering: identifies, logs and/or blocks specific data patterns
    * Wildfire Analysis: will upload suspect files to Wildfire for further analysis to determine if threat or benign.
* Security profiles must be added to a security policy to be activated
* Security Profiles are applied to all packets for the life of a session
* Security Profiles can be added to a group containing several security profiles for easier management, and applying specific types for specific rules.
* Threat log keeps records of vuln, AV, Anti-SW that can be reviewed, and can be forwarded to an external log server.

### Vulnerability Protection Security Profiles
* Include 2 predefined read only profiles. These can be cloned for making custom, or a new profile can be built from scratch.
     * Strict: Strict implementation of the profiles. Used for 'out of the box' protection.
     * Default: Default action that will happen that will be applied to traffic. Generally used for PoC and initial deployments
* Each individual vuln signature has a predefined default action. The default action can be seen under:
     * Objects > Security Profiles > Vulnerability Protection > Add > Exceptions - then select 'show all signatures' checkbox
* New updates are released 2x a week from PAN. *
* Rules can be configured to take packet captures
* Threat Name can be for 'any' for all, or a specific string to only scan for signatures matching that name
* Categories can be for Any or a specific CVE/Vendor ID
* Actions can include:
     * Allow: Permit without logging
     * Alert: Allow with Logging
     * Drop: drops and logs
     * Reset Client: TCP, sends a TCP reset to the client. UDP: Drops traffic/session
     * Reset Server: TCP: sends a TCP reset to the server. UDP: Drops traffic/session
     * Reset Both: TCP: sents TCP resets to both client and server. UDP: Drops the connection/session
     * Block IP: Blocks traffic/sessions from an IP, and a time to block can be set in seconds
* Exceptions can be set to override the actions on rules. This can be used to override false detection being detected blocking legitimate traffic. A list of IP's can be added to the exemptions column, useful for servers that may be flagged as sending out false positives.
