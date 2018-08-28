When you complete this milestone you will have finished setting up your network. It be ready to run services.

## Introduction 

When you built your firewall you have mine to use as a reference. But what if you're working without a reference? That's where the nmap program comes in. The Nmap program performs a port can on a computer to determine what is permitted through the firewall and what is blocked. Nmap is an important tools for an admin because it's the same tool that hackers use. You should know what the hackers know.

## Firewall Rule Recap 

If you completed the steps in[IPTables Howto](ipchains_howto.html)you should have a firewall with the following rules:

### IPv4 INPUT Chain (Policy: DROP) 

#RuleAction1Input from device loACCEPT2Input from device ens192ACCEPT3ICMP protocolACCEPT4NEW packets to TCP port 22ACCEPT5RELATED or ESTABLISHED packetsACCEPT6ALL packetsLOG

### IPv4 FORWARD Chain (Policy: DROP) 

#RuleAction1Input from device ens192, output to ens160ACCEPT2RELATED or ESTABLISHED packetsACCEPT3ALL packetsLOG

### IPv4 NAT Table POSTROUTING Chain (Policy: ACCEPT) 

#RuleAction1Input from device ens192, output to ens160MASQUERADE

### IPv6 INPUT Chain (Policy: DROP) 

#RuleAction1Input from device loACCEPT2Input from device ens192ACCEPT3ICMPv6 protocolACCEPT4Packets to UDP port 546ACCEPT5NEW packets to TCP port 22ACCEPT6RELATED or ESTABLISHED packetsACCEPT7ALL packetsLOG

### IPv4 FORWARD Chain (Policy: DROP) 

#RuleAction1Input from device ens192, output to ens160ACCEPT2ICMPv6 ProtocolACCEPT3NEW packets to TCP port 22ACCEPT4RELATED or ESTABLISHED packetsACCEPT3ALL packetsLOG
Please verify that your rules match these exactly!!!!!

## Using Nmap 

The nmap program is already installed on Tux. Nmap can do more powerful things when run as root, but for our purposes running it as a normal user is just fine. To install nmap on your VMs run the command:

```
$ sudo apt-get install nmap
```

Nmap's default scan is a connect scan and it can be performed by a regular user. To scan a host (for example my router) run the command:

```
 nmap <host-name-or-ip>
```

For example, scanning my router shows:

```

```
tux$ nmap -Pn 172.19.192.30

Starting Nmap 7.01 ( https://nmap.org ) at 2017-03-09 16:12 PST
Nmap scan report for 172.19.192.30
Host is up (0.00037s latency).
Not shown: 999 filtered ports
PORT  STATE SERVICE
22/tcp open ssh

Nmap done: 1 IP address (1 host up) scanned in 6.59 seconds
```

This is what I expect, port 22 is open and the rest are filtered. If the other ports are "closed" that means the firewall is not applied. By default only a few select ports are probed. To specify what ports you're interested in run nmap with the -p argument:

```

```
tux$ nmap -Pn -p 1-2048 172.19.192.30

Starting Nmap 7.01 ( https://nmap.org ) at 2017-03-09 16:13 PST
Nmap scan report for 172.19.192.30
Host is up (0.00045s latency).
Not shown: 2047 filtered ports
PORT  STATE SERVICE
22/tcp open ssh

Nmap done: 1 IP address (1 host up) scanned in 6.95 seconds
```

Sometimes you have to use the -Pn option because nmap will won't start a scan unless it can ping a target.

## Use Nmap 

Use nmap to probe the following:
  - The public IPv4 address of your router (the 172.19.192.x address)
  - The IPv6 address of your router
  - The IPv6 addresses of your switch and server VMs.

Save the output of the scans into text files and submit them.

## Collect your Firewall Rules 

It is absolutely critical that your firewalls match my reference. Subtle differences can mess up your network badly. Do not use any firewall rules you read on StackOverflow, instead figure out the commands and copy my firewall specified in[IPTables Howto](ipchains_howto.html). When you're happy with your rules be sure to save them and submit the following two files:
  - /etc/default/iptables
  - /etc/default/ip6tables

Be sure to reboot your router to make sure the rules re-apply.

## Turn In 

  - router-ipv4-scan.txt
  - router-ipv6-scan.txt
  - switch-ipv6-scan.txt
  - db-server-ipv6-scan.txt
  - web-server-ipv6-scan.txt
  - iptables
  - ip6tables

Submit your files on canvas.