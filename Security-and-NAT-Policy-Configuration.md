# Security and NAT Policies

### Security Policy fundamental concepts
* All traffic must match a session and security policy (stateful firewall)
* Basics are a source and destination zone
* Granular includes Source/Dest Address, ports, application, URL Categories, Source user and HIP profiles
* Sessions are established for bidirectional data flow
* Policies > Security has the current security rules
* Columns on this page can be customized for your preferred information displayed.
* Three types of security rules:
    * Intrazone - all traffic within a zone. This traffic is allowed by default.
    * Interzone - all traffic between zones. This traffic is blocked by default.
    * Universal - Allowing all traffic between source and destination. Combines intra and interzone traffic.
* Any created rules have traffic logged by default; system created rules (intra/interzone at the end) are not logged.
* Rules are evaluated from top to bottom; when a match is found, no further eval is done.
* Rule Shadowing is when multiple rules match the same scope of traffic.

### Security Policy Administration
* Security policy must include:
   * Source zone, Destination zone, Action
* Security policies can also include:
   * Source IP
   * Destination IP
   * User
   * Application
   * Service
   * URL's
   * Additional actions (logging, vuln/av/malware profiles, scheduling and QoS)
* Scheduling can set times when a rule is allowed
* Rules can be reordered, disabled, deleted, added, cloned
* Unused rules can be shown by clicking the 'Highlight Unused Rules' checkbox at the bottom of the screen
* Address Objects is used to give a familiar name(s) to a single IP, IP range or an FQDN (functional dns resolution is needed for FQDN)
* Address Groups are a group of Address Objects. Groups are used to help simplify firewall administration.
* Tags are used to help organize and 'tag' rules that are in related policy groups. examples are: mail, web, DC, SQL, etc
* Custom Services can be created to help provide simplification and identity to services. In services, you can specify a single port, multiple ports with commas, or a range of ports with a dash.
* For the pre-defined intra/interzone allow/deny rules, choose override to set logging or other profile settings such as av/mal/vuln profiles.

### Network Address Translation
* NAT policy is evaluated after the destination zone route lookup
* NAT policy is applied just before packet is forwarded.
* NAT types are Source and Destination, and these are from the perspective of the firewall.
* Source NAT configuration
   * Source NAT is generally used from traffic on private internal IP's to a publicly routable IP (user inside to server outside). Types of Source NAT's include:
      * Static IP: fixed 1-to-1 translation; used when a NAT IP needs to remain the same IP and Port. This can be done with an IP address range, but the translation will always be static 1:1; in a 10-IP range, a x.y.z.2 source will always translate to z.y.x.2 destination.
      * Dynamic IP: 1-to-1 ip address translation only (no port number); can be used with a single or pool of IP's. Generally used for a set number of internal hosts with a matching number of external IP's, but static isn't required.
      * Dynamic IP and port (DIPP): This is used for multiple IP's to one or a few IP's, by allowing the connection to use another port than the default service. This is generally used for internet access outbound from home and business connections.
   * To use Source NAT:
      * Create a NAT policy rule: Original packets (IP's of client(s)) that will be using the NAT (source address), destination address (if needed), and the type of source translation (Static, Dyn, DynDIPP), and the IP to translate to.
      * A security policy will be needed to allow the traffic. The security policy will include:
           * Source Address (private/internal client IP's/subnet)
           * Source Zone where client IP's reside
           * Destination Zone (where IP to NAT to exists)
           * Any application or services
           * Allow the traffic on the policy
   * Bidirectional NAT's can be configured (only available for static NAT).
      * To configure, check the 'bidirectional' checkbox in the NAT configuration source nat translated packet tab.
   * DIPP NAT oversubscription is when a DIPP Source NAT uses more than the available 64,000 ports available per ip address. This is done by using the destination IP of new sessions outbound to 'ride' the same active port as other traffic going to that IP address.
* Destination NAT configuration
   * Destination NAT is used for external traffic coming into a private or secured location inside your network.
   * Types of Destination NAT:
      * Static IP: 1:1 translation of inbound traffic
      * Optional Port Forwarding: Can route to multiple internal servers based on Port number (25 to mail, 80 to web, etc)
   * Configuration:
      * Create a Destination NAT policy, defining the source and destination (pre-nat on both)
           * Incoming: (any or specific) to Destination (external routable IP) - Untrust to Untrust typically for example, with a destination translation to the internal IP address
      * Create a security policy that permits the post NAT zone (and IP's if needed) to the Pre-Nat destination IP/app/service/action
      * Security Policy does pre-nat source/dest, post-nat destination zone.
      * Destination NAT Port Forwarding Configuration:
          * When configuring the Destination NAT address under the 'Translated Packet' tab, put in the translated port of the destination
          * A destination NAT can be set with different destination translation IP's and ports from the same external facing IP, as long as the service is specified.
