# Cryptographic Standards

Last Updated: May 12, 2024

## 1. Encryption Standards

### 1.1 Data at Rest

- Algorithm: AES-256
- Mode: GCM (Galois/Counter Mode)
- Key length: 256-bit
- Key rotation: Annual
- Archived: 10 years

### 1.2 Data in Transit

- Protocol: TLS 1.2 minimum
- Cipher suites: AES-GCM
- ECDHE key exchange
- Forward secrecy required
- Perfect forward secrecy (PFS)
- HSTS header required

## 2. Key Management

### 2.1 Generation

- Cryptographically secure PRNG
- Sufficient entropy
- No hardcoded keys
- Environment-specific
- Unique per deployment
- Documented process

### 2.2 Storage

- Hardware security modules (HSM)
- Or secure vaults
- Access controls
- Audit logging
- Encrypted storage
- Limited access

### 2.3 Rotation

- Annual key rotation
- Triggered by expiration
- Rolling migration
- No service disruption
- Old keys retained temporarily
- Documented rotation

## 3. Hashing

### 3.1 Algorithms

- SHA-256 minimum
- SHA-512 preferred
- Never: MD5, SHA-1
- Password: bcrypt, scrypt
- PBKDF2 acceptable
- Salt: unique per hash

### 3.2 Usage

- Passwords: salted + hashed
- API signatures: HMAC-SHA256
- File integrity: SHA-512
- Checksums: SHA-256

## 4. Certificates

### 4.1 SSL/TLS Certificates

- Issued by trusted CA
- Minimum 2048-bit RSA or 256-bit ECDSA
- Validated domain ownership
- Extended validation recommended
- Annual renewal
- Wildcard for subdomains

### 4.2 Certificate Management

- Centralized inventory
- Automated renewal
- Expiration monitoring
- Revocation capability
- Documented process
- Audit trail

## 5. Random Number Generation

### 5.1 Requirements

- Cryptographically secure
- Non-predictable output
- Sufficient entropy
- Authenticated randomness
- Tested periodically

### 5.2 Sources

- /dev/urandom (Linux)
- CryptGenRandom (Windows)
- Java SecureRandom
- Python secrets module
- Never: Math.random()

## 6. Compliance

### 6.1 Standards

- NIST guidelines
- FIPS 140-2 modules
- Industry best practices
- Regular assessment
- Documented compliance
- Third-party validation

## 7. Contact

- Security: security@lutervyn.com
- Cryptography: crypto@lutervyn.com
