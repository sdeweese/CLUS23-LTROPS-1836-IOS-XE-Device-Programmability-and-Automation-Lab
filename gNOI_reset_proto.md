## **[IOS XE Programmability Lab](https://github.com/jeremycohoe/cisco-ios-xe-programmability-lab)**

## **Module: gNOI reset.proto "factory reset" API**

## Version: Cisco IOS XE 17.8

![](imgs/device_lifecycle.png)

## Factory Reset API

Reset.proto also known as the Factory Reset API is the latest addition to the gNOI operations interface within the gNMI.

The factory reset API as described at [openconfig/gnoi](https://github.com/openconfig/gnoi/tree/master/factory_reset) with tooling from [google/gnxi](https://github.com/google/gnxi/tree/master/gnoi_reset).

This module describes the capabilities of the gnoi_reset tooling which is used to factory reset Cisco Catalyst IOS XE devices like the Catalyst 9300 used in this example.

The console port is use to watch as the device completes the factory reset and reboots.

## Zero Touch Provisioning
As part of the Day 0 workflows, the ZTP process will be used to automatically configure the C9300 switch back to a configured state where the API's are ready for use once again. Once Factory reset is complete, we can use ZTP to add the necessary configurations to our device using NETCONF. To skip directly to learn more about ZTP, jump [here](ZTP.md).

# Enable the gNMI API Interface

In this example the gNMI API interface is enabled with the most basic and minimal configuration. 

c9300-pod28# ***show run | i gnxi***

We should see the following in the response from the show command:

gnxi

gnxi server


# Verify gNMI API Interface

There are two show CLI's to help understand the state and status of gNMI and the gNOI microservices

**show gnxi state**

and

**show gnxi state detail**

Output from each of these is seen below showing that the gNMI is State Enabled and Status Up, and that the three gNOI services are each Enabled/Up/Supported


```
c9300-pod29#show gnxi state
State            Status
--------------------------------
Enabled          Up


c9300-pod29#show gnxi state detail
Settings
========
  Server: Enabled
  Server port: 50052
  Secure server: Disabled
  Secure server port: 9339
  Secure client authentication: Disabled
  Secure trustpoint:
  Secure client trustpoint:
  Secure password authentication: Disabled

GNMI
====
  Admin state: Enabled
  Oper status: Up
  State: Provisioned

  gRPC Server
  -----------
    Admin state: Enabled
    Oper status: Up

  Configuration service
  ---------------------
    Admin state: Enabled
    Oper status: Up

  Telemetry service
  -----------------
    Admin state: Enabled
    Oper status: Up

GNOI
====

  Cert Management service
  -----------------
    Admin state: Enabled
    Oper status: Up

  OS Image service
  ----------------
    Admin state: Enabled
    Oper status: Up
    Supported: Supported

  Factory Reset service
  ---------------------
    Admin state: Enabled
    Oper status: Up
    Supported: Supported
```

# gnoi_reset tooling

As with previous gNOI examples the tooling from Github.com/google/gnxi is used to work with the API. The gnoi_reset tooling is provided that enables testing and validation of the gNOI reset.proto APIs

The tooling is installed using the **go get** and **go build** commands within the Linux environment. 

The following commands to get have already been done for you to set up gNOI.reset on the Linux VM:

***go get github.com/google/gnxi/gnoi_reset​***

***go install github.com/google/gnxi/gnoi_reset​***


# gnoi_reset and zero_fill

gNOI_reset clears all of the configurations on the disk of the Catatlyst 9000 and resets the device back to factory settings.

The **gnoi_reset** tooling can be called with some options to specify the target address, target name, TLS options, and the use of the zero_fill option.

Zero Fill is the process that instructs the target device to zero fill the persistent storage state data. All sectors on the disk are overwritten with zero's so data is found after the operation. 

On the Linux VM, run the following command to reset all of the configurations and disk back to factory settings:

`./gnoi_reset -target_addr 10.1.1.5:50052 -target_name dd -notls -zero_fill`

A message similar to this should appear: 
***I1014 11:53:15.248633 2761247 gnoi_reset.go:59] Reset Called Successfully!***


It will take the switch about 5 minutes to reload. To see an example of what that will look like, see the output below. In the meantime, review how to set up and use [ZTP](ZTP.md). Once the switch comes back up, we will automatically apply the changes in ZTP.

When the gnoi_reset factor reset operation is called then the device will go down for factory reset. The follow is displayed onto the serial console of the device which captures the log messages and shows the progress as the device is zero_filled and reset.

Enter the serial console on the Linux VM using the following: `console-helper`




NOTE: the following out put is simply shown for reference

