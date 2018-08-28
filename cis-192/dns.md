The lecture slides are [here](https://docs.google.com/presentation/d/1M7k9Pm1UFjS5nXwK7oLJrKVd5eKA5vHgvi8qceqbC0Y/edit?usp=sharing).

### Commands 

  * dig

### Configuration 

  * /etc/bind/*

### Further Reading 

  * [Ubuntu Server Guide](https://help.ubuntu.com/12.04/serverguide/index.html) has an excellent section about DNS

  * [Linux DNS HowTo](http://www.tldp.org/HOWTO/DNS_HOWTO.html)

## Introduction 

This guide will show you how to setup DNS with BIND version 9, the reference implementation of the Domain Name System (DNS).

## Testing DNS with dig 

There are easy ways to lookup IP addresses and there are helpful ways to lookup IP addresses. If you're an administrator and you want to know what's really going on there's only one tool for you: dig. Dig can perform any DNS query, iterative or recursive and inform you of the complete result. Dig can also ask any server, not just the one your system is configured to use. This gives you the information you need to fix problems with your DNS configuration.

For a full guide to dig, use it's [manual page](http://manpages.ubuntu.com/manpages/trusty/man1/dig.1.html). This page has some common recipes.

Tell dig to use a particular nameserver (that's not necessarily the system's nameserver). This is very useful for debugging your bind9 configuration before you tell your hosts to use your internal nameserver:

```
 dig @<namserver-to-use> <query>
```

Here's an example where I want to test my CIS 192 server from the server itself:

```

$ dig @localhost router.matera.cis.cabrillo.edu
```

The query argument can has two parts. What type of query you want to make and what you are looking for. Common query types are:

  * A and AAAA: Address records for IPv4 and IPv6 respectively

  * NS: Find a nameserver for a domain

  * MX: Find a mailserver for a domain

Some examples:

```
 dig A www.google.com   # find IPv4 address
 dig AAAA www.google.com # find and IPv6 address 
 dig MX www.google.com  # find Google's mail server
 dig NS www.google.com  # find Google's nameserver
 dig www.google.com    # Default is "A"
```

There are other useful things that dig can do easily. If you want to lookup the name for an IP address you can use the -x option:

```

```
dig -x 216.58.216.132
dig -x 2607:f8b0:400a:806::2004
```

If you want dig to do an iterative lookup, starting with the root servers use the +trace option:

```

dig +trace www.google.com
```

## Configuring BIND 

Bind is the most widely used DNS software on the Internet. It's the standard on Ubuntu and other Linuxes and is written by the Internet Systems Consortium. It's light, fast, secure and can be a bit difficult to configure. With a good understanding of the basics you can build simple to complex domains easily. When BIND starts it reads it's top-level configuration file. That file on Ubuntu is just a pointer to other files:

```

%% /etc/bind/named.conf
// This is the primary configuration file for the BIND DNS server named.
//
// Please read /usr/share/doc/bind9/README.Debian.gz for information on the
// structure of BIND configuration files in Debian, *BEFORE* you customize
// this configuration file.
//
// If you are just adding zones, please do that in /etc/bind/named.conf.local
include "/etc/bind/named.conf.options";
include "/etc/bind/named.conf.local";
include "/etc/bind/named.conf.default-zones";
```

In order to customize your installation you will edit named.conf.options and named.conf.local. In your named.conf.options uncomment the forwarders directive and add Cabrillo's nameservers to the list. It should look like this:

```

forwarders {
8.8.8.8
;
```

 8.8.4.4
```
;
};

dnssec-enable no;

# comment out this line
# dnssec-validation auto;
```

Adding forwarders tells BIND to use the specified naemservers as backup when it can't find names. If there are no forwarders BIND will do an iterative lookup starting at the root servers. This is often slower than depending on a nameserver close by that handles lots of queries and may have the name cached already. It's also polite to keep load off of the root servers.

Next you will add your zones to the local configuration:

```

// this is
/etc/bind/
named.conf.local
// Forward zone so that NAME -> NUMBER lookups work
// both IPv4 and IPv6 records can go here.

zone "matera.cis.cabrillo.edu" {
 type master;
 file "/etc/bind/db.matera";
};
// Reverse zone so that NUMBER -> NAME lookups work (IPv4)
zone "192.20.172.in-addr.arpa" {
 type master;
 notify no;
 file "/etc/bind/db.matera-ipv4";
};
// Reverse zone so that NUMBER -> NAME lookups work (IPv6)
// Note: Use the IPv6 address for your INTERNAL network
zone "1.0.9.f.f.0.8.0.0.8.3.f.7.0.6.2.ip6.arpa" {
 type master;
 notify no;
 file "/etc/bind/db.matera-ipv6";
};
```

In addr WHAT???
Reverse zones have special names. As you know, DNS is like a file path but read left to right and separated by dots. IP addresses are looked at almost the same way. It's a bit of a hack, but in order to figure out the zone name for an IPv4 address you reverse the octets and add "in-addr.arpa". For example:

```
Subnet :

10.11.12.0/24
Zone  :

13.12.11.in-addr.arpa
```

DNS only understands /24 subnets, so larger IPv4 subnets have to be broken up into smaller zones. Reverse zones for IPv6 work in a similar way except they are not broken into octets, there's a dot for every hexadecimal number. That's a lot of dots! You must be careful to put all missing zeros back into your DNS record or else the zone file will not work. Example:

```
Short IPv6: 
2607:f380:80f:f830:192/64
Full IPv6: 
2607:f380:080f:f830:0192/64
Zone:    2.9.1.0.0.3.8.f.f.0.8.0.0.8.3.f.7.0.6.2.ip6.arpa
```

Notice that IPv6 zones end in ip6.arpa. When you reverse your IP addresses, be sure to do it very carefully! With your zones declared you're ready to define what's in them. Here are the zones that I use in my network. You can use them but be careful to replace my names with your names and my IP addresses with your IP addresses.

```

; This is db.matera
$TTL 600
matera.cis.cabrillo.edu. IN SOA ns1.matera.cis.cabrillo.edu. root.matera.cis.cabrillo.edu. (
   11 ; Serial
 604800 ; Refresh
 86400 ; Retry
2419200 ; Expire
 604800 ) ; Negative Cache TTL
;
matera.cis.cabrillo.edu. 
IN NS  ns1.matera.cis.cabrillo.edu.
ns1
 
IN A   172.20.192.1
ns1
IN AAAA 2607:f380:80f:f901::1
router 
 
 
 
 
 
  
IN CNAME ns1.matera.cis.cabrillo.edu.
```

Be sure to set the $TTL to 600 (5 minutes). The default is 1 week if you leave it set that way you will poison DNS cashes for a looong time.

In order for IPv4 reverse lookups to work you need PTR records:

```

; This isdb.matera-ipv4
$TTL 604800
192.20.172.in-addr.arpa. IN SOA ns1.matera.cis.cabrillo.edu. root.matera.cis.cabrillo.edu. (
   11 ; Serial
 604800 ; Refresh
 86400 ; Retry
2419200 ; Expire
 604800 ) ; Negative Cache TTL
;
192.20.172.in-addr.arpa. 
IN NS
ns1.matera.cis.cabrillo.edu.
1
IN PTR
 router.matera.cis.cabrillo.edu.
```

IPv6 reverse lookups also use PTR records. Watch out for those long reversed IP addresses:

```

; This is db.matera-ipv6
$TTL 604800
1.0.9.f.f.0.8.0.0.8.3.f.7.0.6.2.ip6.arpa. IN SOA ns1.matera.cis.cabrillo.edu. root.matera.cis.cabrillo.edu. (
   11 ; Serial
 604800 ; Refresh
 86400 ; Retry
2419200 ; Expire
 604800 ) ; Negative Cache TTL
;
1.0.9.f.f.0.8.0.0.8.3.f.7.0.6.2.ip6.arpa. IN NS ns1.matera.cis.cabrillo.edu.
1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.1.0.9.f.f.0.8.0.0.8.3.f.7.0.6.2.ip6.arpa. IN PTR router.matera.cis.cabrillo.edu.
```

## Gotchas 

There are a number of things that often trip-up beginners:

  * Don't forget semicolons. They are required almost everywhere. If BIND doesn't start look in /var/log/syslog.

  * Use absolute paths! Bind cares about the trailing dot, if you leave it off BIND assumes you mean a relative path. 

Here's an example of getting messing up an absolute path:

```

## Assume this is a record for matera.com.
server1
      IN A 1.2.3.4 # This means "server1.matera.com."
server2.matera.com. IN A 1.2.3.5 # This means "server2.matera.com."
server3.matera.com IN A 1.2.3.6 # Oops! This means "server3.matera.com.matera.com."
```

## Firewall Updates 

In order for DNS to work you must let packets headed for UDP/53 through the firewall. Be sure that both IPv6 and IPv4 firewalls have rules. The examples below are how to add rules to your firewall.

```

```
# Allow UDP datagrams on port 53
iptables -A INPUT -p udp --dport 53 -j ACCEPT
# Do the same for IPv6
ip6tables -A INPUT -p udp --dport 53 -j ACCEPT
```

Note, you must put the iptables entries in the correct order for them to work. These commands may need to be adjusted for your firewall. Also, remember to save your firewall rules with iptables-save and ip6tables-save. For more information on iptables review the notes [here](ipchains_howto.html).