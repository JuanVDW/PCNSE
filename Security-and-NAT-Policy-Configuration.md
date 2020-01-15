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
* ess Groups are a group of Address Objects. Groups are used to help simplify firewall administration.

Tags are used to help organize and 'tag' rules that are in related policy groups. examples are: mail, web, DC, SQL, etc

Custom Services can be created to help provide simplification and identity to services. In services, you can specify a single port, multiple ports with commas, or a range of ports with a dash.

For the pre-defined intra/interzone allow/deny rules, choose override to set logging or other profile settings such as av/mal/vuln profiles.