```
C9300#
Chassis 1 reloading, reason - Factory Reset
Jun  8 16:34:42.825: %PMAN-5-EXITACTION: R0/0: pvp: Procger is exiting: rp processes exit with reload switch code


Enabling factory reset for this reload cycle
 Switch booted with flash:packages.conf
 Switch booted via packages.conf
FACTORY-RESET-RESTORE-IMAGE Taking backup of flash:packages.conf
FACTORY-RESET-RESTORE-IMAGE Searching for packages.conf on flash
factory-reset-restore-image taking a backup of packages.conf from flash
factory-reset-restore-image copying cat9k-cc_srdriver.17.08.01.SPA.pkg to /tmp/factory_reset
factory-reset-restore-image copying cat9k-espbase.17.08.01.SPA.pkg to /tmp/factory_reset
factory-reset-restore-image copying cat9k-guestshell.17.08.01.SPA.pkg to /tmp/factory_reset
factory-reset-restore-image copying cat9k-lni.17.08.01.SPA.pkg to /tmp/factory_reset
factory-reset-restore-image copying cat9k-rpbase.17.08.01.SPA.pkg to /tmp/factory_reset
factory-reset-restore-image copying cat9k-rpboot.17.08.01.SPA.pkg to /tmp/factory_reset
factory-reset-restore-image copying cat9k-sipbase.17.08.01.SPA.pkg to /tmp/factory_reset
factory-reset-restore-image copying cat9k-sipspa.17.08.01.SPA.pkg to /tmp/factory_reset
factory-reset-restore-image copying cat9k-srdriver.17.08.01.SPA.pkg to /tmp/factory_reset
factory-reset-restore-image copying cat9k-webui.17.08.01.SPA.pkg to /tmp/factory_reset
factory-reset-restore-image copying cat9k-wlc.17.08.01.SPA.pkg to /tmp/factory_reset
factory-reset-restore-image copying packages.conf to /tmp/factory_reset
% FACTORYRESET - Started Cleaning Up...

% FACTORYRESET - Unmounting sd1
% FACTORYRESET - Cleaning Up sd1 [0]
% FACTORYRESET - erase In progress.. please wait for completion...
% FACTORYRESET - write zero...
% FACTORYRESET - finish erase

% FACTORYRESET - Making File System sd1 [0]
Creating filesystem with 409600 4k blocks and 102544 inodes
Filesystem UUID: 967e9817-6cad-4a4a-96d4-b142fbc2111f
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Writing superblocks and filesystem accounting information: done

% FACTORYRESET - Mounting Back sd1 [0]
% FACTORYRESET - Handling Mounted sd1
% FACTORYRESET - Factory Reset Done for sd1

% FACTORYRESET - Unmounting sd3
% FACTORYRESET - Cleaning Up sd3 [0]
% FACTORYRESET - erase In progress.. please wait for completion...
% FACTORYRESET - write zero...
% FACTORYRESET - finish erase

% FACTORYRESET - Making File System sd3 [0]
Creating filesystem with 2816000 4k blocks and 704512 inodes
Filesystem UUID: 30856fa9-0d8e-4556-9cf0-81be81f0bcd1
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208

Allocating group tables: done
Writing inode tables: done
Writing superblocks and filesystem accounting information: done

% FACTORYRESET - Mounting Back sd3 [0]
% FACTORYRESET - Handling Mounted sd3
% FACTORYRESET - Factory Reset Done for sd3

% FACTORYRESET - Unmounting sd6
% FACTORYRESET - Cleaning Up sd6 [0]
% FACTORYRESET - erase In progress.. please wait for completion...
% FACTORYRESET - write zero...
% FACTORYRESET - finish erase

% FACTORYRESET - Making File System sd6 [0]
Creating filesystem with 131072 1k blocks and 32768 inodes
Filesystem UUID: 72c3f21a-11ed-4f2f-8619-33a014e139c6
Superblock backups stored on blocks:
	8193, 24577, 40961, 57345, 73729

Allocating group tables: done
Writing inode tables: done
Writing superblocks and filesystem accounting information: done

% FACTORYRESET - Mounting Back sd6 [0]
% FACTORYRESET - Handling Mounted sd6
% FACTORYRESET - Factory Reset Done for sd6
% FACTORYRESET - Lic Clean UP
% FACTORYRESET - Lic Clean Successful...
% FACTORYRESET - Clean Up Successful...
% FACTORYRESET - Check if usbflash0 is mounted...
% FACTORYRESET - Check if usbflash1 is mounted...
% FACTORYRESET - usbflash1 detected..
% FACTORYRESET - Proceed with Unmounting the USB3.0 SSD...
% FACTORYRESET - Cleaning Up /vol/usb1
% FACTORYRESET - In progress.. please wait for completion...
% FACTORYRESET - Making File System on SSD
Discarding device blocks: done
Creating filesystem with 29304945 4k blocks and 7331840 inodes
Filesystem UUID: 59801da9-4162-4b80-8401-cc237328fb35
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
	4096000, 7962624, 11239424, 20480000, 23887872

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done

% FACTORYRESET - Mounting Back usbflash1
% FACTORYRESET - Factory reset done for SSD
FACTORY-RESET-RESTORE-IMAGE Copying back image from /tmp/factory_reset onto /bootflash/
ReloadReason=Factory Reset
FACTORY-RESET-RESTORE-IMAGE Copying image is successful.
Factory reset successful. Rebooting...

Initializing Hardware......

System Bootstrap, Version 17.8.1r[FC2], RELEASE SOFTWARE (P)
Compiled Thu 02/24/2022 22:45:03.07 by rel

Current ROMMON image : Primary
Last reset cause     : SoftwareReload
C9300-24T platform with 8388608 Kbytes of main memory

boot: attempting to boot from [flash:packages.conf]
boot: reading file packages.conf
#
#############################################################################################################################################################################################################################################################################################################################################################################################################################################################################################################################################



Both links down, not waiting for other switches
Switch number is 1

              Restricted Rights Legend

Use, duplication, or disclosure by the Government is
subject to restrictions as set forth in subparagraph
(c) of the Commercial Computer Software - Restricted
Rights clause at FAR sec. 52.227-19 and subparagraph
(c) (1) (ii) of the Rights in Technical Data and Computer
Software clause at DFARS sec. 252.227-7013.

           Cisco Systems, Inc.
           170 West Tasman Drive
           San Jose, California 95134-1706



Cisco IOS Software [Cupertino], Catalyst L3 Switch Software (CAT9K_IOSXE), Version 17.8.1, RELEASE SOFTWARE (fc2)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2022 by Cisco Systems, Inc.
Compiled Sat 09-Apr-22 00:47 by mcpre


This software version supports only Smart Licensing as the software licensing mechanism.


PLEASE READ THE FOLLOWING TERMS CAREFULLY. INSTALLING THE LICENSE OR
LICENSE KEY PROVIDED FOR ANY CISCO SOFTWARE PRODUCT, PRODUCT FEATURE,
AND/OR SUBSEQUENTLY PROVIDED SOFTWARE FEATURES (COLLECTIVELY, THE
"SOFTWARE"), AND/OR USING SUCH SOFTWARE CONSTITUTES YOUR FULL
ACCEPTANCE OF THE FOLLOWING TERMS. YOU MUST NOT PROCEED FURTHER IF YOU
ARE NOT WILLING TO BE BOUND BY ALL THE TERMS SET FORTH HEREIN.

Your use of the Software is subject to the Cisco End User License Agreement
(EULA) and any relevant supplemental terms (SEULA) found at
http://www.cisco.com/c/en/us/about/legal/cloud-and-software/software-terms.html.

You hereby acknowledge and agree that certain Software and/or features are
licensed for a particular term, that the license to such Software and/or
features is valid only for the applicable term and that such Software and/or
features may be shut down or otherwise terminated by Cisco after expiration
of the applicable license term (e.g., 90-day trial period). Cisco reserves
the right to terminate any such Software feature electronically or by any
other means available. While Cisco may provide alerts, it is your sole
responsibility to monitor your usage of any such term Software feature to
ensure that your systems and networks are prepared for a shutdown of the
Software feature.


% Checking backup nvram
% No config present. Using default config

FIPS: Flash Key Check : Key Not Found, FIPS Mode Not Enabled
cisco C9300-24T (X86) processor with 1310600K/6147K bytes of memory.
Processor board ID FCW2129L03E
2048K bytes of non-volatile configuration memory.
8388608K bytes of physical memory.
1638400K bytes of Crash Files at crashinfo:.
11264000K bytes of Flash at flash:.

Base Ethernet MAC Address          : a0:f8:49:11:c5:80
Motherboard Assembly Number        : 73-17952-06
Motherboard Serial Number          : FOC212869N3
Model Revision Number              : A0
Motherboard Revision Number        : A0
Model Number                       : C9300-24T
System Serial Number               : FCW2129L03E
CLEI Code Number                   :


No startup-config, starting autoinstall/pnp/ztp...

Autoinstall will terminate if any input is detected on console
Jun  8 23:44:53.063: %IOX-3-IOX_RESTARTABITLITY: R0/0: run_ioxn_caf: Stack is in N+1 mode, disabling sync for IOx restartability


         --- System Configuration Dialog ---

Would you like to enter the initial configuration dialog? [yes/no]:



...[SNIP]...



Line 2 SUCCESS:  exit

*** Configure cli2yang examples on device... ***
*** Successfully configured cli2yang on device! ***

 About to enable NETCONF API...



 *** NETCONF API enabled... ***


<?xml version="1.0" ?>
<rpc-reply message-id="urn:uuid:daaa3709-49c6-437d-9f22-51f84aae8b6a" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0">
	<data>
		<native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
			<hostname>c9300-pod05</hostname>
		</native>
	</data>
</rpc-reply>
```

# Conclusion

The gNOI Factory Reset API, or reset.proto, can be used to easily factory reset IOS XE devices with a single API call.
