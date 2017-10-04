## Realtek 802.11ac (rtl8812au) for Orange Pi Pluse 2e - Ubuntu armbian 5.25 kernel 3.4.113

This is a fork of the Realtek 802.11ac (rtl8812au) v4.2.2 (7502.20130507)
driver altered to build on Linux kernel version >= 3.10. Thank to [gnab][https://github.com/gnab]

### Purpose

My wireless dual-band USB adapter needs the Realtek 8812au driver to work under Linux on kernel 3.4113 of armbian distro.

### Building

The Makefile is preconfigured to handle most x86/PC versions.  If you are compiling for something other than an intel x86 architecture, you need to first select the platform, e.g. for the Raspberry Pi, you need to set the I386 to n and the ARM_RPI to y:
```sh
...
CONFIG_PLATFORM_I386_PC = n
...
CONFIG_PLATFORM_ARM_OPI = y
```

There are many other platforms supported and some other advanced options, e.g. PCI instead of USB, but most won't be needed.

The driver is built by running `make`, and can be tested by loading the
built module using `insmod`:

```sh
$ make
$ sudo insmod 8812au.ko
```

After loading the module, a wireless network interface named __Realtek 802.11n WLAN Adapter__ should be available.

### Installing

Installing the driver is simply a matter of copying the built module
into the correct location and updating module dependencies using `depmod`:

```sh
$ sudo cp 8812au.ko /lib/modules/$(uname -r)/kernel/drivers/net/wireless
$ sudo depmod
```

The driver module should now be loaded automatically.

### DKMS

Automatically rebuilds and installs on kernel updates. DKMS is in official sources of Ubuntu, for installation do:

```sh
$ sudo apt-get install build-essential dkms 
```

The driver source mus be copied to /usr/src/8812au-4.2.2

Then add it to DKMS:

```sh
$ sudo dkms add -m 8812au -v 4.2.2
$ sudo dkms build -m 8812au -v 4.2.2
$ sudo dkms install -m 8812au -v 4.2.2
```

Check with:
```sh
$ sudo dkms status
```
Eventually remove from DKMS with:
```sh
$ sudo dkms remove -m 8812au -v 4.2.2 --all
```
