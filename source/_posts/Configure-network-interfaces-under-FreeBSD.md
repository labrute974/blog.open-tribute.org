---
title: Configure network interfaces under FreeBSD.
author: Karel Malbroukou
tags:
  - FreeBSD
  - Open Source
date: 2013-01-28 22:22:25
---

Hello ladies and gentlemen,

As said, about one year ago, i now open the BSD category !!! What's up!? ^^
So, today, i'll teach how to configure network interfaces under FreeBSD...

Yeah .. i know ...some of you will think that i think you're a bunch of noobs ... but no! It's just that i don't think that all of you are BSD nerds ...<a name='more'></a>

Linux and FreeBSD are Unix systems (actually Linux is not really a Unix system ... but a derivate) so we can guess that the network configuration is the same, but not really.

We are still using the same command: **ifconfig**, but with a slight difference. Here's the basic syntax of the command under FreeBSD:

`ifconfig <netif> <net_class> <address> [netmask <netmask>]`

Here we can see difference parameters that we need to give to be able to configure our interface, but first we have to get the name of the interface. The net class would be for us _inet_ (for IPV4).

Under Linux, it would have been ethX, raX or wlanX but under BSD, naming are quite different. For Linux, the naming is based on the link characteristics (ethernet ...), but BSD will create the interface name according to the driver.

So you will have to get first the name of the interface on which you want to apply your configuration using:
``` bash
# ifconfig -a
em0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
      inet 192.168.1.3 netmask 0xffffff00 broadcast 192.168.1.255
      ether 00:a0:cc:da:da:da
      media: Ethernet autoselect (100baseTX <full-duplex>)
      status: active
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
      inet 127.0.0.1 netmask 0xff000000
```

Here we can see that we have the loopback interface (_lo0_) and a NIC (_em0_).

Note that on BSD, you can change the name of the interface using the following command:
``` bash
ifconfig <netif> name <new_netif_name>
```

For example:
``` bash
# ifconfig em0 name eth0
eth0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
      inet 192.168.1.3 netmask 0xffffff00 broadcast 192.168.1.255
      ether 00:a0:cc:da:da:da
      media: Ethernet autoselect (100baseTX <full-duplex>)
      status: active
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
      inet 127.0.0.1 netmask 0xff000000
```

So, now to put an IP, we will have for example:
``` bash
# ifconfig eth0 inet 192.168.0.24 netmask 255.255.255.0
```

That's about it.

Now, we will just see how to configure network interface automatically at boot.
You just have to edit the file _/etc/rc.conf_ and add the corresponding lines:
```
ifconfig_em0_name="eth0"
ifconfig_eth0="inet 192.168.0.24 netmask 255.255.255.0"
```

Reboot, and your interface will then have the name _eth0_ with the corresponding IP.

Isn't it easier than on Ubuntu (for Ubuntu users :P)? He he
Any comments appreciated ^^
