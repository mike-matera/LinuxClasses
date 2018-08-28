Dig is the most important tool of a DNS administrator. This lab will help you learn to use it.

## Introduction 

Dig is a utility that probes the domain name system (DNS). It's the most essential tool for understanding what's right and what wrong with your DNS server. If you don't use dig you'll suffer with problems in DNS.

## Searching for Domain Records 

Choose a domain. Any domain. Examine the domain of your choosing by using dig to answer the following questions:
  - What are the nameservers for that domain?
  - What server handles mail on that domain?
  - Are there any SPF or other TEXT records for that domain?

## Understanding an Iterative Lookup 

Now you'll use dig to do an iterative lookup with the +trace option. Dig's output will be much larger than a normal lookup because it's showing you the query at every step of the way. Execute the following command:

```
$ dig +trace opus.cis.cabrillo.edu
```

Answer the following:
  - What is the IP address of every DNS server you queried?
  - What type of name record does opus have?

Turn In
  - The answers to the questions.

Submit your homework on canvas.