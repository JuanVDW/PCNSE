# Site to Site VPN's

###  Site-to-Site VPN

    Overview

        PanOS does IPSec tunnels as route-based tunnels

        Support for connecting to 3rd party IPSec devices

        The tunnel is represented by a logical tunnel interface

        The tunnel interface is placed in a zone

        When traffic is sent to the tunnel, the VPN is connected and traffic sent across

    IKEv1 vs IKEv2

        IKEv1 is the most common version used

        IKEv2 is primarily used to meet NDPP (network device protection profile), Suite B support and/or MS Azure compliance

        IKEv2 preferred mode provides a fail back to IKEv1 after 5 retries (about 30 seconds)

    IKE Phase 1

        Identifies the endpoints of the VPN

        Uses Peer IDs to identify the devices

            Usually the public IP's of each end

            Can also be an FQDN or other string of data

        Three Settings/modes: Agressive, Main, Auto

        5 pieces of info are exchanged during Phase 1:

            Authentication Method

            DH Key Exchange

            Symmetric Key Algorithm bulk data encryption

            Hashing algorithm

            Lifetime

    Ike Phase 2

        Creates the tunnel that will encapsulate traffic

        Each side of the tunnel has a proxy ID to identify traffic

            There is support for multiple proxy ID's

            Proxy ID's are also known as 'Encryption Domain' with other vendors

        Proxy ID's can be specific or 0.0.0.0/0

        5 Pieces of information are passed during phase 2:

            IPsec type/mode

            DH additional exchange if specified

            PFS

            Symmetric key algorithm/Hashing Algorithm

            Lifetime before Rekey

    Route Based site-to-site VPN

        VPN setup depends on the need and requirements of each site and the company configuration

        Each tunnel interface will support up to ten (10) IPSec tunnels

Configuring site-to-site tunnels

    Phase 1 IKE Gateway Configuration

        Create the IKE Gateway under Network > Network Profiles > IKE Gateways

        Simple tunnels (PAN to PAN) only require the interface, IP and PSK are needed.

        If the firewall uses a dynamic IP address (PPPoE DSL for example), leave the local IP address field blank.

        Certificate PKI authentication is supported, with the following Limitations:

            Maximum level of cert chain is 5

            CRL over LDAP is not supported

            Supported ID Types include: FQDN, IP Address, KeyID (Binary format ID Hex string),Email/User FQDN

            If none is specified, local IP is used.

    Phase 1 IKE Gateway Advanced Options

        Configured under Network > Network Profiles > Ike Gateways > Advanced Options

        Enable Passive Mode - will not initiate connections, only receive incoming requests

        Enable NAT traversal - UDP encapsulation used on IKE and UDP protocols, allows them to pass through intermediate NAT devices (upstream routers for example)

        Exchange Mode Options

            Auto (default) allows both Main and Aggresive

            Main: Used for fixed IP tunnels where the IP's on each end will not change

            Aggressive: Used when one endpoint has an IP address that may change, such as and ISP that provides a DHCP Address

            Both sides must have the same mode set

        Ike Crypto Profile

            By default, the crypto profile is set to AES-128-CBC, 3DES, SHA1

            A custome IKE crypto profile can be created under Network > Network Profiles > IKE Crypto

        Enable Fragmentation

            Allows the local gateway to receive fragmented IKE packets - max is 576 bytes

        Dead Peer Detection

            Identifies and confirms that the remote peer is alive and responding by sending a request to confirm, and receiving a response. If no response, the tunnel is torn down.

    Phase 1 IKE Cryptographic Profiles

        Both peers must match a cryptography for the tunnel to be established.

        Specify the DH group for Asymetric Key Exchange

        Multiple encryption types can be set to help match with a peer

        Multiple Authentication types can be set

    Phase 2 IPSec Cryptographic Profiles

        Configured under Network > Network Profiles > IPSec Crypto

        Set the IPSec Protocol (ESP or AH)

        Encryption type (must match remote peer)

        Authentication (MD5, SHA1, SHA256, SHA384, SHA512)

        Set the DH Group

        Set Lifetime

        (Optional) Set the lifesize, which will re-establish the tunnel after a certain amount of traffic has passed and the tunnel will rekey. This is to help prevent session data decryption of sniffed packets if one key has been captured.

    VPN Tunnel Interface

        Configured under Network > Interfaces > Tunnel tab

        Each Tunnel interface represents an individual tunnel connection

        Must be added to a security zone and a VR

        Does not require an IP address, but is needed if traffic will be participating in Dynamic routing protocols (ospf, BGP) or if the tunnel monitor is enabled.

    Phase 2 IPsec Tunnel

        Configured under Network > IPSec Tunnel

        Name the Tunnel with a clear identifier

        Specify the IKE Gateway and IPSec Crypto Profile (also called 'phase 2 proposal')

        'Show Advanced Options' checkbox will show further configuration items:

            Enable Replay protection - adds sequence numbers to packets so that a replay of captured packets to an IPSec device are discarded if not the expected packet numbers

            Tunnel Monitor - Only available if the Tunnel interface has an IP address. Sends Ping traffic to a specified IP across the tunnel to validate the route/path is valid.

            Monitor profile can be configured configured to send pings over the tunnel in an attempt to restore the session, or to fail over to another routing path.

            Monitor profiles can configured under Network > Network Profiles > Monitor

        The Proxy ID tab on the IPSec configuration page can be used to specify a local and remote proxy ID if needed, and a specific protocol of allowed traffic can be set if needed (TCP, UDP, Non-IP protocol number, or Any). By default, the proxy ID is 0.0.0.0/0

    Static Route for VPN

        Configured under Network > Virtual Routers > Add > Static Routes > IPv4

        Add the Static route remote destination network, specifying the tunnel interface of the VPN

        Next-Hop not required, but can be specified if needed

        Any admin distance or metric adjustments

        This step is not required if Dynamic routing will be used through the tunnel.

    Validating Connectivity

        Under Network > IPSec Tunnels

        The status will show green if the tunnel has established and is active.

        Clicking on 'Tunnel Info' will provide details about the tunnel.

IPSec Troubleshooting

    First step is to double-check all the settings in the IKE and IPSec sections. Talk to a rubber duckie!

    Check under Network > IPSec Tunnels

        Tunnel Status red indicates that IPSec Phase 2 is not available or expired.

        Ike Gateway Status green indicates Phase 1 is established, red indicates it has failed.

        Note that Tunnels are only up/established when traffic is needed to cross them (except when Monitoring is used, this will keep the tunnel active).

        The test VPN command can be used to test a VPN:

            Ike Phase 1 test: test vpn ike-sa gateway (name)

            Show VPN ike-sa gateway (name) to check status

            IPSec Phase 2 test: test vpn ipsec-sa tunnel (name)

            Show VPN ipsec-sa tunnel (name) to check status

            To validate traffic flow, use the 'show vpn flow' command.

    Troubleshooting from the responder is easier to track down issues

        VPN error messages can include:

        Wrong IP - Incorrect IP in P1 config or cannot communicate/route to the IP

        No matching P1/P2 proposal - double check IKE/IPSec and encryption settings

        Mismatched Peer ID - Able to communicate, but Peer ID's do not match

        PFS group mismatch - Check/update PFS DH Group

        Mismatched Proxy ID - generally caused by a mismatch from Policy Based VPN's

        The System Log will log attempts and this can be used to troubleshoot the errors
