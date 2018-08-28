This lesson will take you through setting up Postfix on your router. The lecture slides are [here](https://docs.google.com/presentation/d/1pN1RbnkOkWiaBJ5_Vu1goIbMKqwUlW0QVNz6vaIwGxI/edit?usp=sharing).

### Commands 

  * apt-get
  * dpkg-reconfigure

### Configuration 

  * /etc/named/db.<yourdomain>

### Further Reading 

There are multiple mail agents that you can use in Ubuntu. Each with it's pros and cons. The default MTA is Postfix. Here's Ubuntu's official Postfix documentation:
[ https://help.ubuntu.com/community/Postfix](https://help.ubuntu.com/community/Postfix)

Contents
  - [1 Commands](#TOC_Commands)
  - [1.1 Configuration](#TOC_Configuration)
  - [1.2 Further Reading](#TOC_Further_Reading)

  - [2 Introduction](#TOC_Introduction)
  - [3 Install Postfix](#TOC_Install_Postfix)
  - [4 Setup DNS Records](#TOC_Setup_DNS_Records)
  - [5 Checking Your DNS Records](#TOC_Checking_Your_DNS_Records)
  - [6 Checking Your Mail Logs](#TOC_Checking_Your_Mail_Logs)
  - [7 Allowing Inbound Mail Connections](#TOC_Allowing_Inbound_Mail_Connections)

## Introduction 

Email is the most essential protocol on the Internet. Sending and receiving email on your own domain is an important business and marketing tool. You don't have to install your own MTA for that, but you should know how.

## Install Postfix 

Setting up postfix itself is quite easy. You must first install it:

```
infra-server$ sudo apt-get install postfix
```

After that you can have Ubuntu set it up with working defaults for you:

```

```
infra-server$
dpkg-reconfigure postfix
```

## Setup DNS Records 

The DNS system is used to figure out who handles mail for a given domain. You may have noticed that when you send email to your friends with a Gmail account you send the mail to "friend@gmail.com". The name "gmail.com" does not name a machine but a domain. The MTA must find out using a special DNS query:

```
[mmatera@oslab ~]$ dig mx gmail.com
; <<>> DiG 9.8.2rc1-RedHat-9.8.2-0.23.rc1.el6_5.1 <<>> mx gmail.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 41622
;; flags: qr rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 4, ADDITIONAL: 4
;; QUESTION SECTION:
;gmail.com.
IN
MX
;; ANSWER SECTION:
gmail.com.
3600
IN
MX
30 alt3.gmail-smtp-in.l.google.com.
gmail.com.
3600
IN
MX
40 alt4.gmail-smtp-in.l.google.com.
gmail.com.
3600
IN
MX
5 gmail-smtp-in.l.google.com.
gmail.com.
3600
IN
MX
20 alt2.gmail-smtp-in.l.google.com.
gmail.com.
3600
IN
MX
10 alt1.gmail-smtp-in.l.google.com.
;; AUTHORITY SECTION:
gmail.com.
172800
IN
NS
ns1.google.com.
gmail.com.
172800
IN
NS
ns4.google.com.
gmail.com.
172800
IN
NS
ns3.google.com.
gmail.com.
172800
IN
NS
ns2.google.com.
;; ADDITIONAL SECTION:
ns2.google.com.
172176
IN
A
216.239.34.10
ns1.google.com.
172176
IN
A
216.239.32.10
ns3.google.com.
172176
IN
A
216.239.36.10
ns4.google.com.
172176
IN
A
216.239.38.10
;; Query time: 57 msec
;; SERVER: 172.30.5.101#53(172.30.5.101)
;; WHEN: Thu Apr 10 16:15:33 2014
;; MSG SIZE rcvd: 286
```

The answer contains a prioritized list of the servers that handle mail for gmail.com. Servers are supposed to pick the lowest number first. In order to handle mail you must modify your DNS records to contain a mail server. Here's what I did to my zone file to add support for a mail server:

```
@    IN   MX   10   mail
mail  IN  AAAA  <infra-server-public-ipv6-address-here>
@ IN   TXT   "v=spf1 ip6:<infra-server-public-ipv6-address-here>/128-all"
```

The first entry is the MX or mail server record. That contains the name of my domain (abbreviated @) and the name of the mail server (mail). Since my mail server is my infra-server the record points to the IPv6 address of my infra-server. Finally I've added an SPF record stating that I can send email from my infra-server IPv6 address and that's it!

## Checking Your DNS Records 

Once you have your DNS record setup you should check to make sure that other hosts on the Internet can find your mail server. Use dig for that:

```
dig MX <yourdomain>.cis.cabrillo.edu
```

You should see an answer with the proper email address. Also, check your SPF record:

```
dig MX <yourdomain>.cis.cabrillo.edu
```

## Checking Your Mail Logs 

The file:

```
 /var/log/mail.log
```

Will contain the record of any failed attempts to deliver email. That's a very useful place to look for problems.

## Allowing Inbound Mail Connections 

The firewall on your router doesn't allow incoming mail. To do that you'll need to open port 25 in the IPv6 FORWARD chain. You'll need to do that for packets destined for the EUI-64 of your infra server. The command to do that is:

```
router$sudo ip6tables -A FORWARD -m state --state NEW -p tcp --dport 25 -d <EUI-64-ADDRESS-HERE>/128 -j ACCEPT
```

You can check to be sure that it's working using telnet from Opus or Tux

```
opus$ telnet <infra-server-ipv6-address> 25
```

Don't forget to save your firewall after you've made this change!!