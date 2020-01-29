# Interface Configuration

### Security Zones and interfaces
* Security zones are used to group like-devices, user groups, locations or specific-use systems.
* In-band interfaces are traffic-passing ports, ex: ethernet1/1, 1/2, etc
* Each interface (or subinterface) can only be assigned to one zone
* A zone can have multiple physical or logical interfaces
* Traffic inside zones is allowed by default. Example: Trust to trust is permitted by default
* Traffic outside zones is denied by default. Example: Untrust to DMZ is NOT permitted by default
* Zone types support specific interfaces:
  * Tap zone: tap interfaces
  * Tunnel zone: no interface
  * Layer 2 zone: Layer 2 interface
  * Virtual Wire: VWire interfaces
  * Layer 3 zone: L3, Aggregate, VLAN, Loopback and Tunnel interfaces
* Creating a zone is done by naming the zone, selecting the type of zone (from the list above). Interfaces can be added at this time, or later by editing the interface.

### TAP Interfaces
* Interface for receiving data from a mirror port on a switch. Generally used to gather data on the network in preparation for building security polices prior to cutover.
* TAP cannot do anything with the traffic (blocked, shaped,...)
* TAP must be assigned to a TAP security zone
* An Any/Any/Allow rule set with source/dest zones to the TAP interface is needed to start this data gathering, or the data is dropped by the FW in the default deny rule.

### Virtual Wires Interfaces
* This is used as a L2 firewall installation in-line. This way, the firewall can be 'dropped' in without any reconfiguration of the network.
* Interfaces will be L2, no IP's, L3 routing, FW managment or IPSec termination point is available.
* Create VWire object, and add the interfaces if they have been set to VWire. If interfaces are not set, save the VWire instance and then go to the interfaces and add them into the VWire under interface type. A Vwire Zone is also needed.
* Vwire fully supports 802.1q VLAN tagging, and will pass tagged and untagged traffic as long as there is a security policy to allow it.
* Multiple VWire subinterfaces can also be created. Each sub-interface can be set in any zone and uses criteria such as VLAN tags and IP classifiers.

### Layer 2 Interfaces
* Layer 2 switches traffic between 2+ interfaces. This makes the networks into a single ether broadcast domain.
* Steps to create a Layer 2 interface:
  * Create a vlan object under Network > VLAN
  * Configure the L2 interfaces
* L2 does not participate in STP, but forwards STP packets
* L2 can do SSL Decrypt, User-ID, App-ID, Content ID, QoS
* Cannot do FW management as no IP address
* Subinterfaces can be added to an 802.1q vlan
* More than one VLAN can be added to the same top level port (example: e1/1.1 in vlan1 and e1/1.2 in vlan2). However, as there is no routing function, an external router, and security policies would be needed to route the data between the vlans.
* Best practice is to use L3 subinterfaces to provide inter-VLAN routing

### Layer 3 Interfaces
* Layer 3 is able to route data between networks
* Each L3 interface needs an IP assigned
* App-ID, Content-ID, User-ID, SSL Decrypt, NAT, QoS are supported
* Can support management as it has an IP (further config would be needed)
* Support both IPv4 and IPv6, and support dual stack. (IPv6 must be enabled before it is available)
* When configuring interfaces you'll need:
  * Interface type (L3)
  * IP Address
    * IPv4 address can be set to Static, DHCP or PPPoE
  * Security Zone
  * Virtual Router (only if you want to route traffic to/from interface)
* Management Profile
  * Profile can be applied to an L3 interface. Protocols that can be allowed or denied are: 
    * Ping, Telnet, SSH, HTTP, HTTP-OCSP, HTTPS, SNMP, Response Pages, User-ID, User-ID Syslog Listener-SSL, User-ID Syslog Listener-UDP
  * Can be assigned to L3, loopback and tunnel interfaces (interfaces that have an IP address)
  * Security Policies are required to allow traffic to non-MGT interfaces
  * Can have a "permitted IP" list that will only allow a specific source IP address or subnet access to that specific set of permitted services
* Layer 3 Sub-interfaces
  * Assigned to a Layer 2 802.1q vlan
  * Different L3 sub-ints can be added to the same physical interface, but can only route at layer 3 between them if there is a route at (and security policy for the traffic) in the VR.
  * The configuration is the same as a standard Layer 3 interfaces configuration, with the exception of adding a vlan tagged
  * Untagged L3 sub-ints can be used, but the 'untagged interface' must be selected on the main interface advanced tab.

### Virtual Routers
* Used for Layer 3 IP routing
* Supports one or more static routes
* Supports multiple dynamic routing protocols, including RIPv2, OSPFv2, OSPFv3, BGPv4
* Supports Multicast routing protocols PIM-SM and PIM-SSM (both using pimv2). IGMP v1, v2, v3 are also supported on host-facing interfaces.
* Configure under Network > Virtual Routers
  * Give Name
  * Add L3 main, sub ints or tunnel interfaces
    * When interfaces are added, the connected routes are automatically populated into the routing table for traffic forwarding
  * Administrative Differences are used to determine routing decisions when identical destination routes are present.
