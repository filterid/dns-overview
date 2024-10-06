# DNS Resolvers and Servers - A Complete Guide

This page provides an overview DNS resolvers, including operating system configurations, public DNS services, and filtering options.

## Table of Contents
- [DNS Resolver Types](#dns-resolver-types)
- [Operating System DNS Configuration](#operating-system-dns-configuration)
- [ISP DNS Servers](#isp-dns-servers)
- [Public DNS Services](#public-dns-services)
- [DNS Filtering and Security](#dns-filtering-and-security)
- [DNS-based Ad Blocking](#dns-based-ad-blocking)
- [DNS Caching](#dns-caching)
- [Performance Considerations](#performance-considerations)
- [Privacy and Security](#privacy-and-security)

## DNS Resolver Types

### Local Resolvers
- Run on the same machine
- Provide caching
- Forward queries to configured DNS servers
- Examples: systemd-resolved, Unbound

### Forwarding Resolvers
- Run on local network
- Cache responses
- Forward to upstream servers
- Examples: Pi-hole, AdGuard Home

### Recursive Resolvers
- Perform full DNS resolution
- Maintain large caches
- Examples: ISP DNS, Google DNS, Cloudflare DNS

## Operating System DNS Configuration

### Windows

#### Viewing Current Settings
```powershell
# View network adapter settings
ipconfig /all

# View DNS cache
ipconfig /displaydns

# Flush DNS cache
ipconfig /flushdns
```

#### Configuration Methods
1. **GUI Configuration**
   ```
   Control Panel → Network and Internet → Network Connections →
   [Right-click adapter] → Properties → TCP/IPv4 → Properties
   ```

2. **PowerShell Configuration**
   ```powershell
   # Set DNS servers
   Set-DnsClientServerAddress -InterfaceIndex 12 -ServerAddresses "1.1.1.1","1.0.0.1"

   # Get DNS servers
   Get-DnsClientServerAddress
   ```

### Linux

#### systemd-resolved
```bash
# View current settings
resolvectl status

# Configure DNS servers
sudo nano /etc/systemd/resolved.conf

[Resolve]
DNS=1.1.1.1 1.0.0.1
FallbackDNS=8.8.8.8 8.8.4.4
DNSOverTLS=yes
DNSSEC=yes
```

#### Network Manager
```bash
# View current settings
nmcli dev show | grep DNS

# Set DNS servers
nmcli connection modify "Ethernet" ipv4.dns "1.1.1.1,1.0.0.1"
```

#### Traditional resolv.conf
```bash
# /etc/resolv.conf
nameserver 1.1.1.1
nameserver 1.0.0.1
options edns0
options trust-ad
```

### macOS

#### GUI Configuration
```
System Preferences → Network → Advanced → DNS
```

#### Terminal Configuration
```bash
# View current settings
scutil --dns

# Set DNS servers
networksetup -setdnsservers Wi-Fi 1.1.1.1 1.0.0.1
```

## ISP DNS Servers

### Advantages
- Often geographically closer
- Potentially faster responses
- Optimized for the ISP's network
- Automatic configuration via DHCP

### Disadvantages
- Limited privacy
- Potential logging
- DNS hijacking for ads/search
- Variable performance
- Limited security features

## Public DNS Services

### Popular Providers

#### Cloudflare (1.1.1.1)
```
Primary: 1.1.1.1
Secondary: 1.0.0.1
IPv6: 2606:4700:4700::1111
DoT: cloudflare-dns.com
DoH: https://cloudflare-dns.com/dns-query
```
- Privacy focused
- No logging
- DNSSEC validation
- Malware blocking (1.1.1.2)
- Family filtering (1.1.1.3)

#### Google (8.8.8.8)
```
Primary: 8.8.8.8
Secondary: 8.8.4.4
IPv6: 2001:4860:4860::8888
DoT: dns.google
DoH: https://dns.google/dns-query
```
- High reliability
- Global presence
- DNSSEC validation
- Limited logging

#### Quad9 (9.9.9.9)
```
Primary: 9.9.9.9
Secondary: 149.112.112.112
IPv6: 2620:fe::fe
DoT: dns.quad9.net
DoH: https://dns.quad9.net/dns-query
```
- Security focused
- Malware blocking
- DNSSEC validation
- No logging

### Feature Comparison

| Feature | Cloudflare | Google | Quad9 |
|---------|------------|--------|-------|
| Speed | ★★★★★ | ★★★★☆ | ★★★★☆ |
| Privacy | ★★★★★ | ★★★☆☆ | ★★★★★ |
| Security | ★★★★☆ | ★★★★☆ | ★★★★★ |
| Filtering | ★★★★☆ | ★★☆☆☆ | ★★★★★ |
| Coverage | ★★★★☆ | ★★★★★ | ★★★★☆ |

## DNS Filtering and Security

### Local Filtering Solutions

#### Pi-hole
```bash
# Installation
curl -sSL https://install.pi-hole.net | bash

# Configuration
/etc/pihole/
```
- Network-wide ad blocking
- Custom blocklists
- DHCP server option
- Web interface

#### AdGuard Home
```bash
# Docker installation
docker run --name adguardhome \
    -v /my/path/conf:/opt/adguardhome/conf \
    -v /my/path/work:/opt/adguardhome/work \
    -p 53:53/tcp -p 53:53/udp \
    -p 80:80/tcp -p 443:443/tcp \
    -p 3000:3000/tcp \
    adguard/adguardhome
```
- DNS filtering
- Web interface
- HTTPS/TLS support
- Parental controls

### Filtering Categories
1. **Security**
   - Malware domains
   - Phishing sites
   - Botnet C&C servers

2. **Content**
   - Adult content
   - Gambling
   - Social media
   - Streaming services

3. **Advertising**
   - Ad networks
   - Trackers
   - Analytics

### Blocklist Sources
```bash
# Common blocklist sources
https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts
https://v.firebog.net/hosts/lists.php
https://adaway.org/hosts.txt
```

## DNS-based Ad Blocking

### Methods

#### 1. Local Hosts File
```bash
# /etc/hosts
0.0.0.0 ads.example.com
0.0.0.0 tracking.example.com
```

#### 2. DNS Resolver
```bash
# Unbound configuration
local-zone: "ads.example.com" redirect
local-data: "ads.example.com A 0.0.0.0"
```

#### 3. Network-wide Blocking
```bash
# Pi-hole configuration
/etc/pihole/custom.list
0.0.0.0 ads.example.com
```

### Popular Solutions

#### NextDNS
```
Configuration:
- Custom profiles
- Multiple devices
- Analytics
- DNSSEC support
```

#### AdGuard DNS
```
Family Protection: 94.140.14.15
Default Filtering: 94.140.14.14
Non-filtering: 94.140.14.140
```

## DNS Caching

### Operating System Cache

#### Windows DNS Client
```powershell
# View cache statistics
Get-DnsClientCache

# Clear cache
Clear-DnsClientCache
```

#### Linux DNS Cache
```bash
# systemd-resolved
systemd-resolve --statistics

# nscd
service nscd restart
```

### Resolver Cache

#### Unbound
```
server:
    cache-min-ttl: 3600
    cache-max-ttl: 86400
    msg-cache-size: 128m
    neg-cache-size: 64m
```

#### BIND
```
options {
    max-cache-size 256m;
    max-ncache-ttl 3600;
};
```

## Performance Considerations

### Optimization Tips

1. **Use Closest Servers**
   ```bash
   # Test latency
   for dns in 1.1.1.1 8.8.8.8 9.9.9.9; do
       ping -c 1 $dns | grep time=
   done
   ```

2. **Enable DNSSEC**
   ```bash
   # Check DNSSEC validation
   dig +dnssec example.com
   ```

3. **Implement Caching**
   ```bash
   # Unbound configuration
   server:
       cache-min-ttl: 3600
       prefetch: yes
       prefetch-key: yes
   ```

4. **Monitor Performance**
   ```bash
   # Query time monitoring
   dig +stats example.com
   ```

## Privacy and Security

### DNS Privacy Options

#### 1. DNS over TLS (DoT)
```bash
# systemd-resolved configuration
[Resolve]
DNS=1.1.1.1
DNSOverTLS=yes
```

#### 2. DNS over HTTPS (DoH)
```bash
# Firefox DoH configuration
about:config
network.trr.mode = 2
network.trr.uri = https://cloudflare-dns.com/dns-query
```

#### 3. DNSCrypt
```bash
# dnscrypt-proxy configuration
server_names = ['cloudflare', 'google']
```

### Security Best Practices

1. **Enable DNSSEC**
2. **Use encrypted DNS transport**
3. **Implement filtering**
4. **Regular monitoring**
5. **Keep software updated**

## Additional Resources
- [DNS Privacy Project](https://dnsprivacy.org/)
- [DNS Privacy Test](https://dnsprivacy.org/dns_privacy_test.html)
- [Blocklist Tools](https://firebog.net/)
- [DNS Performance](https://www.dnsperf.com/)
