---
title: Feature Comparison of Always On VPN and DirectAccess
description: In previous versions of the Windows VPN architecture, platform limitations made it difficult to provide the critical functionality needed to replace DirectAccess (like automatic connections initiated before users sign in).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: 
ms.assetid: 8fe1c810-4599-4493-b4b8-73fa9aa18535
manager: brianlic
ms.author: pashort
author: shortpatti
---

# Feature Comparison of Always On VPN and DirectAccess

>Applies To: Windows Server \(Semi-Annual Channel\), Windows Server 2016, Windows 10

In previous versions of the Windows VPN architecture, platform limitations made it difficult to provide the critical functionality needed to replace DirectAccess (like automatic connections initiated before users sign in). Always On VPN, however, has mitigated most of those limitations or expanded the VPN functionality beyond the capabilities of DirectAccess. Always On VPN addresses the previous gaps between Windows VPNs and DirectAccess; therefore, Always On VPN is the DirectAccess replacement solution.

This section covers feature similarities and differences between DirectAccess and Always On VPN. This list is not exhaustive, but it does include some of the most common features and functions of DirectAccess. The features and scenarios discussed fall into three categories:

-   **Equivalent functionality.** These are scenarios and features used in DirectAccess that have a directly related capability in Always On VPN. Where possible, this guide provides the configuration service provider (CSP) parameter for configuring each option so that you're aware of the XML setting name.

-   **Improved functionality.** These are situations or scenarios in which Always On VPN provides improved functionality over DirectAccess or fills a gap in functionality. Where possible, this guide provides the CSP parameter for configuring each option so that you're aware of the XML setting name.

-   **Limited comparable functionality.** In a few situations, Always On VPN requires an alternate way to incorporate existing functionality. This section discusses those changes and how you can gain the functionality with Always On VPN.

## Equivalent functionality

Each item in this section is a use case scenario or commonly used DirectAccess feature for which Always On VPN has an equivalent.

