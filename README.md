# DNS Introduction by FilterID

This repository contains a curated list of valuable educational resources, documentation, and learning materials about the Domain Name System (DNS). It provides an overview of DNS concepts, protocols, and standards.

## Table of Contents
- [DNS Fundamentals](#dns-fundamentals)
- [DNS Protocols and Standards](#dns-protocols-and-standards)
- [DNS Record Types](#dns-record-types)
- [DNSSEC](#dnssec)
- [RFCs and Standards](#rfcs-and-standards)
- [DNS Security](#dns-security)
- [Learning Resources](#learning-resources)
- [Tools and Visualization](#tools-and-visualization)

## DNS Fundamentals
- [DNS Fundamentals](dns-fundamentals.md)
- [DNS Resolvers](dns-resolvers.md)
- [DNS Replication](dns-replication.md)
- [DNS Mail Settings](dns-mail-settings.md)
- [How DNS Works (DNSimple)](https://howdns.works/) - A fun and colorful explanation of how DNS works
- [DNS for Rocket Scientists](https://www.zytrax.com/books/dns/) - Comprehensive guide on DNS concepts
- [Root Servers Map](https://root-servers.org/) - Information about DNS root servers
- [DNS Resolution Process](https://www.cloudflare.com/learning/dns/what-is-dns/) - Detailed explanation of DNS resolution

## DNS Protocols and Standards
- [DNS Protocol Overview (IETF)](https://www.ietf.org/rfc/rfc1034.txt) - Domain Names - Concepts and Facilities
- [DNS Message Format](https://www.ietf.org/rfc/rfc1035.txt) - Domain Names - Implementation and Specification
- [DNS over HTTPS (DoH)](https://tools.ietf.org/html/rfc8484) - RFC 8484
- [DNS over TLS (DoT)](https://tools.ietf.org/html/rfc7858) - RFC 7858
- [EDNS(0)](https://tools.ietf.org/html/rfc6891) - Extension Mechanisms for DNS

## DNS Record Types
- [DNS Records Overview](dns-records.md)
- Common Record Types
  - A and AAAA Records
  - CNAME Records
  - MX Records
  - TXT Records
  - NS Records
  - PTR Records
  - SOA Records
- Modern Record Types
  - CAA Records
  - DMARC Records
  - SPF Records
  - DKIM Records
  - HTTPS Records
  - SVCB Records

## DNSSEC
- [DNSSEC Overview](dns-dnssec.md)
- [DNSSEC – What Is It and Why Is It Important?](https://www.icann.org/resources/pages/dnssec-what-is-it-why-important-2019-03-05-en)
- [DNSSEC: DNS Security Extensions](https://www.cloudflare.com/dns/dnssec/how-dnssec-works/)
- [DNSSEC Validation](https://www.internetsociety.org/deploy360/dnssec/)
- Record Types
  - DNSKEY
  - DS
  - RRSIG
  - NSEC and NSEC3

## RFCs and Standards
Essential RFCs for DNS understanding:
- [RFC Overview](dns-rfc.md)
- [RFC 1034](https://tools.ietf.org/html/rfc1034) - Domain Names - Concepts and Facilities
- [RFC 1035](https://tools.ietf.org/html/rfc1035) - Domain Names - Implementation and Specification
- [RFC 4033-4035](https://tools.ietf.org/html/rfc4033) - DNSSEC
- [RFC 6891](https://tools.ietf.org/html/rfc6891) - EDNS(0)
- [RFC 7858](https://tools.ietf.org/html/rfc7858) - DNS over TLS
- [RFC 8484](https://tools.ietf.org/html/rfc8484) - DNS over HTTPS

## DNS Security
- [DNS Cache Poisoning](https://www.cloudflare.com/learning/dns/dns-cache-poisoning/)
- [DNS Hijacking](https://www.icann.org/resources/pages/dns-hijacking-2019-01-17-en)
- [DNS Amplification Attacks](https://www.cloudflare.com/learning/ddos/dns-amplification-ddos-attack/)
- [Domain Generation Algorithms (DGA)](https://www.cloudflare.com/learning/dns/dns-security-threats/)

## Learning Resources
- Books
  - "DNS and BIND" by Cricket Liu
  - "Pro DNS and BIND 10" by Ron Aitchison
  - "DNSSEC Mastery" by Michael W. Lucas

- Online Courses
  - [DNS Fundamentals (Linux Academy)](https://linuxacademy.com/)
  - [DNS & BIND Security (SANS)](https://www.sans.org/)

## Tools and Visualization
- [DNSViz](https://dnsviz.net/) - DNS visualization and debugging tool
- [DNS Looking Glass](https://www.dns-lg.com/) - DNS propagation checker
- [DNS Propagation Checker](https://dnschecker.org/) - Check DNS propagation worldwide
- [DNS Tree](https://www.dnstree.com/) - Visual DNS tree explorer