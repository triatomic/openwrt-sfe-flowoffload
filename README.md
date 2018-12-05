# Fast Path Builds for LEDE/OpenWRT

After flashing only basic services are enabled, to use additional features (including Flow Offload & SFE) you have to enable them 
--------------------------------------------------------------------------------------------------------------------------------

Summary
-------

This repository contains Openwrt firmware images compiled with shortcut-fe module, additional features, performance as well as security optimizations.

The firmware images are based on the Trunk Branch of Openwrt.

During to kernel configuration modifications, upstream kmods cannot be used but upstream packages can be installed

If you need any kmods can you open an issue.

If your router is among the supported (must be ar71xx, ath79 or mpc85xx based) and you want to use this firmware open an issue.

This firmware is optimize for networking performance aspect, I don't recommend running NAS on embedded devices.

For better performance NAS workloads should be running on dedicated devices.

Which to use ?
--------------

The images are subdivided into their respective SOC architectures.

There are differences between mips24k, mips74k, powerpc(mpc85xx) and different optimizations are required to maximise the performance respectively.

Linux Kernel size increase, as well as larger rootfs/security changes in Openwrt, 4MB Router devices are increasingly hard to support.

There are 2 factors affecting this:

-if the flash is too small, the firmware cannot be compiled to fit

-if the RAM is too small, after loading there is insufficient memory to load the firmware in memory and operate properly

Therefore if your router has 32MB RAM OR 4MB Flash please go for the mini builds, otherwise you can go for the normal builds.


Features
--------

Base on Linux Kernel 4.14 using GCC 7.3 toolchain (Both Builds)

Full IPv6 support (Both Builds)

SQM QoS (Both Builds)

Memory Operations optimization for mips24k,mips74k and mpc85xx Architectures (Both Builds)

Shortcut-fe Fast Path Module for accelerated NAT/Routing performance (Both Builds)

FlowOffload Fast Path Module for accelerated NAT/Routing performance (Both Builds)

Default to BBR Congestion Control Algorithm (Both Builds)

```
root@openwrt:~# cat /proc/sys/net/ipv4/tcp_congestion_control
bbr
```

Stack-Smashing Protection (Normal Builds STRONG, Mini builds REGULAR)

Compiled with -O2 Compiler optimization (Normal Builds Only)

OpenSSL Crypto assembly optimizations for mips24k,mips74k and mpc85xx Architectures (Normal Builds Only)

Nlbwmon - Network Bandwidth Monitoring (Normal Builds Only)

4G LTE USB Support (Normal Builds Only)

HTTPS-DNS-Proxy encrypted DNS to query upstream DNS Servers (Normal Builds Only)

https://hacks.mozilla.org/2018/05/a-cartoon-intro-to-dns-over-https/

OpenVPN encrypted tunneling (Normal Builds Only)

Wireguard encrypted stateless tunneling (Normal Builds Only)

ShadowSocks encrypted proxy tunneling (Normal Builds Only)

Use
---

Default Network Address after login: 192.168.1.1

Subnet Mask: 255.255.255.0

Login: root

DYNACK is enabled on hostapd so for NETWORK -> WIRELESS -> ADVANCE SETTINGS -> DISTANCE OPTIMIZATION -> ENTER "auto" for best performance.

![alt text](https://raw.githubusercontent.com/gwlim/openwrt-sfe-flowoffload/master/dynack.PNG)

To enable FlowOffload go to NETWORK -> FIREWALL

To enable SFE go to NETWORK -> FIREWALL

![alt text](https://raw.githubusercontent.com/gwlim/openwrt-sfe-flowoffload/master/sfe-offload.PNG)

It is recommended to reboot after enabling it.

Running Benchmark
-----------------

To benchmark network performance

https://openwrt.org/docs/guide-user/perf_and_log/benchmark.nat

The benchmark below is for WR1043NDv1 AR9132 SoC@400MHZ, anything better should easily hit wired speeds

![alt text](https://raw.githubusercontent.com/gwlim/openwrt-sfe-flowoffload/master/bench.PNG)

To benchmark crypto

https://oldwiki.archive.openwrt.org/doc/howto/benchmark.openssl

Compared on the same hardware, this firmware should achieve faster crypto performance

If you need VPN performance, wireguard should perform significantly better than OpenSSL

Recovery in case of bad flash
-----------------------------

Most Routers have a tftp recovery mode to recover from bad flash.

The generic steps are highlighted below

https://openwrt.org/docs/guide-user/troubleshooting/tftpserver

However you will need to find the specific steps for your router model.

When running tftp server remember to set static ip on your computer and turn off your computer firewall temporarily.