| DirectAccess functionality | Always On VPN equivalent |
| ---- | ---- |
| Seamless, transparent connectivity to the corporate network. | You can configure Always On VPN to support auto-triggering based on application launch or namespace resolution requests; you can also set it for permanent connection through Always On VPN.<br><br>Define this functionality by using the following VPNv2 CSP parameters:<br><br>**VPNv2/ProfileName/AlwaysOn**<br>**VPNv2/ProfileName/AppTriggerList**<br>**VPNv2/ProfileName/DomainNameInformationList/AutoTrigger** |
| Use of a dedicated Infrastructure Tunnel to provide connectivity to the corporate network when the user is not signed in. | You can achieve this functionality by using the Device Tunnel feature in the VPN profile.<br><br>Define this functionality by using the following VPNv2 CSP parameter:<br><br>**VPNv2/ProfileName/DeviceTunnel** |
| Use of manage-out to allow remote connectivity to DirectAccess-connected clients from management clients located on the corporate network. | You can achieve this functionality by using the Device Tunnel feature in the VPN profile combined with configuring the VPN connection to dynamically register the IP addresses assigned to the VPN interface with internal DNS services.<br><br>Define this functionality by using the following VPNv2 CSP parameter:<br><br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/RegisterDNS** |
| Fall back to IP-HTTPS when DirectAccess clients are behind firewalls or proxy servers. | You can configure to fall back to SSTP (from IKEv2) by using the automatic tunnel/protocol type within the VPN profile.<br><br>Define this functionality by using the following VPNv2 CSP parameter:<br><br>**VPNv2/ProfileName/NativeProfile/NativeProtocolType** |
| Support for DirectAccess end-to-edge access mode. | Always On VPN provides connectivity to corporate resources by using tunnel policies that require authentication and encryption until they reach the VPN gateway. By default, the tunnel sessions terminate at the VPN gateway, which also functions as the IKEv2 gateway, providing end-to-edge security. |
| Support for limited access to specific application servers using DirectAccess end-to-end access mode. | DirectAccess provides the ability to extend end-to-edge IPsec policies all the way to specified application servers. You can configure similar capability in Always On VPN by using an IPsec transport policy tunneled inside the end-to-edge VPN tunnel. The VPN gateway then forwards the authenticated and traffic-protected IPsec sessions to the application servers. Also, you can employ traffic filters to limit connectivity to specific destinations across the Always On VPN connection, providing end-to-end security to specific servers. |
| Support for load balancing using Windows Network Load Balancing (NLB) or external load balancers. | Always On VPN supports the use of integrated Windows NLB or third-party hardware load-balancing solutions to provide VPN load-balancing capabilities. Both IKEv2 and SSTP tunnel types/protocols can be successfully load balanced. |
| Support for machine certificate authentication. | The IKEv2 protocol type available as part of the Always On VPN platform specifically supports the use of authentication by machine or computer certificates for VPN authentication.<br><br>Define this functionality by using the following VPNv2 CSP parameter:<br><br> **VPNv2/ProfileName/NativeProfile/Authentication/MachineMethod** |
| Use DirectAccess security groups to limit remote access functionality to specific clients. | You can configure Always On VPN to support granular authorization when using RADIUS, which includes the use of security groups to control VPN access. |
| Support for DirectAccess servers behind an edge firewall or NAT device. | Always On VPN gives you the ability to use protocols like IKEv2 and SSTP that fully support the use of a VPN gateway that is behind a NAT device or edge firewall. |
| Use of NLS to determine intranet connectivity when connected to the corporate network. | Trusted network detection provides a similar capability to DirectAccess NLS for detecting corporate network connections, but it is based on an assessment of the connection-specific DNS suffix assigned to network interfaces and network profile rather than depending on a web service probe mechanism and associated internal web server infrastructure to function.<br><br>Define this functionality by using the following VPNv2 CSP parameter:<br><br>**VPNv2/ProfileName/TrustedNetworkDetection** |
| DirectAccess compliance using Network Access Protection (NAP). | The Always On VPN client can integrate with Azure conditional access to enforce MFA, device compliance, or a combination of both. When compliant with conditional access policies, Azure AD issues a short-lived (by default, 60 minutes) IPsec authentication certificate that the client can then use to authenticate to the VPN gateway. Device compliance takes advantage of System Center Configuration Manager/Intune compliance policies, which can include the device health attestation state. At this time, Azure VPN conditional access provides the closest replacement to the existing NAP solution, although there is no form of remediation service or quarantine network capabilities.<br><br>Define this functionality by using the following VPNv2 CSP parameter:<br><br>**VPNv2/ProfileName/DeviceCompliance** |
| Ability to define which management servers are accessible before user sign-in. | You can achieve this functionality in Always On VPN by using the Device Tunnel feature (available in version 1709) in the VPN profile combined with traffic filters to control which management systems on the corporate network are accessible through the Device Tunnel.<br><br>Define this functionality by using the following VPNv2 CSP parameters:<br><br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/TrafficFilterList** |
| Monitoring of DirectAccess operational status. | Although not strictly specific to Always On VPN, the Windows Server 2016 Remote Access Management console natively provides the operational status of VPN services. |
| Monitoring of DirectAccess remote access client status. | Although not strictly specific to the Always On VPN, the Windows Server 2016 Remote Access Management console natively provides the connection status of Always On VPN clients.  |
| Use of Inbox or RADIUS accounting for reporting of DirectAccess connections. | Although not strictly specific to Always On VPN, the Windows Server 2016 Remote Access Management console natively provides reporting for Always On VPN clients. |

## Improved functionality

Each item in this section is a use case scenario or commonly used DirectAccess function for which Always On VPN has improved functionality—either through an expansion of functionality or elimination of a previous limitation.

