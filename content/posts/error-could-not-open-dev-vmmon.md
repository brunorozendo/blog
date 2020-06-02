---
title: "Error could not open dev vmmon"
description: "solução para o erro `error could not open dev vmmon`"
author:
  name: "Bruno Rozendo"
date: 2015-02-19
draft: false
tags:
- "vmware"
comments: true
image: "/images/posts/error_vmware.png"
---




O problema aparece ao iniciar o VMware Workstation 10 ou VMWare Player. 

> Could not open /dev/vmmon: No such file or directory.
> Please make sure that the kernel module `vmmon' is loaded.

Esse erro aparece em *unix mas  para resolve-lo é fácil.

{{< terminal >}}{{< highlight bash >}}$ sudo vmware-modconfig --console --install-all
{{< /highlight >}}
{{< /terminal >}}



A saida deve ser algo parecido com:

{{< terminal >}}{{< highlight bash >}}Stopping VMware services:
   VMware Authentication Daemon                                        done
   VM communication interface socket family                            done
   Virtual machine communication interface                             done
   Virtual machine monitor                                             done
   Blocking file system                                                done
make: Entering directory '/tmp/modconfig-mQeEZQ/vmmon-only'
Using kernel build system.
/sbin/make -C /lib/modules/3.18.5-1-ARCH/build/include/.. SUBDIRS=$PWD SRCROOT=$PWD/. \
  MODULEBUILDDIR= modules
make[1]: Entering directory '/usr/lib/modules/3.18.5-1-ARCH/build'
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/linux/driverLog.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/linux/driver.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/linux/hostif.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/common/memtrack.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/common/apic.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/common/hashFunc.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/common/vmx86.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/common/cpuid.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/common/task.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/common/comport.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/common/phystrack.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmmon-only/vmcore/moduleloop.o
/tmp/modconfig-mQeEZQ/vmmon-only/linux/driver.c:1332:1: warning: always_inline function might not be inlinable [-Wattributes]
 LinuxDriverSyncReadTSCs(uint64 *delta) // OUT: TSC max - TSC min
 ^
  LD [M]  /tmp/modconfig-mQeEZQ/vmmon-only/vmmon.o
  Building modules, stage 2.
  MODPOST 1 modules
  CC      /tmp/modconfig-mQeEZQ/vmmon-only/vmmon.mod.o
  LD [M]  /tmp/modconfig-mQeEZQ/vmmon-only/vmmon.ko
make[1]: Leaving directory '/usr/lib/modules/3.18.5-1-ARCH/build'
/sbin/make -C $PWD SRCROOT=$PWD/. \
  MODULEBUILDDIR= postbuild
make[1]: Entering directory '/tmp/modconfig-mQeEZQ/vmmon-only'
make[1]: 'postbuild' is up to date.
make[1]: Leaving directory '/tmp/modconfig-mQeEZQ/vmmon-only'
cp -f vmmon.ko ./../vmmon.o
make: Leaving directory '/tmp/modconfig-mQeEZQ/vmmon-only'
make: Entering directory '/tmp/modconfig-mQeEZQ/vmnet-only'
Using kernel build system.
/sbin/make -C /lib/modules/3.18.5-1-ARCH/build/include/.. SUBDIRS=$PWD SRCROOT=$PWD/. \
  MODULEBUILDDIR= modules
make[1]: Entering directory '/usr/lib/modules/3.18.5-1-ARCH/build'
  CC [M]  /tmp/modconfig-mQeEZQ/vmnet-only/driver.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmnet-only/userif.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmnet-only/hub.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmnet-only/netif.o
In file included from include/linux/pci.h:34:0,
                 from /tmp/modconfig-mQeEZQ/vmnet-only/compat_netdevice.h:27,
                 from /tmp/modconfig-mQeEZQ/vmnet-only/netif.c:43:
include/linux/pci_ids.h:2248:0: warning: "PCI_VENDOR_ID_VMWARE" re-definido
 #define PCI_VENDOR_ID_VMWARE  0x15ad
 ^
In file included from /tmp/modconfig-mQeEZQ/vmnet-only/net.h:38:0,
                 from /tmp/modconfig-mQeEZQ/vmnet-only/vnetInt.h:26,
                 from /tmp/modconfig-mQeEZQ/vmnet-only/netif.c:42:
/tmp/modconfig-mQeEZQ/vmnet-only/vm_device_version.h:56:0: note: essa é a localização da definição anterior
 #define PCI_VENDOR_ID_VMWARE                    0x15AD
 ^
  CC [M]  /tmp/modconfig-mQeEZQ/vmnet-only/bridge.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmnet-only/procfs.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmnet-only/smac_compat.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmnet-only/smac.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmnet-only/vnetEvent.o
  CC [M]  /tmp/modconfig-mQeEZQ/vmnet-only/vnetUserListener.o
In file included from /tmp/modconfig-mQeEZQ/vmnet-only/net.h:38:0,
                 from /tmp/modconfig-mQeEZQ/vmnet-only/vnetInt.h:26,
                 from /tmp/modconfig-mQeEZQ/vmnet-only/bridge.c:52:
/tmp/modconfig-mQeEZQ/vmnet-only/vm_device_version.h:56:0: warning: "PCI_VENDOR_ID_VMWARE" re-definido
 #define PCI_VENDOR_ID_VMWARE                    0x15AD
 ^
In file included from include/linux/pci.h:34:0,
                 from /tmp/modconfig-mQeEZQ/vmnet-only/compat_netdevice.h:27,
                 from /tmp/modconfig-mQeEZQ/vmnet-only/bridge.c:51:
include/linux/pci_ids.h:2248:0: note: essa é a localização da definição anterior
 #define PCI_VENDOR_ID_VMWARE  0x15ad
 ^
  LD [M]  /tmp/modconfig-mQeEZQ/vmnet-only/vmnet.o
  Building modules, stage 2.
  MODPOST 1 modules
  CC      /tmp/modconfig-mQeEZQ/vmnet-only/vmnet.mod.o
  LD [M]  /tmp/modconfig-mQeEZQ/vmnet-only/vmnet.ko
make[1]: Leaving directory '/usr/lib/modules/3.18.5-1-ARCH/build'
/sbin/make -C $PWD SRCROOT=$PWD/. \
  MODULEBUILDDIR= postbuild
make[1]: Entering directory '/tmp/modconfig-mQeEZQ/vmnet-only'
make[1]: 'postbuild' is up to date.
make[1]: Leaving directory '/tmp/modconfig-mQeEZQ/vmnet-only'
cp -f vmnet.ko ./../vmnet.o
make: Leaving directory '/tmp/modconfig-mQeEZQ/vmnet-only'
Starting VMware services:
   Virtual machine monitor                                             done
   Virtual machine communication interface                             done
   VM communication interface socket family                            done
   Blocking file system                                                done
   Virtual ethernet                                                    done
   VMware Authentication Daemon                                        done
   Shared Memory Available                                             done

{{< /highlight >}}
{{< /terminal >}}



## referências 

[nowhereLAN](http://blog.nowherelan.com/2014/12/03/vmware-workstation-10-error-could-not-open-devvmmon-no-such-file-or-directory-please-make-sure-that-the-kernel-module-vmmon-is-loaded/)
