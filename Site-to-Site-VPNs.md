# Site to Site VPN's

###  Site-to-Site VPN
* Overview
  * PanOS does IPSec tunnels as route-based tunnels
  * Support for connecting to 3rd party IPSec devices
  * The tunnel is represented by a logical tunnel interface
  * The tunnel interface is placed in a zone
  * When traffic is sent to the tunnel, the VPN is connected and traffic sent across
* IKEv1 vs IKEv2
  * IKEv1 is the most common version used
  * IKEv2 is primarily used to meet NDPP (network device protection profile), Suite B support and/or MS Azure compliance
  * IKEv2 preferred mode provides a fail back to IKEv1 after 5 retries (about 30 seconds)
* IKE Phase 1
  * Identifies the endpoints of the VPN
  * Uses Peer IDs to identify the devices
    * Usually the public IP's of each end
    * Can also be an FQDN or other string of data
  * Three Settings/modes: Agressive, Main, Auto
  * 5 pieces of info are exchanged during Phase 1 (HAGLE):
    * H: **H**ashing algorithm
    * A: **A**uthentication Method
    * G: DH Key Exchange (**G**roup)
    * L: **L**ifetime
    * E: Symmetric Key Algorithm bulk data **E**ncryption
* Ike Phase 2
  * Creates the tunnel that will encapsulate traffic
  * Each side of the tunnel has a proxy ID to identify traffic
    * There is support for multiple proxy ID's
    * Proxy ID's are also known as 'Encryption Domain' with other vendors
  * Proxy ID's can be specific or 0.0.0.0/0
  * 5 Pieces of information are passed during phase 2:
    * IPsec type/mode
    * DH additional exchange if specified
    * PFS
    * Symmetric key algorithm/Hashing Algorithm
    * Lifetime before Rekey
    
    
