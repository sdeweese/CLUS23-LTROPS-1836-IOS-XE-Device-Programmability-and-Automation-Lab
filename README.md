# Welcome to the Cisco Live 22 IOS XE Programmablity Lab

### This lab guide serves as the Cisco Live US (Las Vegas) Programmability and Automation Lab with Catalyst IOS XE Platforms session LTROPS-1836


# Lab Introduction
To access the lab, you will need to use a Remote Desktop connection to the specific jump host. The jump host is used to allow remotes access into all lab devices within the given pod envrionment.

The services, features, and technologies that are enabled with the lab envrionment are shown below:

![](./imgs/pod_details.png)

The modules below enable IOS XE Device Lifecycle Management:

![](./imgs/device_lifecycle.png)

# Lab Modules

Lab modules can be completed in any order. Mark the lab completed in the [SmartSheet](https://app.smartsheet.com/b/form/134240eac2d84a57acd4efc24fd8f3d0) form once you have successfully completed each module. 



## [ZTP](ZTP.md)
Use the NETCONF API with Guest Shell to create the base configurations for a switch to implement Zero Touch Provisioning.



## [gNOI reset.proto Module](gNOI_reset_proto.md)
Reset.proto also known as the Factory Reset API is the latest addition to the gNOI operations interface within the gNMI.

The factory reset API as described at [openconfig/gnoi](https://github.com/openconfig/gnoi/blob/master/factory_reset/​) with tooling from [google/gnxi](https://github.com/google/gnxi/tree/master/gnoi_reset).



## [YANG Suite Module](YANG_Suite.md)
[YANG Suite](https://github.com/CiscoDevNet/yangsuite) is HTML5 based tooling that is available for working with the YANG based programmable interfaces on Cisco IOS XE, XR, and NX Network Operating Systems. It has plugins that allow for interacting with the programmable interfaces and supports downloading YANG files directly from network devices. In this module, we will explore using NETCONF and RESTCONF to configure a switch and we will create a gRPC telemetry subscriptions.



## [Secure Streaming Telemetry Module](Secure_Streaming_Telemetry.md)
< link to mTLS lab guide >

## [Terraform Module](Terraform.md)
Terraform is an Infrastructure as Code (IaC) tooling that allows network operators to easily view operational data, configure devices and manage network resources​. Since Terraform is cloud native, it works well with Cisco IOS XE cloud native solutions for routing, switching, and wireless platforms including the Cisco Catalyst 9000 Family switches, the Cisco Catalyst 8000V (virtual) router and the Cisco Wireless LAN Controller (WLC) 9800-CL (CL stands for “Cloud”). As well as easily managing cloud-native solutions, Terraform can also configure campus solutions. With Cisco IOS XE, we can automate with any tooling on any interface.

Terraform is declarative, meaning that it defines the desired state. It has commercial support from HashiCorp.

IOS XE Terraform utilizes RESTCONF + YANG to configure devices using a single binary file. To build out the RESTCONF payload, we can use the CLI2YANG feature. In Cisco IOS XE Cupertino 17.7.1 and later releases, you can automatically translate IOS commands into relevant NETCONF-YANG XML or RESTCONF-JSON request messages. You can analyze the generated configuration messages and familiarize with the Xpaths used in these messages. The generated configuration in the structured format can be used to provision other devices in the network; however, this configuration cannot be modified.

In this module, we will use CLI2YANG to create a Terraform payload to configure a 9300 device.







# Feedback
 Mark the lab completed and provide any feedback and comments in the [SmartSheet](https://app.smartsheet.com/b/form/134240eac2d84a57acd4efc24fd8f3d0) form once you have successfully completed the modules above. 
