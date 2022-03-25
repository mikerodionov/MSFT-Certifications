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
- We can also optionally can add IPv6 CIDR blocks = VNETs are dual stack

```diff
+10.0.0.0    - 10.255.255.255  (10/8 prefix)
+172.16.0.0  - 172.31.255.255  (172.16/12 prefix)
+192.168.0.0 - 192.168.255.255 (192.168/16 prefix)
```

- We segment VNETs into subnets

9:43