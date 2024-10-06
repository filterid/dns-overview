# DNS Related RFCs - Overview

This page provides an overview of DNS-related Request for Comments (RFCs), categorized by topic with brief descriptions.

## Table of Contents
- [Core DNS Standards](#core-dns-standards)
- [DNS Security (DNSSEC)](#dns-security-dnssec)
- [DNS Extensions and Updates](#dns-extensions-and-updates)
- [DNS over Encrypted Transports](#dns-over-encrypted-transports)
- [DNS Operations](#dns-operations)
- [Resource Record Types](#resource-record-types)
- [Internationalized Domain Names](#internationalized-domain-names)
- [Experimental and Historic RFCs](#experimental-and-historic-rfcs)

## Core DNS Standards

### [RFC 1034](https://www.rfc-editor.org/rfc/rfc1034) - Domain Names - Concepts and Facilities
- Published: November 1987
- Core concepts of the Domain Name System
- Defines the DNS architecture and theory of operation
- **Status**: Internet Standard

### [RFC 1035](https://www.rfc-editor.org/rfc/rfc1035) - Domain Names - Implementation and Specification
- Published: November 1987
- Details of DNS implementation
- Defines message formats and protocol operations
- **Status**: Internet Standard

### [RFC 2181](https://www.rfc-editor.org/rfc/rfc2181) - Clarifications to the DNS Specification
- Published: July 1997
- Important clarifications to RFCs 1034 and 1035
- Addresses various implementation issues
- **Status**: Proposed Standard

### [RFC 2308](https://www.rfc-editor.org/rfc/rfc2308) - Negative Caching of DNS Queries
- Published: March 1998
- Defines handling of negative responses
- Specifies TTL for negative caching
- **Status**: Proposed Standard

## DNS Security (DNSSEC)

### [RFC 4033](https://www.rfc-editor.org/rfc/rfc4033) - DNS Security Introduction and Requirements
- Published: March 2005
- Overview of DNSSEC
- Security requirements and threats
- **Status**: Proposed Standard

### [RFC 4034](https://www.rfc-editor.org/rfc/rfc4034) - Resource Records for DNS Security Extensions
- Published: March 2005
- Defines new resource record types for DNSSEC
- Covers DNSKEY, RRSIG, NSEC, DS records
- **Status**: Proposed Standard

### [RFC 4035](https://www.rfc-editor.org/rfc/rfc4035) - Protocol Modifications for DNS Security Extensions
- Published: March 2005
- Protocol modifications for DNSSEC support
- Defines message processing and serving
- **Status**: Proposed Standard

### [RFC 5155](https://www.rfc-editor.org/rfc/rfc5155) - DNS Security (DNSSEC) Hashed Authenticated Denial of Existence
- Published: March 2008
- Defines NSEC3 and NSEC3PARAM records
- Alternative to NSEC for authenticated denial
- **Status**: Proposed Standard

### [RFC 6781](https://www.rfc-editor.org/rfc/rfc6781) - DNSSEC Operational Practices
- Published: December 2012
- Guidelines for DNSSEC deployment
- Key management and rollover procedures
- **Status**: Informational

## DNS Extensions and Updates

### [RFC 2136](https://www.rfc-editor.org/rfc/rfc2136) - Dynamic Updates in the DNS
- Published: April 1997
- Defines DNS UPDATE operations
- Allows dynamic zone modifications
- **Status**: Proposed Standard

### [RFC 2671](https://www.rfc-editor.org/rfc/rfc2671) - Extension Mechanisms for DNS (EDNS0)
- Published: August 1999
- Defines mechanism for DNS extensions
- Allows larger DNS messages
- **Status**: Proposed Standard

### [RFC 6891](https://www.rfc-editor.org/rfc/rfc6891) - Extension Mechanisms for DNS (EDNS(0))
- Published: April 2013
- Updates and replaces RFC 2671
- Current EDNS specification
- **Status**: Internet Standard

## DNS over Encrypted Transports

### [RFC 7858](https://www.rfc-editor.org/rfc/rfc7858) - Specification for DNS over Transport Layer Security (TLS)
- Published: May 2016
- DNS over TLS (DoT) specification
- Port 853 for encrypted DNS
- **Status**: Proposed Standard

### [RFC 8484](https://www.rfc-editor.org/rfc/rfc8484) - DNS Queries over HTTPS (DoH)
- Published: October 2018
- DNS over HTTPS specification
- REST API for DNS queries
- **Status**: Proposed Standard

### [RFC 9250](https://www.rfc-editor.org/rfc/rfc9250) - DNS over Dedicated QUIC Connections
- Published: May 2022
- DNS over QUIC specification
- Modern encrypted transport
- **Status**: Proposed Standard

## DNS Operations

### [RFC 1912](https://www.rfc-editor.org/rfc/rfc1912) - Common DNS Operational and Configuration Errors
- Published: February 1996
- Common mistakes in DNS operations
- Best practices for configuration
- **Status**: Informational

### [RFC 2182](https://www.rfc-editor.org/rfc/rfc2182) - Selection and Operation of Secondary DNS Servers
- Published: July 1997
- Guidelines for secondary DNS servers
- Redundancy and placement considerations
- **Status**: Best Current Practice

### [RFC 7719](https://www.rfc-editor.org/rfc/rfc7719) - DNS Terminology
- Published: December 2015
- Comprehensive DNS terminology
- Clarifies commonly used terms
- **Status**: Informational

## Resource Record Types

### [RFC 3596](https://www.rfc-editor.org/rfc/rfc3596) - DNS Extensions to Support IP Version 6
- Published: October 2003
- Defines AAAA record type
- IPv6 support in DNS
- **Status**: Draft Standard

### [RFC 4408](https://www.rfc-editor.org/rfc/rfc4408) - Sender Policy Framework (SPF)
- Published: April 2006
- Defines SPF record format
- Email authentication framework
- **Status**: Experimental

### [RFC 6844](https://www.rfc-editor.org/rfc/rfc6844) - DNS Certification Authority Authorization (CAA) Resource Record
- Published: January 2013
- Defines CAA record type
- Controls certificate issuance
- **Status**: Proposed Standard

### [RFC 7208](https://www.rfc-editor.org/rfc/rfc7208) - Sender Policy Framework (SPF)
- Published: April 2014
- Updates RFC 4408
- Current SPF specification
- **Status**: Proposed Standard

## Internationalized Domain Names

### [RFC 5890](https://www.rfc-editor.org/rfc/rfc5890) - Internationalized Domain Names for Applications (IDNA)
- Published: August 2010
- IDNA2008 protocol
- Framework for non-ASCII domains
- **Status**: Proposed Standard

### [RFC 5891](https://www.rfc-editor.org/rfc/rfc5891) - IDNA Protocol
- Published: August 2010
- Technical specification for IDNA
- Processing rules for IDNs
- **Status**: Proposed Standard

## Experimental and Historic RFCs

### [RFC 974](https://www.rfc-editor.org/rfc/rfc974) - Mail Routing and the Domain System
- Published: January 1986
- Original MX record specification
- Basic mail routing concepts
- **Status**: Historic

### [RFC 1101](https://www.rfc-editor.org/rfc/rfc1101) - DNS Encoding of Network Names and Other Types
- Published: April 1989
- Early DNS encoding schemes
- Network name encoding
- **Status**: Historic

## Updates and Living Documents

This document tracks DNS-related RFCs up to 2024. For the most current RFC status and updates:

1. Visit the [IETF Datatracker](https://datatracker.ietf.org/)
2. Check the [IANA DNS Parameters](https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml)
3. Follow the [IETF DNSOP Working Group](https://datatracker.ietf.org/wg/dnsop/documents/)

## Contributing

To suggest updates or corrections to this document:

1. Check RFC status at [RFC Editor](https://www.rfc-editor.org/)
2. Verify current standards track status
3. Submit a pull request with changes
