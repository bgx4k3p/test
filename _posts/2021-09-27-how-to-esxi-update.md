---
date: 2021-09-27 12:26
layout: post
title: Install ESXi updates from command line
subtitle: Quick notes
description: How to install ESXi updates from command line
image: https://bgx4k3p.github.io/test/assets/img/esxi.jpg
category: linux
tags: vmware esxi
author: bgx4k3p
paginate: true
---


## 1. Put the ESXi server in Maintenance mode

You can install the updates without Maintenance mode, but it's better to be on the safe side. Power off all runnings VMs and run the commands below:

```bash
esxcli system maintenanceMode set --enable=true
esxcli system maintenanceMode get 
```

## 2. Allow outbound HTTP requests  

```bash
esxcli network firewall ruleset set -e true -r httpClient
```

## 3. Get al ist of all available ESXi profiles  

```bash
esxcli software sources profile list --depot=https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml |grep ESXi
-7|grep standard

# Output
ESXi-7.0.0-15843807-standard      VMware, Inc.  PartnerSupported  2020-03-16T10:48:54  2020-03-16T10:48:54
ESXi-7.0bs-16321839-standard      VMware, Inc.  PartnerSupported  2020-06-02T05:57:00  2020-06-02T05:57:00
ESXi-7.0b-16324942-standard       VMware, Inc.  PartnerSupported  2020-06-02T17:26:43  2020-06-02T17:26:43
ESXi-7.0U1b-17168206-standard     VMware, Inc.  PartnerSupported  2020-11-11T11:34:51  2020-11-11T11:34:51
ESXi-7.0U1d-17551050-standard     VMware, Inc.  PartnerSupported  2021-02-01T18:29:07  2021-02-01T18:29:07
ESXi-7.0U1c-17325551-standard     VMware, Inc.  PartnerSupported  2020-12-15T12:44:19  2020-12-15T12:44:19
ESXi-7.0U1a-17119627-standard     VMware, Inc.  PartnerSupported  2020-11-01T08:18:49  2020-11-01T08:18:49
ESXi-7.0.1-16850804-standard      VMware, Inc.  PartnerSupported  2020-09-04T18:28:17  2020-09-04T18:28:18
ESXi-7.0U1sc-17325020-standard    VMware, Inc.  PartnerSupported  2020-12-15T10:50:21  2020-12-15T10:50:21
ESXi-7.0U2a-17867351-standard     VMware, Inc.  PartnerSupported  2021-04-29T00:00:00  2021-04-29T00:00:00
ESXi-7.0U2sc-18295176-standard    VMware, Inc.  PartnerSupported  2021-08-24T00:00:00  2021-08-24T00:00:00
ESXi-7.0U2c-18426014-standard     VMware, Inc.  PartnerSupported  2021-08-24T00:00:00  2021-08-24T00:00:00
ESXi-7.0U2d-18538813-standard     VMware, Inc.  PartnerSupported  2021-09-14T00:00:00  2021-09-14T00:00:00
```

## 4. Dry Run

Select the latest one based on date and do dry run (optional)

```bash
esxcli software profile update -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml -p ESXi-7.0U2d-18538813-standard --dry-run
```

## 5. Install Update

```bash
esxcli software profile update -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml -p ESXi-7.0U2d-18538813-standard
```

## 6. Disable outbound HTTP requests  

```bash
esxcli network firewall ruleset set -e false -r httpClient
```

## 7. Reboot

## 8. Troubleshooting

Sometimes this error occurs during update and vmware-tools

```bash
# Common error
[InstallationError]
[Errno 28] No space left on device
       vibs = VMware_locker_tools-light_11.0.5.15389592-15999342

# Install without tools
esxcli software profile update -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml -p ESXi-6.7.0-20191204001-no-tools

# Install the tools after
esxcli software vib install -v https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/esx/vmw/vib20/tools-light/VMware_locker_tools-light_11.0.5.15389592-15999342.vib

```

<https://my.vmware.com/group/vmware/downloads/details?downloadGroup=VMTOOLS1120-OSS&productId=1073>

Another issue is you have an older CPU, just add the extra flag to disable the warning

```bash
[HardwareError]
 Hardware precheck of profile ESXi-7.0U2d-18538813-standard failed with warnings: <CPU_SUPPORT WARNING: The CPU in this host may not be supported in future ESXi releases. Please plan accordingly.>
 
 Apply --no-hardware-warning option to ignore the warnings and proceed with the transaction.

```
