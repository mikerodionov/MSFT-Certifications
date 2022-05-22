# Azure VNETs design considerations

## Address space and subnets

It is recommended to use RFC 1918 private address ranges:

````diff
+ 10.0.0.0 - 10.255.255.255 (10/8 prefix)
+ 172.16.0.0 - 172.31.255.255 (172.16/12 prefix)
+ 192.168.0.0 - 192.168.255.255 (192.168/16 prefix)
````

Following addresses cannot be used

````diff
+ 224.0.0.0/4 (Multicast)
+ 255.255.255.255/32 (Broadcast)
+ 127.0.0.0/8 (Loopback)
+ 169.254.0.0/16 (Link-local)
+ 168.63.129.16/32 (Internal DNS)
````

Azure reserves 5 IPs within each subnet - x.x.x.0-x.x.x.3 and the last address of the subnet:

```diff
+ x.x.x.0: Network address
+ x.x.x.1: Reserved by Azure for the default gateway
+ x.x.x.2, x.x.x.3: Reserved by Azure to map the Azure DNS IPs to the VNet space
+ x.x.x.255: Network broadcast address
```

VNETs planning considerations

+ Ensure non-overlapping address spaces. Make sure your VNet address space (CIDR block) does not overlap with your organization's other network ranges.
+ Is any security isolation required?
+ Do you need to mitigate any IP addressing limitations?
+ Will there be connections between Azure VNets and on-premises networks?
+ Is there any isolation required for administrative purposes?
+ Are you using any Azure services that create their own VNets?

## Subnets

A subnet is a range of IP address in the VNet. You can segment VNets into different size subnets.
The smallest supported IPv4 subnet is /29, and the largest is /2 (using CIDR subnet definitions).

Considerations:

+ Each subnet must have a unique address range, specified in CIDR format.
+ Certain Azure services require their own subnet.
+ Subnets can be used for traffic management. For example, you can create subnets to route traffic through a network virtual appliance.
+ You can limit access to Azure resources to specific subnets with a virtual network service endpoint. You can create multiple subnets, and enable a service endpoint for some subnets, but not others.

## Micro-segmentation

Although subnets are the smallest unit you can create based on IP addressing, you can further segment your network by using Network Security Groups (NSGs) to control access to the subnet. Each network security group contains rules, which allow or deny traffic to and from sources and destinations.

You can associate zero or one NSG to each subnet in a virtual network. You can associate the same, or a different, network security group to each subnet.

## Naming

+ Follow clear naming convention
+ All Azure resource names has to be unique within its scope (Management Group, Subscription, Resource Group). In case of VNET scope is RG, so its name has to be unique within it.

## Regions and subscriptions

+ A resource can only be created in a virtual network that exists in the same region and subscription as the resource.
+ It is possible to connect virtual networks that exist in different subscriptions and regions.

## Availability zones

+ AZs = unique physical locations within a region.
+ AZ is made up of one or more datacenters equipped with independent power, cooling, and networking.
+ AZ services that support AZ fall into 3 categories:
  + Zonal - pinned to specific zone (VMs, virtual disks, managed disks, standard IP addreesses)
  + Zone-reduntant - automatically replicated/distributed across zonnes
  + Non-regional - always available from Azure geographies and resilient to zone-wide outages and region-wide outages
