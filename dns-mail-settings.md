# DNS Mail Settings - Complete Guide

This page provides an overview to DNS mail settings, focusing on email authentication, security, and best practices for domain email configuration.

## Table of Contents
- [Email-Related DNS Records Overview](#email-related-dns-records-overview)
- [MX Records](#mx-records)
- [SPF Records](#spf-records)
- [DKIM Records](#dkim-records)
- [DMARC Records](#dmarc-records)
- [PTR Records for Email](#ptr-records-for-email)
- [Common Configurations](#common-configurations)
- [Troubleshooting](#troubleshooting)

## Email-Related DNS Records Overview

Email authentication relies on several DNS record types working together:
- MX records direct incoming mail to the correct servers
- SPF records specify authorized sending servers
- DKIM records enable cryptographic signing of messages
- DMARC records define authentication policies and reporting
- PTR records enable reverse DNS lookup for sending IPs

## MX Records

### Purpose
MX (Mail Exchanger) records specify which mail servers accept incoming email for your domain.

### Format
```
domain.com. IN MX priority mailserver.domain.com.
```

### Example Configuration
```
example.com. IN MX 10 primary-mail.example.com.
example.com. IN MX 20 backup-mail.example.com.
```

### Key Concepts
- Lower priority numbers (e.g., 10) indicate preferred servers
- Multiple MX records provide fallback options
- Always include at least one backup MX record
- Mail servers should have corresponding A/AAAA records

## SPF Records

### Purpose
Sender Policy Framework (SPF) records specify which mail servers are authorized to send email for your domain.

### Format
```
domain.com. IN TXT "v=spf1 [mechanisms] [qualifier]all"
```

### Mechanisms
- `ip4`: IPv4 addresses or ranges
  - Example: `ip4:192.0.2.0/24`
- `ip6`: IPv6 addresses or ranges
  - Example: `ip6:2001:db8::/32`
- `a`: Domain's A record
  - Example: `a:mail.example.com`
- `mx`: Domain's MX records
  - Example: `mx`
- `include`: Include another domain's SPF record
  - Example: `include:_spf.google.com`
- `exists`: Domain exists check
  - Example: `exists:%{i}._spf.example.com`
- `ptr`: PTR record check (not recommended)
  - Example: `ptr:example.com`

### Qualifiers
- `+`: Pass (default)
- `-`: Fail
- `~`: SoftFail
- `?`: Neutral

### Example Configurations

Basic SPF:
```
example.com. IN TXT "v=spf1 ip4:192.0.2.0/24 mx -all"
```

Google Workspace SPF:
```
example.com. IN TXT "v=spf1 include:_spf.google.com ~all"
```

Complex SPF:
```
example.com. IN TXT "v=spf1 ip4:192.0.2.0/24 ip4:198.51.100.0/24 include:_spf.google.com include:mailchimp.com mx ~all"
```

### SPF Best Practices
1. Use `-all` for strict enforcement
2. Limit DNS lookups to 10 or fewer (includes `include:`, `exists:`, `a:`, `mx:`, `ptr:`)
3. Keep record length under 255 characters
4. Avoid `ptr` mechanism due to performance
5. Document all authorized sending sources

## DKIM Records

### Purpose
DomainKeys Identified Mail (DKIM) enables cryptographic signing of messages to verify authenticity.

### Format
```
selector._domainkey.domain.com. IN TXT "v=DKIM1; k=rsa; p=public-key"
```

### Key Components
- `selector`: Identifies specific DKIM record
- `k`: Key type (usually rsa)
- `p`: Public key
- `t`: Optional flags
- `s`: Service type
- `h`: Acceptable hash algorithms

### Example Configuration
```
google._domainkey.example.com. IN TXT "v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAq6Ii..."
```

### Best Practices
1. Use strong keys (2048-bit RSA minimum)
2. Rotate keys annually
3. Use different selectors for different services
4. Keep backup of private keys
5. Monitor key expiration

## DMARC Records

### Purpose
Domain-based Message Authentication, Reporting, and Conformance (DMARC) defines policy for handling authentication failures.

### Format
```
_dmarc.domain.com. IN TXT "v=DMARC1; p=policy; [options]"
```

### Policy Options (`p=`)
- `none`: Monitor only
- `quarantine`: Send suspicious mail to spam
- `reject`: Reject suspicious mail

### Additional Tags

#### Alignment Mode
- `adkim`: DKIM alignment
  - `s`: Strict
  - `r`: Relaxed (default)
- `aspf`: SPF alignment
  - `s`: Strict
  - `r`: Relaxed (default)

#### Reporting
- `rua`: Aggregate report URI
  - Example: `rua=mailto:dmarc-reports@example.com`
- `ruf`: Forensic report URI
  - Example: `ruf=mailto:forensics@example.com`
- `fo`: Forensic options
  - `0`: Generate for all failures
  - `1`: Generate for any alignment failure
  - `d`: DKIM failure
  - `s`: SPF failure

#### Other Settings
- `pct`: Percentage of messages subject to policy (1-100)
- `ri`: Reporting interval in seconds
- `sp`: Subdomain policy
- `v`: Version (always DMARC1)

### Example Configurations

Basic Monitoring:
```
_dmarc.example.com. IN TXT "v=DMARC1; p=none; rua=mailto:dmarc@example.com"
```

Strict Enforcement:
```
_dmarc.example.com. IN TXT "v=DMARC1; p=reject; rua=mailto:dmarc@example.com; ruf=mailto:forensics@example.com; fo=1; adkim=s; aspf=s"
```

Gradual Implementation:
```
_dmarc.example.com. IN TXT "v=DMARC1; p=quarantine; pct=25; rua=mailto:dmarc@example.com"
```

### DMARC Implementation Strategy

1. **Monitoring Phase**
   ```
   _dmarc.example.com. IN TXT "v=DMARC1; p=none; rua=mailto:dmarc@example.com"
   ```
   - Collect data for at least 2-4 weeks
   - Analyze reports for legitimate mail flows

2. **Testing Phase**
   ```
   _dmarc.example.com. IN TXT "v=DMARC1; p=quarantine; pct=10; rua=mailto:dmarc@example.com"
   ```
   - Start with low percentage
   - Gradually increase `pct` value

3. **Enforcement Phase**
   ```
   _dmarc.example.com. IN TXT "v=DMARC1; p=reject; rua=mailto:dmarc@example.com; ruf=mailto:forensics@example.com"
   ```
   - Move to full rejection after successful testing

## PTR Records for Email

### Purpose
Maps IP addresses to hostnames for reverse DNS lookup.

### Format
```
reversed-ip.in-addr.arpa. IN PTR hostname.
```

### Example
```
34.216.184.93.in-addr.arpa. IN PTR mail.example.com.
```

### Best Practices
1. Ensure PTR records exist for all sending IPs
2. PTR should match HELO/EHLO hostname
3. Keep records updated
4. Use meaningful hostnames

## Common Configurations

### Google Workspace
```
# MX Records
example.com. IN MX 1 aspmx.l.google.com.
example.com. IN MX 5 alt1.aspmx.l.google.com.
example.com. IN MX 5 alt2.aspmx.l.google.com.
example.com. IN MX 10 alt3.aspmx.l.google.com.
example.com. IN MX 10 alt4.aspmx.l.google.com.

# SPF Record
example.com. IN TXT "v=spf1 include:_spf.google.com ~all"

# DKIM Record (selector provided by Google)
google._domainkey.example.com. IN TXT "v=DKIM1; k=rsa; p=your-public-key"

# DMARC Record
_dmarc.example.com. IN TXT "v=DMARC1; p=quarantine; rua=mailto:dmarc@example.com"
```

### Microsoft 365
```
# MX Record
example.com. IN MX 0 example-com.mail.protection.outlook.com.

# SPF Record
example.com. IN TXT "v=spf1 include:spf.protection.outlook.com -all"

# DKIM Record (selector provided by Microsoft)
selector1._domainkey.example.com. IN TXT "v=DKIM1; k=rsa; p=your-public-key"

# DMARC Record
_dmarc.example.com. IN TXT "v=DMARC1; p=quarantine; rua=mailto:dmarc@example.com"
```

## Troubleshooting

### Common Issues
1. **SPF Errors**
   - Too many DNS lookups
   - Record syntax errors
   - Conflicting records

2. **DKIM Failures**
   - Incorrect selector
   - Key rotation issues
   - Record formatting

3. **DMARC Problems**
   - Alignment issues
   - Report delivery failures
   - Policy too strict

### Validation Tools
- [MXToolbox](https://mxtoolbox.com/)
- [Google Admin Toolbox](https://toolbox.googleapps.com/apps/checkmx/)
- [DMARC Analyzer](https://www.dmarcanalyzer.com/)

### Best Practices for Testing
1. Start with monitoring mode
2. Use test domains
3. Gradually increase policy strictness
4. Monitor bounce rates
5. Review authentication reports regularly

## Additional Resources
- [RFC 7208](https://tools.ietf.org/html/rfc7208) - SPF
- [RFC 6376](https://tools.ietf.org/html/rfc6376) - DKIM
- [RFC 7489](https://tools.ietf.org/html/rfc7489) - DMARC
