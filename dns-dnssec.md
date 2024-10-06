# DNSSEC - Overview

This page provides an overview to understanding and implementing Domain Name System Security Extensions (DNSSEC).

## Table of Contents
- [DNSSEC Overview](#dnssec-overview)
- [Record Types](#record-types)
- [Algorithms](#algorithms)
- [Key Management](#key-management)
- [Chain of Trust](#chain-of-trust)
- [Implementation Guide](#implementation-guide)
- [Validation Process](#validation-process)
- [Common Issues](#common-issues)

## DNSSEC Overview

DNSSEC provides authentication and integrity verification for DNS responses through cryptographic signatures. It creates a chain of trust from the root zone down to individual domain records.

### Core Functions
1. Origin authentication of DNS data
2. Data integrity verification
3. Authenticated denial of existence
4. Key distribution

## Record Types

### DNSKEY Record
Used to store public keys used in zone signing.

#### Format
```
example.com. IN DNSKEY flags proto algorithm public-key
```

#### Flags Field Values
- `256` (Zone Signing Key - ZSK)
  - Used to sign zone data
  - Rotated more frequently
- `257` (Key Signing Key - KSK)
  - Signs DNSKEY records
  - Used in DS records
  - Changed less frequently

#### Protocol Field
- Always `3` for DNSSEC

#### Example
```
example.com. IN DNSKEY 256 3 13 ( /* ZSK, ECDSA P-256, SHA-256 */
    mdsswUyr3DPW132mOi8V9xESWE8jTo0d xCjjnopKl+GqJxpVXckHAeF+KkxLbxIL
    fDLUT0rAK9iUzy1L53eKGQ== )
```

### RRSIG Record
Contains the cryptographic signature for a record set.

#### Format
```
name TTL class RRSIG type algorithm labels original-ttl expiration inception key-tag signer-name signature
```

#### Example
```
www.example.com. 3600 IN RRSIG A 13 3 3600 (
    20240615230000 20240515230000 12345 example.com.
    AISGD23dF2d1f2... )
```

### DS Record
Delegates signing to create chain of trust. Published in parent zone.

#### Format
```
example.com. IN DS key-tag algorithm digest-type digest
```

#### Digest Types
1. `1` - SHA-1 (deprecated)
2. `2` - SHA-256 (recommended)
3. `4` - SHA-384

#### Example
```
example.com. IN DS 12345 13 2 (
    3ABC456789ABCDEF0123456789ABCDEF0123456789ABCDEF0123456789ABCDEF )
```

### NSEC/NSEC3 Records
Provide authenticated denial of existence.

#### NSEC Format
```
name TTL class NSEC next-domain-name [record-types]
```

#### Example
```
alpha.example.com. 3600 IN NSEC beta.example.com. A MX RRSIG NSEC
```

#### NSEC3 Format
```
label.example.com. TTL IN NSEC3 algorithm flags iterations salt next-hashed-owner [record-types]
```

#### Example
```
2t7b4g4vsa5smi47k61mv5bv1a22bojr.example.com. 3600 IN NSEC3 1 1 12 aabbccdd (
    2vptu5timamqttgl4luu9kg21e0aor3s A RRSIG )
```

## Algorithms

### Signing Algorithms
| Number | Name | Description | Status |
|--------|------|-------------|---------|
| 3 | DSA/SHA1 | DSA with SHA-1 | Historic |
| 5 | RSASHA1 | RSA with SHA-1 | Not Recommended |
| 6 | DSA-NSEC3-SHA1 | DSA with NSEC3/SHA-1 | Historic |
| 7 | RSASHA1-NSEC3-SHA1 | RSA with NSEC3/SHA-1 | Not Recommended |
| 8 | RSASHA256 | RSA with SHA-256 | Recommended |
| 10 | RSASHA512 | RSA with SHA-512 | Recommended |
| 13 | ECDSAP256SHA256 | ECDSA P-256 with SHA-256 | Recommended |
| 14 | ECDSAP384SHA384 | ECDSA P-384 with SHA-384 | Recommended |
| 15 | ED25519 | Edwards-curve DSA, SHA-512 | Recommended |
| 16 | ED448 | Edwards-curve DSA, SHA-912 | Recommended |

### Algorithm Selection Guidelines

#### Recommended Choices
1. **ECDSAP256SHA256 (13)**
   - Good balance of security and performance
   - Smaller signatures than RSA
   - Widely supported

2. **RSASHA256 (8)**
   - Very widely supported
   - Good security
   - Larger signatures than ECDSA

#### Key Sizes
- RSA: Minimum 2048 bits (3072+ recommended)
- ECDSA P-256: 256 bits
- ECDSA P-384: 384 bits
- ED25519: 256 bits
- ED448: 456 bits

## Key Management

### Key Types

#### Zone Signing Key (ZSK)
- Signs zone data
- Flag value: 256
- More frequent rotation (quarterly)
- Smaller key size acceptable

#### Key Signing Key (KSK)
- Signs DNSKEY records
- Flag value: 257
- Less frequent rotation (yearly)
- Larger key size recommended

### Key Rollover Procedures

#### ZSK Rollover (Double-Signature Method)
1. Generate new ZSK
2. Add new ZSK to zone (DNSKEY)
3. Sign all records with both keys
4. Remove old signatures
5. Remove old ZSK

```
# Phase 1: Initial state
example.com. IN DNSKEY 256 3 13 <current-ZSK>
example.com. IN RRSIG  ... <signed-with-current-ZSK>

# Phase 2: Add new key
example.com. IN DNSKEY 256 3 13 <current-ZSK>
example.com. IN DNSKEY 256 3 13 <new-ZSK>
example.com. IN RRSIG  ... <signed-with-current-ZSK>
example.com. IN RRSIG  ... <signed-with-new-ZSK>

# Phase 3: Final state
example.com. IN DNSKEY 256 3 13 <new-ZSK>
example.com. IN RRSIG  ... <signed-with-new-ZSK>
```

#### KSK Rollover (Double-DS Method)
1. Generate new KSK
2. Add new KSK to zone
3. Submit new DS to parent
4. Wait for parent propagation
5. Remove old KSK
6. Remove old DS from parent

```
# Phase 1: Initial state
example.com. IN DNSKEY 257 3 13 <current-KSK>
parent.     IN DS     ... <current-DS>

# Phase 2: Add new key
example.com. IN DNSKEY 257 3 13 <current-KSK>
example.com. IN DNSKEY 257 3 13 <new-KSK>
parent.     IN DS     ... <current-DS>
parent.     IN DS     ... <new-DS>

# Phase 3: Final state
example.com. IN DNSKEY 257 3 13 <new-KSK>
parent.     IN DS     ... <new-DS>
```

## Chain of Trust

### Trust Anchor Configuration
```
. IN DS 20326 8 2 E06D44B80B8F1D39A95C0B0D7C65D08458E880409BBC683457104237C7F8EC8D
```

### Validation Process
1. Start from trust anchor
2. Follow DS records down the chain
3. Verify DNSKEY using DS
4. Verify RRSIG using DNSKEY
5. Verify record data using RRSIG

## Implementation Guide

### Basic BIND Configuration
```
options {
    dnssec-enable yes;
    dnssec-validation auto;
    dnssec-lookaside auto;
};

zone "example.com" {
    type master;
    file "example.com.signed";
    key-directory "/etc/bind/keys";
    inline-signing yes;
    auto-dnssec maintain;
};
```

### Key Generation
```bash
# Generate KSK
dnssec-keygen -a ECDSAP256SHA256 -b 256 -n ZONE -f KSK example.com

# Generate ZSK
dnssec-keygen -a ECDSAP256SHA256 -b 256 -n ZONE example.com
```

### Zone Signing
```bash
dnssec-signzone -A -3 $(head -c 1000 /dev/random | sha1sum | cut -b 1-16) \
    -N INCREMENT -o example.com -t example.com.zone
```

## Validation Process

### Step-by-Step Verification
1. **DS Record Lookup**
   ```bash
   dig DS example.com @8.8.8.8
   ```

2. **DNSKEY Verification**
   ```bash
   dig DNSKEY example.com @8.8.8.8 +dnssec
   ```

3. **Record Signature Check**
   ```bash
   dig A example.com @8.8.8.8 +dnssec
   ```

### Common Tools
- `dig` with DNSSEC options
- `delv` (DNS lookup and validation)
- `dnssec-verify`
- `dnssec-checker`

## Common Issues

### Troubleshooting Guide

#### 1. SERVFAIL with DNSSEC
- Check if DS record matches KSK
- Verify time synchronization
- Check signature expiration

#### 2. Key Rollover Issues
- Timing problems
- Parent zone publication delays
- Old key removal too early

#### 3. Algorithm Migration
- Compatibility issues
- Incomplete signing
- Missing algorithms

### Best Practices
1. Use modern algorithms (ECDSA P-256, ED25519)
2. Regular key rotation
3. Monitor signature expiration
4. Regular validation testing
5. Maintain accurate time synchronization
6. Document key management procedures
7. Use automated tools for maintenance

## Additional Resources
- [RFC 4033](https://tools.ietf.org/html/rfc4033) - DNSSEC Introduction
- [RFC 4034](https://tools.ietf.org/html/rfc4034) - Resource Records
- [RFC 4035](https://tools.ietf.org/html/rfc4035) - Protocol
- [RFC 6781](https://tools.ietf.org/html/rfc6781) - Operational Practices
