# DNS Record Types - Overview

This page provides an overview of DNS record types, including traditional records, modern standards, and DNSSEC-specific records.

## Table of Contents
- [Basic DNS Records](#basic-dns-records)
- [Mail-Related Records](#mail-related-records)
- [Security Records](#security-records)
- [DNSSEC Records](#dnssec-records)
- [Modern and Special Use Records](#modern-and-special-use-records)
- [Less Common Records](#less-common-records)

## Basic DNS Records

### A Record
- **Purpose**: Maps a hostname to an IPv4 address
- **Format**: `name IN A IPv4_address`
- **Example**: `example.com. IN A 93.184.216.34`
- **Use Case**: Basic website hosting, server IP assignment

### AAAA Record
- **Purpose**: Maps a hostname to an IPv6 address
- **Format**: `name IN AAAA IPv6_address`
- **Example**: `example.com. IN AAAA 2606:2800:220:1:248:1893:25c8:1946`
- **Use Case**: IPv6-enabled services

### CNAME Record
- **Purpose**: Creates an alias for another domain name
- **Format**: `alias IN CNAME canonical_name`
- **Example**: `www.example.com. IN CNAME example.com.`
- **Use Case**: Creating aliases, subdomain redirects
- **Limitations**: Cannot coexist with other records at the same name

### NS Record
- **Purpose**: Delegates a DNS zone to an authoritative server
- **Format**: `domain IN NS nameserver`
- **Example**: `example.com. IN NS ns1.example.com.`
- **Use Case**: DNS zone delegation

### SOA Record
- **Purpose**: Stores administrative information about a DNS zone
- **Format**: `domain IN SOA primary_ns admin_email serial refresh retry expire minimum`
- **Example**: 
```
example.com. IN SOA ns1.example.com. admin.example.com. (
    2024040601 ; Serial
    7200       ; Refresh (2 hours)
    3600       ; Retry (1 hour)
    1209600    ; Expire (2 weeks)
    3600       ; Minimum TTL (1 hour)
)
```
- **Use Case**: Zone transfers and caching control

### PTR Record
- **Purpose**: Maps an IP address to a hostname (reverse DNS)
- **Format**: `reverse_IP IN PTR hostname`
- **Example**: `34.216.184.93.in-addr.arpa. IN PTR example.com.`
- **Use Case**: Reverse DNS lookups

## Mail-Related Records

### MX Record
- **Purpose**: Specifies mail servers for a domain
- **Format**: `domain IN MX preference mailserver`
- **Example**: `example.com. IN MX 10 mail.example.com.`
- **Use Case**: Email routing

### SPF Record (TXT)
- **Purpose**: Defines authorized mail servers
- **Format**: `domain IN TXT "v=spf1 [rules]"`
- **Example**: `example.com. IN TXT "v=spf1 ip4:192.0.2.0/24 include:_spf.example.com ~all"`
- **Use Case**: Email authentication

### DMARC Record (TXT)
- **Purpose**: Email authentication policy
- **Format**: `_dmarc.domain IN TXT "v=DMARC1; [policy]"`
- **Example**: `_dmarc.example.com. IN TXT "v=DMARC1; p=reject; rua=mailto:dmarc@example.com"`
- **Use Case**: Email authentication policy enforcement

## Security Records

### CAA Record
- **Purpose**: Specifies authorized certificate authorities
- **Format**: `domain IN CAA flags tag value`
- **Example**: `example.com. IN CAA 0 issue "letsencrypt.org"`
- **Use Case**: SSL/TLS certificate issuance control

## DNSSEC Records

### DNSKEY Record
- **Purpose**: Stores public keys for DNSSEC
- **Format**: `domain IN DNSKEY flags protocol algorithm key`
- **Use Case**: DNSSEC key distribution

### DS Record
- **Purpose**: References DNSKEY record in parent zone
- **Format**: `domain IN DS key_tag algorithm digest_type digest`
- **Use Case**: DNSSEC delegation

### RRSIG Record
- **Purpose**: Contains cryptographic signatures
- **Format**: `name IN RRSIG type algorithm labels ttl expiration inception key_tag signer signature`
- **Use Case**: DNSSEC record authentication

### NSEC Record
- **Purpose**: Proves non-existence of records
- **Format**: `name IN NSEC next_domain_name [record_types]`
- **Use Case**: DNSSEC authenticated denial of existence

### NSEC3 Record
- **Purpose**: Like NSEC but with hashed names
- **Format**: Complex hashed format
- **Use Case**: DNSSEC authenticated denial with zone walking prevention

## Modern and Special Use Records

### HTTPS Record
- **Purpose**: Defines alternative endpoints for HTTPS connections
- **Format**: `name IN HTTPS priority target params`
- **Example**: `example.com. IN HTTPS 1 . alpn=h2,h3`
- **Use Case**: HTTPS service endpoints, alt-svc

### SVCB Record
- **Purpose**: Service binding and parameter specification
- **Format**: Similar to HTTPS record
- **Use Case**: Generic service endpoint binding

### TLSA Record
- **Purpose**: Associates TLS certificate with service
- **Format**: `_port._proto.name IN TLSA usage selector match_type cert_data`
- **Use Case**: TLS certificate verification

## Less Common Records

### HINFO Record
- **Purpose**: Host information (CPU and OS)
- **Format**: `name IN HINFO cpu_type os_type`
- **Use Case**: System documentation

### RP Record
- **Purpose**: Responsible person
- **Format**: `name IN RP mbox_domain txt_domain`
- **Use Case**: Contact information

### LOC Record
- **Purpose**: Geographic location
- **Format**: `name IN LOC latitude longitude altitude`
- **Use Case**: Physical location documentation

## Best Practices

1. **TTL Settings**
   - Use appropriate TTL values based on record volatility
   - Consider lower TTLs for records that change frequently

2. **Security Considerations**
   - Implement DNSSEC where possible
   - Use CAA records to control SSL/TLS issuance
   - Implement proper SPF, DKIM, and DMARC records

3. **Record Management**
   - Keep SOA serial numbers updated
   - Regular audit of DNS records
   - Document all record changes

## Additional Resources

- [IANA DNS Parameters](https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml)
- [RFC 1035](https://tools.ietf.org/html/rfc1035) - Original DNS implementation specification
- [RFC 4033](https://tools.ietf.org/html/rfc4033) - DNSSEC specification
