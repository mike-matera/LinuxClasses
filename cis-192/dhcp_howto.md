This lesson will show you how to setup your DHCP server. The presentation slides are [here](https://docs.google.com/a/lifealgorithmic.com/presentation/d/1WNeupEJD5vkjxMnFA14FrHZoVkTPGvLMEVmDEFzQMUc/edit?usp=sharing).

### Commands 

  * apt-get
  * service
  * netstat

### Configuration 

  * /etc/dhcp/dhcpd.conf
  * /etc/default/isc-dhcp-server

Contents
  - [1 Commands](#TOC_Commands)
  - [1.1 Configuration](#TOC_Configuration)

  - [2 Introduction](#TOC_Introduction)
  - [3 Install the DHCP Server](#TOC_Install_the_DHCP_Server)
  - [4 Setup Your Network](#TOC_Setup_Your_Network_)
  - [5 Create a Reservation](#TOC_Create_a_Reservation)

## Introduction 

Ubuntu comes with the DHCP client installed by default. The DHCP client is the software responsible for obtaining leases from a DHCP server and every host should have one. The DHCP server is not commonly installed and is responsible for generating and handing out leases. This is an important job on the network but few hosts need to do it.

## Install the DHCP Server 

Use apt:

```
router$ sudo apt-get install isc-dhcp-server
```

This installs DHCP server for both IPv4 and IPv6. They have very different roles on the network and we will not need DHCPv6 in our networks. By default your DHCP server will have an empty configuration so it won't give out IP addresses until you have changed things.

## Setup Your Network 

Now it's time to configure DHCP for your network. The configuration is located in /etc/dhcp/dhcpd.conf. There are some key options that you should set in the file: 

```
# Set your internal domain name
option domain-name "yourname.cis.cabrillo.edu";

# Name servers can be referenced by name, they will be turned into IP addresses automatically by the server
option domain-name-servers 10.192.0.1; 

# Short lease times are good for debugging
default-lease-time 600; 
max-lease-time 1200;
```

Notice that you have setup your router's IP address at the DNS server. Since your hosts don't yet have IPv4 connectivity they can't use Cabrillo's IPv4 nameservers anyway. Soon your router will run DNS and this will work. These global options are sufficient for your network. A complicated network will have multiple scopes. Yours only has one so there's no real difference between global and local options. Add the following zone to your configuration file:

```
subnet 10.192.0.0 netmask 255.255.0.0 { 
  
```

option routers 10.192.0.1; 
  option broadcast-address 10.192.255.255; 
  range 10.192.5.1 10.192.5.100;
}
```

The DHCP server is started with an option that restricts which Ethernet devices that it will hand out addresses on. That option is located in /etc/default/isc-dhcp-server. Make sure the INTERFACES variable is set like this:

```
# This is /etc/default/isc-dhcp-server
INTERFACES="ens192"
```

WARNING: DO NOT PUT ens160 IN THE LIST
Now you are ready to test your DHCP server. You can restart the daemon by running:

```
$ sudo systemctl restart isc-dhcp-server
```

If you have everything setup correctly you will see the following lines in /var/log/syslog:

```
dhcpd: Internet Systems Consortium DHCP Server 4.2.4
dhcpd: Copyright 2004-2012 Internet Systems Consortium.
dhcpd: All rights reserved.
dhcpd: For info, please visit https://www.isc.org/software/dhcp/
dhcpd: Internet Systems Consortium DHCP Server 4.2.4
dhcpd: Copyright 2004-2012 Internet Systems Consortium.
dhcpd: All rights reserved.
dhcpd: For info, please visit https://www.isc.org/software/dhcp/
dhcpd: Wrote 0 leases to leases file.
dhcpd:
dhcpd: No subnet declaration for eth0 (172.20.192.1).
dhcpd: ** Ignoring requests on eth0. If this is not what
dhcpd:  you want, please write a subnet declaration
dhcpd:  in your dhcpd.conf file for the network segment
dhcpd:  to which interface eth0 is attached. **
dhcpd:
```

Ignoring eth0 is exactly what you want! You can also see that DHCP is listening by running the command:

```
$ sudo netstat -lnup
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address      Foreign Address     State    PID/Program name
udp    0   0 0.0.0.0:1587      0.0.0.0:*              8049/dhcpd   
udp    0   0 0.0.0.0:67       0.0.0.0:*              8049/dhcpd   
udp6    0   0 :::60895        :::*                8049/dhcpd   
```

The output of the netstat command shows that the DHCP server is listening on UDP port 67. That's good!

## Create a Reservation 

Last week you observed your servers futilely attempting to receive an IPv4 address over DHCP. If you have your server setup correctly they should now get an address. Verify that they are doing so by using tcpdump to observer the DHCP conversation. On your router run the following command:

```
$ sudo tcpdump -i ens192 udp
```

If you used the default lease time of 600 seconds each server will renew every five minutes. If you don't want to wait that long you could reboot your server. The switch is statically configured so don't wait for it. The IP addresses your servers get are from the pool but not entirely predicable. That's bad news for servers because you need their addresses to stay the same.Fix this by reserving a DHCP address for each server by MAC address. You should have collected MAC addresses in the previous week's lab. Add a declaration like the following to your DHCP configuration file:

```
host web-server {
 hardware ethernet00:50:56:bd:0a:6c;
 fixed-address 10.192.0.4;
}
host db-server {
 hardware ethernet00:50:56:bd:0a:7c;
 fixed-address 10.192.0.3;
}
```

Be sure to change the MAC address to match your host! Also, be sure that the IP address you have listed is NOT in the pool.