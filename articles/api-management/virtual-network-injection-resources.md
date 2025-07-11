---
title: Azure API Management virtual network injection - network resources
description: Learn about requirements for network resources when you deploy (inject) your API Management Developer or Premium tier instance in an Azure virtual network.
author: dlepow

ms.service: azure-api-management
ms.topic: concept-article
ms.date: 04/17/2025
ms.author: danlep
---

# Network resource requirements for API Management injection into a virtual network

[!INCLUDE [api-management-availability-premium-dev](../../includes/api-management-availability-premium-dev.md)]

The following are virtual network resource requirements for injection of an API Management Developer or Premium instance into a virtual network. 

> [!NOTE]
> To inject a Premium v2 instance in a virtual network, the requirements and configuration are different. [Learn more](inject-vnet-v2.md)


* An Azure Resource Manager virtual network is required.
* The subnet used to connect to the API Management instance might contain other Azure resource types.
* The subnet used to connect to the API Management instance shouldn't have any delegations enabled. The **Delegate subnet to a service** setting for the subnet should be set to *None*. 
* A [network security group](../virtual-network/network-security-groups-overview.md) attached to the subnet above. A network security group (NSG) is required to explicitly allow inbound connectivity, because the load balancer used internally by API Management is secure by default and rejects all inbound traffic. 
* Depending on whether you inject your API Management instance into a virtual network in external mode or internal mode, you may provide a Standard SKU [public IPv4 address](../virtual-network/ip-services/public-ip-addresses.md#sku) in addition to specifying a virtual network and subnet.
* The API Management service, virtual network and subnet, and public IP address resource (if provided) must be in the same region and subscription.
* For multi-region API Management deployments, configure virtual network resources separately for each location.


## Subnet size

The minimum size of the subnet in which API Management can be deployed is /29, which provides three usable IP addresses. Each extra scale [unit](api-management-capacity.md) of API Management requires two more IP addresses. The minimum size requirement is based on the following considerations:

* Azure reserves five IP addresses within each subnet that can't be used. The first and last IP addresses of the subnets are reserved for protocol conformance. Three more addresses are used for Azure services. For more information, see [Are there any restrictions on using IP addresses within these subnets?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets).

* In addition to the IP addresses used by the Azure virtual network infrastructure, each API Management instance in the subnet uses:
    * Two IP addresses per unit of Basic, Standard, or Premium SKU, or 
    * One IP address for the Developer SKU. 

* When deploying into an [internal virtual network](./api-management-using-with-internal-vnet.md), the instance requires an extra IP address for the internal load balancer.

> [!NOTE]
> When considering a subnet size, it is advisable to err on the side of caution due to the integral role that API Management typically holds. Consider growth and scaling in your sizing.

### Examples

The following table shows subnet sizing examples for API Management virtual network injection, illustrating how different CIDR blocks affect the number of scale-out units possible:

| Subnet CIDR | Total IP addresses | Azure reserved IPs | API Management instance IPs | Internal load balancer IP | Remaining IPs for scale-out | Max scale-out units | Total max units |
|------------:|-------------------:|-------------------:|----------------------------:|--------------------------:|----------------------------:|--------------------:|----------------:|
|        /29  |                  8 |                  5 |                           2 |                         1 |                           0 |                   0 |               1 |
|        /28  |                 16 |                  5 |                           2 |                         1 |                           8 |                   4 |               5 |
|        /27  |                 32 |                  5 |                           2 |                         1 |                          24 |                  12 |              13 |
|        /26  |                 64 |                  5 |                           2 |                         1 |                          56 |                  28 |              29 |
|        /25  |                128 |                  5 |                           2 |                         1 |                         120 |                 60* |             61* |

## Key Points

- **Minimum subnet size**: /29 (provides 3 usable IP addresses for API Management)
- **Azure reserved IPs**: 5 addresses per subnet (first and last for protocol conformance, plus 3 for Azure services)
- **Scale-out requirement**: Each scale-out unit requires 2 IP addresses
- **Internal load balancer**: Only required when API Management is deployed in internal virtual network mode
- **Premium SKU limit**: * Currently supports up to 31 units maximum
- **Recommended sizing**: For high-scale scenarios approaching the Premium SKU limit, consider /26 or /25 subnets

> [!NOTE]
> It is currently possible to scale the Premium SKU to 31 units. If you foresee demand approaching this limit, consider the /26 subnet or /25 subnet.

> [!IMPORTANT]
> The private IP addresses of internal load balancer and API Management units are assigned dynamically. Therefore, it is impossible to anticipate the private IP of the API Management instance prior to its deployment. Additionally, changing to a different subnet and then returning might cause a change in the private IP address.

## Routing

See the Routing guidance when deploying your API Management instance into an [external virtual network](./api-management-using-with-vnet.md#routing) or [internal virtual network](./api-management-using-with-internal-vnet.md#routing).

Learn more about the [IP addresses of API Management](api-management-howto-ip-addresses.md).

## DNS

* In external mode, the virtual network enables [Azure-provided name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) by default for your API Management endpoints and other Azure resources. It doesn't provide name resolution for on-premises resources. Optionally, configure your own DNS solution.

* In internal mode, you must provide your own DNS solution to ensure name resolution for API Management endpoints and other required Azure resources. We recommend configuring an Azure [private DNS zone](../dns/private-dns-overview.md).

For more information, see the DNS guidance when deploying your API Management instance into an [external virtual network](./api-management-using-with-vnet.md#routing) or [internal virtual network](./api-management-using-with-internal-vnet.md#routing).

Related information: 
* [Name resolution for resources in Azure virtual networks](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).  
* [Create an Azure private DNS zone](../dns/private-dns-getstarted-portal.md)

> [!IMPORTANT]
> If you plan to use a custom DNS solution for the VNet, set it up **before** deploying an API Management service into it. Otherwise, you'll need to update the API Management service each time you change the DNS server(s) by running the [Apply Network Configuration Operation](/rest/api/apimanagement/current-ga/api-management-service/apply-network-configuration-updates), or by selecting **Apply network configuration** in the service instance's network configuration window in the Azure portal.

## Limitations


* A subnet containing API Management instances can't be moved across subscriptions.
* For multi-region API Management deployments configured in internal virtual network mode, users own the routing and are responsible for managing the load balancing across multiple regions.
* To import an API to API Management from an [OpenAPI specification](import-and-publish.md), the specification URL must be hosted at a publicly accessible internet address.


## Related content

* [Site-to-site VPN](../vpn-gateway/design.md#s2smulti)
* [Connect virtual networks from different deployment models using PowerShell](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Azure Virtual Network frequently asked questions](../virtual-network/virtual-networks-faq.md)




