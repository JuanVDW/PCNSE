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
  * Layer 2 Zone: Layer 2 interface
  * Virtual Wire: VWire interfaces
  * Layer 3 Zone: L3, Aggregate, VLAN, Loopback and Tunnel interfaces
* Creating a zone is done by naming the zone, selecting the type of zone (from the list above). Interfaces can be added at this time, or later by editing the interface.

### TAP Interfaces
* Interface for receiving data from a mirror port on a switch. Generally used to gather data on the network in preparation for building security polices prior to cutover.
* TAP cannot do anything with the traffic blocked, shaped,...)
* TAP must be assigned to a TAP security zone.
* An Any/Any/Allow rule set with source/dest zones to the TAP interface is needed to start this data gathering, or the data is dropped by the FW in the default deny rule.

### Virtual Wires Interfaces
* This is used as a L2 firewall installation in-line. This way, the firewall can be 'dropped' in without any reconfiguration of the network.
* Interfaces will be L2, no IP's, L3 routing, FW managment or IPSec termination point is available.
* Create VWire instance, and add the interfaces if they have been set to VWire. If interfaces are not set, save the VWire instance and then go to the interfaces and add them into the VWire under interface type. A Vwire Zone is also needed.
* Vwire fully supports 802.1q VLAN tagging, and will pass tagged and untagged traffic as long as there is a security policy to allow it.
* Multiple VWire subinterfaces can also be created. Each sub-interface can be set in any zone and uses criteria such as VLAN tags and IP classifiers.

### Layer 2 Interfaces

Layer 2 switches traffic between 2+ interfaces. This makes the networks into a single ether broadcast domain.

Steps to create a Layer 2 interface:

create a vlan object under Network >

configuring the L2 interfaces

L2 does not participate in STP, but forwards STP packets.

L2 can do SSL Decrypt, User-ID, App-ID, Content ID, QoS.

Cannot do FW management, as no IP address.

Subinterfaces can be added to an 802.1q vlan

More than one VLAN can be added to the same top level port (example: e1/1.1 in vlan1 and e1/1.2 in vlan2). However, as there is no routing function, an external router, and security policies would be needed to route the data between the vlans.

Best practice is to use L3 subinterfaces to provide inter-VLAN routing.
