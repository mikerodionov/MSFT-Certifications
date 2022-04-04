# Design, implement, and manage hybrid networking (10-15%)

## Design, implement, and manage a site-to-site VPN connection

### design a site-to-site VPN connection for high availability

- VPN Gateway allows you to connect Azure with on-premise networks using S2S VPN creating encrypted tunnel between those

- Azure S2S VPN configuration steps outline:
  - Create and configure VNET GW and Local Network GW
  - Configure the on-premises VPN device with PSK
  - Create a VPN connection in VNET GW settings
  - Connect to Azure resource from on-premise network to test connectivity

- When configuring S2S VPN you set on-premise/remote network prefixes on Local Network Gateway - this entity represents your on-premises network for routing purposes. Local Network Gateway is used to configure the on-site VPN device public IP address (or FQDN) and local network address prefixes. This configuration can be set when you create Local Network Gateway or adjusted afterwards.

Once you created VNET Gateway, Local Network Gateway and Gateway Subnet, you need to configure on-site VPN device with the following:
  - VPN PSK configured on the VNET Gateway
  - Public IP of the VPN Gateway is set as on-site VPN device remore gateway
  - VPN device configuration script - Azure provides vendor specific configuration script

When S2S VPN tunnel fails to be established while configuration is done right, the following can be tried:
  - Verify PSK - should be the same between Azure VPN Gateway and on-premises devices
  - Reset the VPN gateway - it uses 2 active/passive instances so you should reset twice to ensure that both instances are reset
  - Reset the VPN connction - to attempt to re-establish connection

- VNET gateway has 2 types
  - VPN
  - Express Route

Further reading:

[What is VPN Gateway?](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways)

### select an appropriate virtual network (VNet) gateway SKU

- VNET gateway has various SKUs which define number of IPSec tunnels an the throughput

### identify when to use policy-based VPN versus route-based VPN
### create and configure a local network gateway
### create and configure an IPsec/IKE policy
### create and configure a virtual network gateway
### diagnose and resolve virtual network gateway connectivity issues

## Design, implement, and manage a point-to-site VPN connection

- To check P2S client connectivity failures look into IKEDiagnosticLog - P2S clients use IPSec protocol which in turn uses Internet Key Exchange (IKE) protocol to establiosh the security association (SA). That's a starting point for troubleshooting. This logging has to be enabled (Monitor > Diagnostic Settings > VNET GW > enable logging).
- Other logs
  - GatewayDiagnosticLog - GW configuration events, changes an maintenance events
  - RouteDiagnosticLog - changes to static routes and BGP events on GW
  - P2SDiagnosticLog - P2S control messages and events on GW

- P2S can use AD server for VPN authentication - for that Remote Access Dial In User Service (RADIUS) server has to be installed (on-prem or in Azure) and integrated with AD. Azure VPN GW can relay messages from VPN clients via RADIUS server to the AD controller
- P2S VPN supports the following protocols
  - OpenVPN Protocol
  - Secure Socket Tunneling Protocol (SSTP) - uses PPP to send traffic over TLS
  - IKEv2 VPN
 - P2S VPN supports the following authentication methods
  - Native Azure certificate authentication - authenticate by certificate present on client's device
  - Native AzureAD authentication (only supported for OpenVPN protocol and Windows 10 or 11, requires use of Azure VPN Client)
  - Authenticate using AD Domain Server (via RADIUS, uses OpenVPN)

### select an appropriate virtual network gateway SKU
### plan and configure RADIUS authentication
### plan and configure certificate-based authentication
### plan and configure OpenVPN authentication
### plan and configure Azure Active Directory (Azure AD) authentication
### implement a VPN client configuration file
### diagnose and resolve client-side and authentication issues
## Design, implement, and manage Azure ExpressRoute
### choose between provider and direct model (ExpressRoute Direct)
### design and implement Azure cross-region connectivity between multiple ExpressRoute locations
### select an appropriate ExpressRoute SKU and tier
### design and implement ExpressRoute Global Reach
### design and implement ExpressRoute FastPath
### choose between private peering only, Microsoft peering only, or both
### configure private peering
### configure Microsoft peering
### create and configure an ExpressRoute gateway
### connect a virtual network to an ExpressRoute circuit
### recommend a route advertisement configuration
### configure encryption over ExpressRoute
### implement Bidirectional Forwarding Detection
### diagnose and resolve ExpressRoute connection issues

- Bidirectional Forwarding Detection (BFD) - can be used for link failure detection to ensure faster failover to the secondary link. BGP keep-alive and hold-time lead to link failure detection timings of up to 3 minutes between MSEE and customer edge routers. BFD reduces that to a few seconds.

- When checking ER circuits verify that the Provider status (ER or Cloud Exchange) = Provisioned
