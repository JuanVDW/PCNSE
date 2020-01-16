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

### AV Security Profiles
* Default Policy is available out of the box. This is recommended for initial configurations and TAP gatherings
* A custom policy is recommended. Options are to clone the default or make a new one from scratch
* The profile has predefined application decoders for common apps: FTP, HTTP, IMAP, POP3, SMB, SMTP
* Virus signatures are release every 24 hours by PAN
* Action is what will occur when a virus signature is detected.
* Actions can include:
     * Allow: Permit without logging
     * Alert: Allow with Logging
     * Drop: drops and logs
     * Reset Client: TCP, sends a TCP reset to the client. UDP: Drops traffic/session
     * Reset Server: TCP: sends a TCP reset to the server. UDP: Drops traffic/session
     * Reset Both: TCP: sents TCP resets to both client and server. UDP: Drops the connection/session
* Application Exceptions can be added to the Application Exception section in the profile config screen. Any application can be added, and the action specified.
* Packet Capture can be set to run a capture when a suspected virus is detected. This can be useful to help troubleshoot and resolve false positives.
* The Virus Exception tab can be configured to add false positives to virus detections. Add the Thread ID to the list to whitelist that pattern from having the specified action taken.

### Anti-Spyware Security Profiles
* Include 2 predefined read only profiles. These can be cloned for making custom, or a new profile can be built from scratch.
     * Strict: Strict implementation of the profiles. Used for 'out of the box' protection.
     * Default: Default action that will happen that will be applied to traffic. Generally used for PoC and initial deployments
* Each individual Anti-Spyware signature has a predefined default action. The default action can be seen under:
     * Objects > Security Profiles > Anti-Spyware Protection > Add > Exceptions - then select 'show all signatures' checkbox
* Signatures are release every 24 hours by PAN
* Spyware is generally detected when it attempts to 'phone home' to a C2 Server
* A custom policy is recommended. Options are to clone the default or make a new one from scratch. Best Practice is to create to your network design, deployment and company security policy.
* Each profile can contain several rules to apply policy based on the severity or type of spyware.
* Threat Name can be for 'any' for all, or a specific string to only scan for signatures matching that name
* Actions can include:
     * Allow: Permit without logging
     * Alert: Allow with Logging
     * Drop: drops and logs
     * Reset Client: TCP, sends a TCP reset to the client. UDP: Drops traffic/session
     * Reset Server: TCP: sends a TCP reset to the server. UDP: Drops traffic/session
     * Reset Both: TCP: sents TCP resets to both client and server. UDP: Drops the connection/session
* The Exception tab can be configured to add false positives to anti-spyware detections. Add the item to the list to whitelist that pattern from having the specified action taken. The action here will override the rule with the action in the 'Action' column
* DNS Signatures are included in the anti-spyware definition updates from PAN, but additional custom DNS domains can be blacklisted manually.
     * Actions are:
         * Allow: Permit without logging
         * Alert: Permit with Logging
         * Block: Block with Logging
         * Sinkhole: This is a specified IP to send DNS lookup for C2 traffic servers to a dead end. This can be sent to a PAN-provided IP, a local loopback, or a custom specified IP address. It is recommended that the sinkhole be in a different zone unless intrazone traffic is logged, so that the traffic can be logged.
* Actions are also available with single packet or extended packet capture
* Sinkhole traffic can be seen in the Monitor > Logs > Threat - action of 'sinkhole'

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

### File Blocking Profiles
* Allows blocking of prohibited, malicious and sensative files
* File blocking can be done by extension or examination of files
* Granular control can be done by (example) blocking .exe files from gmail, but allowing .exe's from FTP
* Profiles have these actions available:
     * Alert: Allow and Log
     * Continue: Log incident, send user to a browser response page for them to review/continue/stop.
     * Block: Block file and log
* Monitor > Logs > Data Filtering can be used to see the actions taken and the file name/type
* Rules can be set for:
     * Specific applications
     * File Types
     * Direction (upload/download/both)
     * Action (alert/continue/block)
* If a file matches multiple rules, the highest matching rule is applied.
* If Continue is set, the transfer is halted to alert the user that a matched file is attempting to be downloaded. This can be set to help prevent 'drive-by' downloads, or downloads that are done without the user knowing or interaction by the user.
     * Continue only functions with web-browsing
* The File Block can decode up to 4 layers of encoding. Encoding includes files such as .zip, .tar, docx, .gzip, etc
     * The 'Multi-Level Encoding' needs to be set under the 'File Types' in the file block rule
     
