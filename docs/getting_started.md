# Getting Started

![Banner](https://raw.githubusercontent.com/arpsch/openapi3.0/master/docs/iot_gateway_banner.png)

### Requirements
* OS and User-agent
  Ubuntu 18.04 is preferred with 10GB HDD and 2GB RAM. Chrome browser is preferred, other browsers are also fine.
* Docker engine 17.03 or later version
* Docker-compose 1.6 or later version
* Fast internet connection.
### Fleet concepts
* Authentication and authorisation: All the communications are encrypted with TLS. Client based certificates are stored in the device, which carries device unique id (UUID) as well. Failing to carry one of these authentication fails. After authentication, device will be listed in pending list.
* Authorisation is done by administrator or by pre-authorisation.
* Configurations
   * Configurations are set of configuration parameters that shall be bound to a device or group of devices while authorising the device. *without assiging any configuration, authorization can't be performed.*
 * Flavors
   * Video surveillance as a service (VSaaS) and Video Management Service (VMS) are two flavors supported. An onboarding device should also be bound with flavor. *without assiging any flavor, authorization can't be performed.*
* Reject
  * Device can be rejected, any further request from the device will be rejected without notice.
* Dismiss
  * Device can be dismissed, where device will not be seen until next request from the same device.
* Decommission
  * Device can be decommissioned, where the device will be completely removed from the fleet, next request from the device will be handled as a fresh request.
### Further information
