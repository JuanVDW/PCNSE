# Platforms and Architecture

### Security Platform Overview
* Recon, Weaponize, Deliver, Exploitation, Installation, Command & Control, Act on Objective

### NGFW
* Identifies and inspects all traffic
* Blocks known threats
* Sense unknown to cloud
* Extends to mobile and virtual networks

### Threat Intel Cloud
* Gathers potential threats from network and endpoints
* Analyzes and correlates threat intel
* Disseminates threat intel to network and endpoints

### Advanced Endpoint Protection
* Inspects all Processes and files
* Prevents known and unknown exploits
* Integrates with cloud to prevent known and unknown malware

### Other items available
* Panorama: Centralized management of all PAN firewall points.
* AutoFocus: Hosted security service, provides aggregate info on threat intel from multiple sources
* Aperture: protection for cloud-based systems like box, sales force, etc. helps manage permissions and file scans. focused on DLP, PCI, and other personally exploitable data.
* GlobalProtect: VPN that puts traffic back through the firewall to ensure traffic scanning, traffic visibility and security.

### Next Gen Firewall Architecture
* PAN is a single-pass Parallel Processing system 
* Single-Pass Architecture
    * Per-Packet Operations:
        * Traffic Classification with App-Id
        * User/group mapping
        * Content Scan (threats, URL, confidential data)
* Parallel Processing
    * Function specific parallel processing hardware engine
    * Separate data/control plane
* Dataplane
    * Signature Matching
        * IPS, virus, spyware, CC#, SSN
    * Security Processing
        * App-ID, User-ID, URL Match, Policy Match, App Decoding, SSL/IPSec, decompression
    * Network Processing
        * Flow Control, Route Lookup, MAC lookup, QoS, NAT
* Control Plane
    * Management
         * Configuration, Management UI, Logging, Reporting
* Zero Trust Security Model
    * No trust provided by default
    * Never Trust, always verify
    * Need to establish trust boundaries
    * NGFW offer several options to help approach the zero-trust model
         * App-ID, User-ID, URL filtering, Vuln filtering, anti-spyware, anti-virus, Traps, file blocking, DOS protection, Zone Protection, Wildfire
### Public Cloud Security
* SaaS security provided by Apreture
* Can be spun up on demand
* Can be managed through Panorama, same or different device groups

### Firewall Offerings
* Physical devices:
    *  PA-220, PA-800, PA-5200 - Next Gen hardware
    *  PA-7050 and PA-7080 are Chassis architecture
* Virtual devices:
    *  VM-700, VM-500, VM-300, VM-100, VM-50, VM-50lite
    *  Supported environment
        * ESXi: All
        * KVM/Openstack: All
        * Hyper-V: All
        * VMWare NSX: VM100-500
        * AWS: VM100-700
        * Azure: VM100-700
    * New sessions all models: 8000 per second
    * IPSec throughput, all models: Varies according to model (originally was 250mbps, per the training, however documentation states otherwise. Please see the hardware spec sheet at the top of this post for specifics)
* Virtual Systems
    * Ability to have seperate virtual firewalls in a single physical chassis
    * Each system has its own zones, policies, administrators.
    * Supported on the 3k/5k/7k models