| DirectAccess functionality | Always On VPN equivalent |
| ---- | ---- |
| Domain-joined devices with Enterprise SKUs requirement. | Always On VPN supports domain-joined, nondomain-joined (workgroup), or Azure AD–joined devices to allow for both enterprise and BYOD scenarios. Always On VPN is available in all Windows editions, and the platform features are available to third parties by way of UWP VPN plug-in support. |
| DirectAccess clients depend on IPv6 and associated IPv6 translation services to function. | With Always On VPN, users can access both IPv4 and IPv6 resources on the corporate network. The Always On VPN client uses a dual-stack approach that doesn't specifically depend on IPv6 or the need for the VPN gateway to provide NAT64 or DNS64 translation services. |
| Support for two-factor or OTP authentication. | The Always On VPN platform natively supports EAP, which allows for the use of diverse Microsoft and third-party EAP types as part of the authentication workflow. Always On VPN specifically supports smart card (both physical and virtual) and Windows Hello for Business certificates to satisfy two-factor authentication requirements. Also, Always On VPN supports OTP and MFA by way of EAP RADIUS integration.<br><br>Define this functionality by using the following VPNv2 CSP parameter:<br><br>**VPNv2/ProfileName/NativeProfile/Authentication** |
| Support for multiple domains and forests. | The Always On VPN platform has no dependency on Active Directory Domain Services (AD DS) forests or domain topology (or associated functional/schema levels) because it doesn't require the VPN client to be domain joined to function. Group Policy is therefore not a dependency to define VPN profile settings because you do not use it during client configuration. Where Active Directory authorization integration is required, you can achieve it through RADIUS as part of the EAP authentication and authorization process. |
| Support for both split and force tunnel for internet/intranet traffic separation. | You can configure Always On VPN to support both force tunnel (the default operating mode) and split tunnel natively. Always On VPN provides additional granularity for application-specific routing policies.<br><br>Define this functionality by using the following VPNv2 CSP parameters:<br><br> **VPNv2/ProfileName/NativeProfile/RoutingPolicyType**<br>**VPNv2/ProfileName/TrafficFilterList/App/RoutingPolicyType** |
| Support for IP-HTTPS transition technology. | Always On VPN does not require the use of an IPv6 encapsulation protocol like IP-HTTPS. You can configure it to support SSTP natively if Secure Sockets Layer fallback from IKEv2 is required. |
| Support for the DirectAccess Connectivity Assistant to provide corporate connectivity status. | Always On VPN is fully integrated with the native Network Connectivity Assistant and provides connectivity status from the View All Networks interface. With the advent of Windows 10 Creators Update (version 1703), VPN connection status and VPN connection control are now available through the Network flyout, as well. |
| Name resolution of corporate resources using short-name, fully qualified domain name (FQDN), and DNS suffix. | Always On VPN can natively define one or more DNS suffixes as part of the VPN connection and IP address assignment process, including corporate resource name resolution for short names, FQDNs, or entire DNS namespaces. Always On VPN also supports use of Name Resolution Policy Tables to provide namespace-specific resolution granularity.<br><br>Define this functionality by using the following VPNv2 CSP parameters:<br><br>**VPNv2/ProfileName/DnsSuffix**<br>**VPNv2/ProfileName/DomainNameInformationList** |

## Limited comparable functionality

Each item in this section is a use case scenario or commonly used DirectAccess function for which Always On VPN does not have an equivalent natively. Although these are limited functionalities for Always On VPN, you can implement these functionalities in other ways as mentioned in the table below.

| DirectAccess functionality | Always On VPN equivalent |
| ---- | ---- |
| Use of multisite to provide multiple remote access entry points and geo-redundancy. | <!-- pashort 2/15/2018: eventually re-write this column for the two; let's make some sense out of these to prevent user confusion --> No native multisite-equivalent feature exists in Always On VPN without the use of third-party networking equipment or services such as Azure Traffic Manager or a third-party global server load balancer. However, users can manually select an appropriate VPN endpoint if you define multiple entries in the VPN profile. Third-Party UWP VPN plug-ins may support similar features for connecting to the nearest or most appropriate server VPN endpoint, but this varies by provider. |
| Deployment of client and server configuration settings through Group Policy. | Always On VPN does not require devices to be domain joined, so there are no dedicated Group Policy settings to configure it. Instead, you can configure clients by using Windows PowerShell, System Center Configuration Manager, Intune (or a third-party MDM provider), or Windows Configuration Designer. Also, because there's no dependency on a Microsoft VPN gateway, you must configure and manage server settings independent of the Always On VPN. |

