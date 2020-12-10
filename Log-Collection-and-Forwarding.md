# Log Collection and Forwarding

### Log Collector
* Panorama uses Log Collectors to aggregate logs from managed firewalls
  * Local Log Collector : runs locally on the Panorama management server
  * Dedicated Log Collector : M-600, M-500, M-200, M-100 appliance, or Panorama virtual appliance in Log Collector mode
* A Log Collector Group is 1 to 16 managed collectors that operate as a single logical log collection unit

### Log Forwarding Options
* Firewalls, Log Collector, and Panorama can forward logs to external services using :
  * HTTP(s) payloads
  * Syslog
  * SNMP
  * Email
* Cortex Data lake can only forward logs with syslog
