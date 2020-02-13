# Route 53 is a DNS service

DNS is on the port 53. So it has a name route 53. (just like actual route 66)

As I already know, DNS is just a phone book. It converts human friendly domain names into an IP address. IP address has two different forms IPv4 and IPv6. 

## IPv4 is running out

It is a 32 bit field, and has over 4 billion different addresses. IPv6 was created to solve this depletion issue and has an address space of 128bits.

## Top level domains

The last word in a domain name represents the top level domain. And the second word in a domain name is known as a *second level domain name*. 

e.g) .com is a top level domain, .co.uk .co would be the second level domain name.

Who controls the top level domain name? IANA (Internet Assigned Numbers Authority) it is. IANA has a root zone database.

## Domain Registrars

How about the whole domain name? It must be **unique**. Domain Registrar is an authority that can assign domain names directly under one or more top-level domains. These domains are registered with InterNIC, a service of ICANN. Each domain name is registered in a central database known as the **WhoIS database.**

## (Super duper important) Records

### Start of Authority Record (SOA)

Every time you buy a domain name, you are given with the Start of Authority Record. This record contains, 

- The name of the server that supplied the data for the zone
- The administrator of the zone
- The current version of the data file
- The default number of seconds for the time-to-live file on resource records.

What is TTL (time to live)?

The length that a DNS record is cached on either the Resolving Server or the users' own local PC. The lower the TTL is, the faster changes to DNS records take to propagate. 

### NS records

Name server records are used by Top Level Domain servers to direct traffic to the Content DNS server which contains the authoritative DNS records.

So let's say the our user types in "hellocloudgurus2019.com". 

Their browser doesn't know the IP address for that domain.

So it goes to the top level domain server(.com), and queries for the **authoritative DNS records**. In order to know the IP address of it. 

And then, with the authoritative DNS records, go and query the NS records, and the NS records are going to give us the start of authority. 

### A records (address records)

The most fundamental type of DNS record. It is used to translate the name of the domain to an IP address. http://www.google.com might point to http://22.12.80.10 or something.

### CName

Canonical Name. It can be used to resolve one domain name to another. It makes "http://m.acloud.guru" and "http://mobile.acloud.guru" point to the same address. Technically, cname can be expressed as the latter one, when we look at our phone book, says "see http://m.acloud.guru". The entity of the phone book doesn't have the IP address itself. It has another Domain Number in it.

### Alias Records

It works like a CNAME record in that you can map one DNS name to another target DNS name. (make "www.example.com" point "elb1234.elb.amazonaws.com") This term is usually for AWS. It maps resource record sets in your hosted zone to ELBs, CloudFront Distributions, or S3 buckets.

**But there are crucial difference**

CNAME can't be used for naked domain names (Entire domain without www. or mobile. ) (a.k.a. zone apex record). 

You can't have a CNAME for "http://acloud.guru" it must be an A record or an Alias record.

# Route 53 Summary

- ELBs do not have pre-defined IPv4 address; you address to them using a DNS name! 
- Alias Record VS CNAME
- PTR record is a reversed version of A record. Input: IP-address, output: domain name


