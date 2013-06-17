ax88179_178a
============

ASIX AX88179_178A Linux Driver Source Code

============================================================================
ASIX AX88179 USB 3.0 Gigabit Ethernet Network Adapter
Driver Compilation & Configuration on the Linux
============================================================================

==================
Revision History :
==================
Version 1.4.0		Feb. 26, 2013
 * Fixed RX checksum errors issue
 * Fixed RX hungup issue during long time iperf heavy traffic test
 * Support Big-endian systems
 * Fixed the default MTU to 1500
 * Added the ax88179_change_mtu function to support Jumbo Frame
 * Removed linkup variable from the driver private data structure.
 * Added netif_carrier_on function in ax88179_status.
 * Added bsize and ifg driver parameters. 
 * Removed AX_Suspend and AX_Resume

Version 1.3.0		Dec. 13, 2012
 * Modified bulk in parameter to improve the throughput on ARM.
 * Fixed suspend/resume issue on ARM.
 * Fixed down/up network interface issue on ARM.
 * Added Sitecom VID:DID 0df6:0072.
 * Removed checking mii_err in rx_fixup function to 
   avoid packet loss under some circumstances.
 * Modified the ax88179_link_reset function.

Version 1.2.0		Aug. 28, 2012
 * Modified the ax88179_link_reset function.
 * Implemented the auto detach function.
 * Supported the AX88178A device.
 * Fixed the media change issue.
 * Fixed the wakeup issue by modified the clock select register.

Version 1.1.0		May. 08, 2012
 * Modified the ax88179_led_workaround function.
 * Fixups the ethtool_ops struct for compilation issues in newest kernel version 3.3.0. 
 * Fixups the net_device_ops struct for compilation issues in newest kernel version 3.2.0.

Version	1.0.0 Beta	Jan. 4, 2012
 * Initial release.

================
Prerequisites
================

Prepare to build the driver, you need the Linux kernel sources installed on the
build machine, and make sure that the version of the running kernel must match
the installed kernel sources. If you don't have the kernel sources, you can get
it from www.kernel.org or contact to your Linux distributor. If you don't know
how to do, please refer to KERNEL-HOWTO.

Note: Please make sure the kernel is built with one of the "Support for
       Host-side, EHCI, OHCI, or UHCI" option support.

================
File Description
================
README		This file
ax88179.c	AX88179 Linux driver main file
ax88179.h	AX88179 Linux driver header file
Makefile	AX88179 driver make file
dkms.conf       Configuration for DKMS support
COPYING	GNU GERNERAL LICENSE

===========================
Conditional Compilation Flag
===========================

================
Getting Start
================

1. Extract the compressed driver source file to your template directory by the
   following command:

	[root@localhost template]# tar -xf DRIVER_SOURCE_PACKAGE.tar.bz2

2. Now, the driver source files should be extracted under the current directory.
   Executing the following command to compile the driver:
 
	[root@localhost template]# make
			
3. If the compilation is well, the asix.ko will be created under the current
   directory.
 
4. If you want to use modprobe command to mount the driver, executing the
   following command to install the driver into your Linux:

	[root@localhost template]# make install


================
Usage
================

1. If you want to load the driver manually, go to the driver directory and
   execute the following commands:

	[root@localhost template]# insmod ax88179.ko

2. If you had installed the driver during driver compilation, then you can use
   the following command to load the driver automatically.

	[root@localhost anywhere]# modprobe ax88179

If you want to unload the driver, just executing the following command:

	[root@localhost anywhere]# rmmod ax88179

===============
DKMS support
===============

If you want the driver being compiled each time a new kernel is installed,
you need to enable DKMS support.

1. Make sure that version in dkms.conf matches that in ax88179_178a.c:

	[root@localhost template]# grep DRV_VERSION ax88179_178a.c
	[root@localhost template]# grep PACKAGE_VERSION dkms.conf

2. Create the source directory for DKMS (must be named module-version):

	[root@localhost template]# mkdir /usr/src/ax88179_178a-1.4.0

3. Copy all the module source files to the source directory:

	[root@localhost template]# cp * /usr/src/ax88179_178a-1.4.0

4. Enable, build and install DKMS:

	[root@localhost anywhere]# dkms add -m ax88179_178a -v 1.4.0
	[root@localhost anywhere]# dkms build -m ax88179_178a -v 1.4.0
	[root@localhost anywhere]# dkms install -m ax88179_178a -v 1.4.0

5. Now the module should load automatically if the device is plugged in,
   but you can also load it manually:

	[root@localhost anywhere]# modprobe ax88179_178a

6. Whenever a new kernel is installed, the module will be installed too.

===============
DRIVER PARAMETERS
===============
The following parameters can be set when using insmod.

msg_enable=0xNNNNNNN
	specifies the msg_enable of usbnet.

example: insmod ax88179.ko msg_enable=0x00000000
