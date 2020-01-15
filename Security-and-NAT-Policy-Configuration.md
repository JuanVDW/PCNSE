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
