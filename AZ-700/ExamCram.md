# AZ-700 Designing and Implement Azure Networking Exam Cram

## Exam Info

- [AZ-700 Exam page](https://docs.microsoft.com/en-us/learn/certifications/exams/az-700)
- [AZ-700 Learning path](https://docs.microsoft.com/en-us/learn/paths/design-implement-microsoft-azure-networking-solutions-az-700)

59 questions, 2 hours

## VNETs

- VNETs exist within subscription and region - they can't span regions or subscriptions = regional service

```
|---------------------|
|----SUBSCRIPRTION----|
|---------------------|
|   |--------------|  |
|   |----REGION----|  |
|   |--------------|  |
|   | |----------| |  |
|   | |---VNET---| |  |
|   | |----------| |  | 
|   | ||-snet1-||  |  |
|   | ||- ... -||  |  |
|   | ||-snetN-||  |  |
|   | ||-------||  |  |
|---------------------|
```

- VNETs span AZs within region

- VNETs contain your Azure resources

- VNETs are L3 SDNs. IP/TCP/UDP. As VNETs are L3 SDNs, we can't do/control anything below/L2, e.g. we can't do VLANs, broadcast, multicast, [Generic Routing Encapsulation (GRE)](https://en.wikipedia.org/wiki/Generic_Routing_Encapsulation) incapsulation etc. Lower levels are used by Azure itself.

```
Generic Routing Encapsulation (GRE) is a tunneling protocol developed by Cisco Systems that can encapsulate a wide variety of network layer protocols inside virtual point-to-point links or point-to-multipoint links over an Internet Protocol network.
```

- VNET is defined as one or more CIDR blocks of private IPs (defined in [RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918) or others - you can specify public IP blocks but they will be considered internal/private anyway, i.e. not accessible from internet)
- It is recommended to use IP ranges from RFC 1918
- We can also optionally can add IPv6 blocks (we can add 0 or more IPv6 addresses), thus creating dual stack netwrok - this can be done on VNET creation stage or afterwards
- The subnets for IPv6 must be exactly /64 in size. This ensures future compatibility should you decide to enable routing of the subnet to an on-premises network since some routers can only accept /64 IPv6 routes
- Make sure your IP rabnges do not overlap both between on-premise and other Azure VNETs you use/manage

```diff
+Always create IPv6 subnets with /64 size
```
It is recommended to use addresses defined in RFC 1918:
```diff
+10.0.0.0    - 10.255.255.255  (10/8 prefix)
+172.16.0.0  - 172.31.255.255  (172.16/12 prefix)
+192.168.0.0 - 192.168.255.255 (192.168/16 prefix)
```
Also the following IP ranges cannot be used for Azure VNETs:
```diff
-224.0.0.0/4        - Multicast
-255.255.255.255/32 - Broadcast
-127.0.0.0/8        - Loopback
-169.254.0.0/16     - Link-local
-168.63.129.16/32   - Internal DNS
```

## Subnets

- We segment VNETs into subnets
- Subnets use portions of VNET IP addresses block
- Smallest IPv4 subnet is /29, largest is /2
- 5 IPs of every block are used by Azure for:
    - .0 - network address
    - .1 - default gateway
    - .2 - Azure DNS
    - .3 - Azure DNS
    - .255 - network broadcast
- Thus /29 gives not 8, but 3 usable IPs, /28 - 11 instead of 16, /27 - 27 instead of 32 etc.

## Public IPs

- When you create public IP in Azure it is **regional**
- By default Azute resources are allowed to access internet (outbound + traffic send in response - stateful connection) with their private Azure IPs (exist on resource/assigned to its interface directly)
- Azure Public IP can be added to Azure as a resource and attached to another resource (i.e. you won't see it in IP config as it is "linked externally to resource" - resource do not see it, linking handled by Azute fabric)
- Azure Public IPs have 2 SKUs:
    - Basic
        - You can have certain number of those for free
        - Open by default (use NSG to lock down)
        - Can be dynamic (can change on stop/start) or static
        - No AZ support (no resilience against data center failure)
    - Standard
        - Static only
        - Locked down by default (use NSG to allow)
        - AZ support
- Certain networking resources require matching IP SKU - e.g. Standard SKU LB require standard SKU IP (mainly for AZ support)
- Azure public IPs are used when we need to provide access to our resources from Internet (with NLBs etc.)
- Public IPs
- When multiple public IPs are required and you need them as a continuous block you can use **Public IP prefix** (contiguous block of public IPs), e.g. you can use this for NAT Gateways

## VNETs Peering

- When you have multiple regions and want to connect VNETS you have in them you will need peering.
- Peering requires **unique IP ranges** for connecting VNETs - **IP scpaces can not overlap**
- Withouth peering you would need to implement site-to-site VPN which will introduce more complixity and be limited by speed of the tunnel/appliance
- Peering uses native Azure backbone free of site-to-site VPN limitations
- We can peer across regions, but not across distinct Azure clouds (China, US Gov, Germany) - you can only peer within the same cloud
- Cross subscription peering has to be authorized (it will be made up of 2 peerings for 2 directions)
- Once peering is implemented we have "common IP space" comprised of all peered/known IP spaces (Azure VNET is known IP scpace tag), once VNETS are peered VNET tags will be including all peered IP spaces
- For hub and spoke topology peering is not transitive so you have to create additional peerings if you want to bypass hub (move to mesh topology) or, alternatively route through hub, placing virtual appliance into hub which will be doing routing or gateway subnet with gateway devices (site-to-site VPN or Express Route)
- On peering level we can further enable flag "Allow gateway transit" (let them use your gateways) on one side and "Use remote gateway" on another
    - Allow Gateway Transit on hub/receiving side - "Use this virtual network's gateway or Route Server" - AllowGatewayTransit
    - Use remomte gateway on spoke VNET side - "Use the remote virtual network's gateway or Route Server"
    - Additionally on spoke VNET side you may need to allow hub network to forward traffic from other VNETs - "Traffic forwarded from remote virtual networks" (enabled by default)

    ![image](/AZ-700/Images/AllowGatewayTransit.png)

## VNETs Routing

- In Portal for every NIC you can see "Effective Routes" - to see routes it knows about (Source - State - Prefix - Next Hop Type), as well as "Effective security rules" - to see associated NSGs
- Route source can be Default or User
- Next hop types: Virtual network gateway, Virtual network, Internet, Virtual appliance, None
- UDR - User Defined Routes can be defined on NIC level to specify next hop for prefix to change default behavior

## NAT Gateway

- Provides outbound NAT connectivity
- Uses standard SKU public IPs or public IPs prefixes and links those to particular subnets, so that their outbound traffic goes via NAT
- Efficient, but can suffer from port exhaustion (limited number of ports per IP - 64k ports) - to scale add more IPs or prefix
- Can be zonal or regional, but cannot be zone redundant (pin to a zone or deploy to a region only)
- IPv4 only
- Requires/works only with standard SKU resources - IPs, subnets, NLBs

## DNS Services

- By default all resources in VNET use Azure DNS - provides name resolution for resources **in the same VNET**
- Uses 168.63.129.16/32 IP - Azure DNS end point
- Custom DNS can be configured on VNET or NIC level
    - Your own DNS servers of any kind (BlueCat etc.)
    - Azure Private DNS Zones - specific zone name within which you can create DNS records manually or having auto-registration from VNETs
        - VNET can link to only 1 Azure Private DNS zone for registration
        - VNET can be linked to multiple Azure Private DNS zones for resolution
        - Azure Private DNS Zones are global resources (can be used cross-regions)
        - Each zone supports up to 100 VNETS for registation
        - Each zone supports up to 1000 VNETS for resolution
    - Azure Public DNS Zones
        - Only manual registration, no auto-registration
        - Specific zone name, zone is authoritative, you should own the name

## Hybrid Connectivity

### S2S VPN

- Connecting Azure VNET to on-prem network(s)
- Start with creating GW subnet (min /29, recommended /27)
    - /29 = 3 usable IPs - fine if subnet is use only for S2S-VPN or only Express Route Gateway
    - /27 = 27 - you will need - if you need both VPN and Express Route
    - Plan carefully as you can resize subnet afterwards, when  not sure use /27
- Deploy **VPN GW** into GW VNET
    - [A lot of SKUs](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku) - main differentiator is speed
        - Basic SKU - very limited
            - Policy based (static) / route based is possible but limited to 1 S2S and no P2S
            - Can have only 1 tunnel (connects to only one network)
            - Can't coexist with Express Route
            - Can't do point to site VPN
        - Other SKUs (Gen1, Gen 2) - recommended for use
            - Route based (dynamic)
            - N number of tunnels, encryption per tunnel
            - Coexistance with Express Route
            - Point to Site VPN supported
            - Optional BGP support
            - Active-Active configurations
        - You can't convert GW SKU from Basic to Gen1/2 without recreating it, but you can convert between Gen1/Gen2 SKUs
- GW SKUs can be split by feature SET into Basic & Gen1/2 SKUs as explained above
    ![image](/AZ-700/Images/GatewaySkuByFeatureSet.png)
- Provisioning outline
    - We create in Azure VPN GW (for active-active we can create two)
    - On-premise we have CIDR block and GW devices with public IPs
    - We then create LAN GW in Azure which represents public IPs of on-premise GW devices (for active-active we can create two)
    - We then create VPN cpnnectopn betwee LAN GW and VPN GW
    - There are multiple active-active configurations
    - Each individual tunnel is 1 Gbps (your GW can be 10 Gbps, but individual tunnels are limited by 1 Gbps)
- Once you did correct configuration your LAN is joined with your Azure VNET(s)

### P2S VPN

- Clients
    - OpenVPN - all clients, TLS 443
    - SSTP - Windows clients, TLS 443
    - IKEv2 - Mac clients
- Optional certificate based authentication is supported
- Integration with AD is possible (RADIUS required) - conditional access, MFA
- Connection over Interner - speed is less predictable

### Express Route

- Private L2 connection to [MSFT WAN/backbone](https://infrastructuremap.microsoft.com/explore)
- In every region where MSFT has presence we have an opportunity to connect to their their backbone
- [Meet Me peering points/enterprise edge locations](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-locations#global-commercial-azure) where you connect your or provider's equipment to MSFT equipment
- MPLS when using provider's equipment - you buying a circuit/line with certain speed
- Express Route Direct when using customer's equipment - you buy a port with certain speed
- Some of the location have Local Azure Region
- Once connection (physical) is in place you can't yet connect, you still need to configure/advertise routes - private peering or Microsoft peering
    
    - **Private peering** - LAN IP space to Azure VNET(s) IP space
        - LAN address space advertised up to VNET, inbound LAN to VLAN traffic flows throug GW, outbound VLAN to LAN goes via **Microsoft Enterprise edge routers (MSEEs)**
        - Requires ER GW to be deployed in GW subnet (ER GW is different from VPN GW, they can coexist and ER GW takes precedence by default)
        - /27 GW subnet and premium SKU of ER GW required for deploying BOTH ER and VPN gateways into it
        - You can do VPN over ER to ensure end-to-end encryption (ER itself considered to be private connection with no encryption in place)
        - You can enable FastPath so that inbound connection will not go through ER GW, but go directly to Azure resources instead (FastPath has limitation - no support for basic NLB, doesn't allow connection to Private Link) - ER GW still will be required for accessing other resources and BGP routes propagation
        - There are different SKUs of ER GW
            - By speed: Standard, HighPerformance, UltraPerformance
            - By features:
                - Standard - up to 4 circuit connections
                - High Port - up to 8 circuit connections + VPN and ER GWs coexistence
                - Ultra Performance (up to 16 circuit connections) + VPN and ER GWs coexistence + FastPath (allows inbound connection go directly to Azure resources bypassing ER GW via routes filter)
                ![image](/AZ-700/Images/ExpressRouteGatewaySkuByFeatureSet.png)
                - Can be zonal or zone-redundant (depends on SKU)
                - Connect your Azure VNETs between each other using VNET peering, as connecting them via Express Route can add latency (Meet Me point can be too far away and will add latency)
                - Depending on ER Circuit size (from 50 Mbps to 100 Gbps) number of VNETs links can vary from 20 to 100

    - **Micrsoft peering** - LAN to Azure Services not living/existing within VNETs - you normally connect to those via public IPs but with ER Microsoft Peering you can reach out to them via ER
        - If you just turn it on nothing changes, as by default roues not advertised via BGP to redirect traffic into ER peering connection
        - To enable this you have to create **route filter** and link that to Microsoft peering, whch will advertise Azure services via BGP (you can select which service community and region to advertise)
        - Microsoft/Provider edge requires private IPs for privare peering (no point to use public for that) or public for Microsoft peering

- **More on Express Route SKUs**
    - Standard - has geo-political boundary (created within [geo-political region](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-locations#locations) which includes multiple regions)
    - Premium
        - can be global (can connect to any region on MSFT backbone network) - Global connectivity to Microsoft core network
        - you can advertise more routes - increased route table limit from 4000 to 10000 for private peering
        - Connectivity to Microsoft 365

- Azure ER data plans
    - In Azure generally we do not pay for ingress, but we do pay for egress traffic, the same applicable to ER, but we have 3 SKUs:
        - Metered Plan - you only pay for egress - no egress included into plan, e.g. 100 Mbps - 110$ per month, but you pay per GB for outbound data
        - Unmetered Plan (price is significantly higher) - but egress is free, e.g. 100 Mbps - 575$ per month, but you don't pay for egress - fix rate for unlimited egress
        - Local Circuit - cheaper, when meet me location is very close to specific region, you can use local circuit to this closest specific region (and only to it) - **local only region**

#### BFD

- [BFD (Bidirectional Forwarding Detection)](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-bfd) - enables bi-derectional forwarding detection, enables much faster link failover (sub-seconds)

#### [MACsec](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-macsec)

- Express Route Direct - no service provider involved, at meet me location you connect directly your edge router to MSFT edge router ports
- MACsec - allows you encrpyt traffic between your and microsoft edge routers when you use ER Direct (not end to end encryption only between edge routes)
- Express Route Direcr uses 2 ports, MACsec can be enabled simultaneously on both or alternatively you can enable it one port at a time to minimize downtime
- Both XPN and non-XPN ciphers can be configured:
    - GcmAes128
    - GcmAes256
    - GcmAesXpn128
    - GcmAesXpn256

#### Global Reach

- You can use multiple local meet me locations in different parts of the world - this enables you to connect to Azure via local point of presense directly, as you won't want your remote region to talk with Azure over ER connection/circuit located in other part of the world
- Once you have mutliple ER connections/circuits you can configure your locations to talk between each other via Express Route using ER Global Reach feature

### Azure Virtual WAN

- Service which enables you to create managed VNET within region which simplifies connecting all your VNETS and LANs = software defined WAN (SD-WAN)
- To remove administrative overhead of multiple peerings, VPNs and Express routes
- This managed VNET allows you to peer all your other VNETs to it (options depend on SKU)
- Regional service, you create VWAN and inside of it there is Managed VNET (we do not have any direct access to it for you)
- You further can peer various VNETs to Managed VNET, as well as on-premise locations via S2S VPN or ER (and more depending on SKU)
- 2 SKUs - Basic & Standard
    - Basic - only supports S2S VPN
    - Standard - supports S2S VPN, P2P VPN, ER, VNET transitive & Multihub routing (to connect to another region with VWAN)
- Custom route tables can be added to restrict default "any-to-any"
- 3rd party NVA (Network Virtual Appliances) can be added into managed VNET (e.g. Barracuda CloudGen)

## Load Balancing Solutions

Options to make publicly available services HA - various types of LB, choise depend on use case/needs.

**Load Balancers**

| -            | Global           | Regional            |
|--------------|------------------|---------------------|
| L7 (http)    | Azure Front door | Application Gateway |
| L4 (tcp/udp) | Traffic Manager  | Azure Load Balancer |

Various solutions from the table can be combined on a global or regional level.

![image](./Images/LBHelpMeChoose.png)

Documentation links
[Load-balancing options](https://docs.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview)
[Azure Load Balancing Solutions: A guide to help you choose the correct option](https://devblogs.microsoft.com/premier-developer/azure-load-balancing-solutions-a-guide-to-help-you-choose-the-correct-option/)

### Azure Load Balancer (regional, L4)

- L4 - 5 tuples - SRC IP/PORT, DEST IP/PORT, PROTOCOL
- Acts as front end, can be either INT or EXT (either, not both at the same time)
- Has rules which decide how traffic is distributed, depending on SKU
    - Basic SKU
        - free, no SLA
        - up to 300 targets from the same availability set or VM scale set
        - no AZ support
        - open by default (similar to basic public IP)
        - Basic LB used with Basic public IP
    - Standard SKU
        - paid, has SLA
        - up to 1000 backend targets in the same VNET as LB
        - can point to NICs or IPs (some resources don't have NICs - e.g. pods in AKS don't have their own NICs, but we can target them by IP as long as they are in the same VNET as LB)
        - AZ support (can be zonal or zone-redundant)
        - locked down by default (similar to standard public IP)
- Backend resource pools - has to be in the same VNET or the same Availability Set
- Health probes are send to backend pools to ensure health of targets
- Rule types for traffic distribution across backend pool(s):
    - LB rule - hash based distribution using 5 tuples (SRC IP/PORT, DEST IP/PORT, PROTOCOL)
        - Stickenes can be configured, by default 5 tuples stickeness used, but can be 3 tuple (- PORT) or 2 tuple (- PORT, - PROTOCOL)
    - NAT rule - no distribution just always directing to particular VM PORT
        - With standard SKU we can do outbound NAT rules = Source Network Address Translation (SNAT)
- There is a finite number of rules
- Standard SKU allows to use HA ports (all flows) - it allows you don't create individual port rules anymore, instead it distributes flows over backen
- Floating IP - sends front end IP to the backend, so that backend sees is as destination (as opposed to normally seeing its own IP there)

#### Azure load balancer HA ports

Azure Standard Load Balancer helps you load-balance all protocol flows on all ports simultaneously when you're using an internal Load Balancer via HA Ports.

High availability (HA) ports is a type of load balancing rule that provides an easy way to load-balance all flows that arrive on all ports of an internal Standard Load Balancer. The load-balancing decision is made per flow. This action is based on the five-tuple connection: source IP address, source port, destination IP address, destination port, and protocol

The HA ports load-balancing rules help you with critical scenarios, such as HA and scale for network virtual appliances (NVAs) inside virtual networks. The feature can also help when a large number of ports must be load-balanced.

The HA ports load-balancing rules is configured when you set the front-end and back-end ports to 0 and the protocol to All. The internal load balancer resource then balances all TCP and UDP flows, regardless of port number.

### Azure App Gateway (regional, L7)

- Regional, L7
- L7 (http, https, http2 ) = URL based routing, redirection (http to https, one site to another), SSL offload (decrypt and send further in unencrypted form, or re-encrypt and send on), URL rewrite, request rewrtite, cookie-based affinity
- Support for connection draining
- Web Application Firewall (WAF) - component to protect against standard attacks - OWASP (Open Web Applications Security Project) core rules
- Gets deployed into VNET = NSGs can be applied
- Front end IP - AAG always has a public IP, optionally can have private; public has to be present - it may be unused but you must have it always
- You can't have AAG with private IP only, but you can lock down public
- Listeners are further configure to liste on particular IP and port
- At listeners level we can do SSL offload, assign certificates
    - Basic listener
        - Everything goes to the specific rule
    - Multi-site
        - Multiple-listener on the sampe IP & port
        - Looks at incoming FQDN (with support of wildcards) to send traffic to different rule based on that
- AAG uses rules
    - Basic rule - port to specific rule
    - Path based - based on part of the path route to different baclend pool
    - Re-write (request, URL, header)
- Backend pool can include on-prem via S2SVPN or ER
- HTTP settings (affinitiy, encryption)
- Different SKUs

### Azure Traffic Manager (Global, L4)

**Azure Traffic Manager** is a DNS-based traffic load balancer. This service allows you to distribute traffic to your public-facing applications across the global Azure regions. Traffic Manager also provides your public endpoints with high availability and quick responsiveness.

- Global, L4
- Adds anycast IP that can point to regional Azure LBs
- DNS based - you create a global traffic manager name, in fron of it you create CNAME exposed to/accessed by users
- Behind global traffic manager name are possible resolutions - FQDN names or IP addresses
- Possible targets are: Azure end-points, FQDN, IP, nested (another traffic manager profile)
- When you query name linked to global traffic manager it returns result based on different methods (supported targets vary per method)
    - Performance - target based on the latency (closest to the client) - most used
    - Priority - assign one target and backup targets in order of priority
    - Weighted - distribute according to assigned weight
    - Geographic - based on origing of DNS query
    - Multivalue - only supports IPv6/4 end points, returns all healthy endpoins
    - Subnet - maps subnet or sets of end-user address to specific endpoint
- Limitation - DNS record TTL delay (5 mins) which slows down switching to different target in case of outage

### Azure Front Door (Global, L7)

- Uses massive MSFT backbone network and its points of presence. AFD adds anycast IP which is available at all PoP (can be behind WAF) and then you have targets behind it
- L7: http, https, http2
- User goes to anycast IP, then split TCP kicks in, which establish TCP and TLS to local PoP
- You can do SSL offload on local PoP

## Network Security Groups and Application Security Groups

## Service Endpoints

## App Service Integrations

## Azure Firewall

## Network Monitoring

## Summary