* To add a default static routes, click: Network > Virtual Routers > Static Routes > Add
  * Give the route a name
  * Add a default of 0.0.0.0/0
  * Specify the interface this route will forward packets on (security policy will be needed to route the traffic)
  * Set the next hop type from the list: IP Address, FQDN, Next VR, Discard or None. Typically a default route is sent to a next hop IP address (upstream to an edge router or ISP link). Next VR sends it to the specified Virtual router (not this one), Discard will Discard (and no log). None is used if there is no text hop for the route.
  * Set any changes to the admin distance that are needed. Administrative distance defaults are specified by the type of route (static, connected, ospf, bgp, etc). Leaving this blank will set it to the default value.
  * Set any metric changes desired. This is useful if you have multiple links out and want to prefer one over the other. If the preferred link fails, the other route can be used to forward packets.
  * Select which routing table to install the route in: Unicast, Multicast, Both or no install. No install would stage the route, but would not be actively used.
  * Bi-Directional Forwarding can be selected. Both endpoints must support BFD. (see docs for more details)
    * BFD is not supported on the PA-200 or the PA-500
* Multiple Static Default Routes
  * Multiple SDR's can be configured
  * Route with lowest metric will be installed in the forwarding table
  * Path Monitoring can be used to determine if the route is usable
  * If ath Monitoring detects a failure, FW will switch to the higher metric route until the lower metric path is restored.
  * Path Monitoring can be configured under: Network > Virtual Routers > Static Routes > Add
    * On the bottom of the static route configuration, click the check on Path Monitoring
    * Multiple failure conditions can be added. Single or multiple source/dest entries can be set as criteria. Select either 'any' or 'all' when configuring more than one condition.
    * Set interval for ping interval and ping counts
  * If the lowest metric link fails monitoring, and then is restored, the 'Preemptive hold time' setting will be the timeout that the firewall will wait before failing traffic back to the lower metric link. This is defaulted to 2 minutes, but can be changed.
* Troubleshooting Routing
  * The 'More Runtime Stats' on the Network > Virtual Routers page will pull up a new screen to show the stats on the current VR.
  * Routing and Route table has all known routes (RIB)
  * Forwarding Table has all routes of where traffic will be forwarded to (FIB)
  * Static Route Monitoring tab will show the status of all Path Monitors configured
  
### VLAN Interfaces
* VLAN are Layer 2 802.1q network
* VLAN objects can be assigned and IP address, and connected to Layer 3 networks for Layer 3 routing
* Configure under Network > Network > VLAN > Add
* All vlan interfaces will start with 'vlan' - add the ID number (NOT a vlan ID, but matching them is recommended to avoid confusion)
* Interface must be assigned to an exiting vlan
  * If one doesn't exist or a new VLAN interface is needed, selecting 'New VLAN' on the drop down can be done to create a new VLAN.
  * Select the virtual router to add the interface to
  * Select the Security Zone to add the interface to

### Loopback Interfaces
* Loopbacks are logical interfaces that do not have a physical presence. They are assigned in a security zone and can be reached by their IP through another physical main or sub interface.
* Typical use includes Management UI access, Global Protect interface, or IPSEC tunnel interface termination point.
* Configure under Network > Interfaces > Loopback
  * Loopback interfaces always start with 'loopback', which cannot be changed. the ID number is set by the admin
  * Configured the same as a Layer 3 interface; Only exception is a loopback IP must be a /32 host IP.
  * Set the VR and the Security Zone the LB will be added to.

### Policy-Based Forwarding
* PBF rules are used to send specific traffic to an interface that is not the default route the traffic would follow from the routing table.
  * Use cases would include a private leased line you want to use for unencrypted traffic or traffic that needs low latency (VoIP, etc), while letting non-critical encrypted traffic over a DIA (direct internet access) circuit using an IPSec Tunnel.
  * PBF can be set using specific criteria, including source zone or interface, source user, destination IP and/or port.
  * Includes a Path Monitoring feature; if the interface the PBF is sent out goes down, the traffic will be able to go out the other interface.
* Configure under Policies > Policy Based Forwarding
  * Name the Policy
  * Enter the criteria: Source IP, Zone and/or User-ID
  * Specify desination/application/service. It is NOT recommended to use the application, as it may take several packets to identify the traffic, and it may not be forwarded based on the PBF.
  * Enter the details of where the traffic will be forwarded, including egress interface and optional next-hop. The Path Monitoring can also be configured.
  * Symmetric Return can also be set to be enforced here. 
