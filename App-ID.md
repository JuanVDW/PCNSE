# App-ID

### Application ID Overview
* An application is a specific program or feature who's communication can be labeled, monitored and controlled
* App-ID does additional work beyond just port
* Port-based rules use 'Service'
* Application-based rules use 'Application'
* Application rules will allow only the application traffic that is allowed (ex: FTP) and not other traffic using that port
* Zero-day or unknown traffic trying to pass on an application policy is also blocked, because it doesn't match the application traffic
* App-ID for UDP can generally identify the application on the first packet
* App-ID for TCP will take several packets to identify, as the 3-way handshake needs to be done, and then the app data will need to be examined, depending on the app data.
* Application DB is updated weekly with new and updated application identifiers
* Unknown protocol decoder will attempt to identify unknown app-ID traffic
* Known protocol decoder will match traffic with a known app
* Decryption to ID traffic will check if decrypt is configured.
* App-ID steps:
    * Packet comes in - IP/Port identified
    * Check if allowed by Security policy
    * If allowed, App-ID will attempt to identify - Known, Unknown or Decrypt (if configured).
    * Does it match?
    * Security policy applied to allow or block

### Using App-ID in a Security Policy
* Traffic can shift from one app to another during a session lifetime
* As more traffic is received, it can also refine what the traffic it sees is
* This is why several applications are sometimes needed; web browsing, Facebook base and facebook chat could all be in the same session.
* Signatures contain data on several versions of applications
* Application dependance can be seen in the applications section under objects
* Some objects have dependencies built in - example, facebook has web-browsing as a needed dependence
* Under Objects > Applications, you can find what applications have what implicit use of other applications.
    * Search for an application
    * Click the application
    * Look for the 'implicitly uses' to see what apps it will implicitly use
* Application Filters can be used to allow access to a series of applications, such as Office application systems, or online streaming audio and video.
* Application Groups can be used to group together several applications for easier deployment to firewall security policy rules. They also can be used for QoS and Policy Based Forwarding (PBF) Policies.
* Applications, Filters and groups can be nested to several levels and added to policies.
* Application groups are added to security policy rules just like single applications.
* Under Objects > Services can be used to build custom services on specific ports. This can be used to narrow access on applications.
* Application Block Page can be configured to block access to specific applications. If User ID is in use, it will use the name of the user. If not, it will use their IP address.

### Identifying Unknown Application Traffic
* Traffic known to the PAN FW will be shown in the traffic log with the app identified
* When it's not able to be identified, if it is http, it is identified as web browsing. If it is not http, it is 'unknown tcp' or 'unknown udp'.
* In intitial deployments in TAP mode, in the the Policies > Security section, you can create a policy to block 'known good' or 'known bad' apps, and add known applications on your network to the appropriate rule. A third rule set for 'any/any/allow' will let you see the other applications not identified to help pinpoint what they are and their source/destination.
* To control unknown applications:
    * Create a custom application after identifying the traffic via packet captures
    * Configure an application override policy. This will disable the application ID for this traffic.
    * Block unknown-tcp, unknown-udp. Be cautious if in production, this could block legitimate traffic. This isn't recommended unless you are confident the traffic will have no production impact.

### Updating App-ID
* App-ID DB is updated weekly, and can be added to the application/threat auto update. It can also be manually downloaded and installed.
* A check can be done on the updated App-ID under Object > Applications, and clicking on the 'review policies' on the bottom of the page.
* On the Application > Review policies page, you can see what rules will be impacted by the new application matches.
