# Data Classification and Handling

Last Updated: May 12, 2026

## 1. Overview

### 1.1 Purpose
To define how data is categorized and the specific security controls required for each category to prevent unauthorized disclosure or loss.

### 1.2 Scope
Applies to all data owned, processed, or stored by Lutervyn.

## 2. Data Classification Levels

### 2.1 Level 1: Restricted (Sensitive)
- **Definition**: Data that, if disclosed, would cause severe damage to Lutervyn, its customers, or its partners.
- **Examples**: PII (Personal Identifiable Information), financial records, trade secrets, encryption keys, source code for core algorithms.

### 2.2 Level 2: Private (Confidential)
- **Definition**: Internal data intended for use within Lutervyn only.
- **Examples**: Internal memos, employee contact lists, product roadmaps, non-public research.

### 2.3 Level 3: Public
- **Definition**: Information that has been approved for public release.
- **Examples**: Public marketing materials, official press releases, open-source projects, published documentation.

## 3. Handling Requirements

| Control | Restricted | Private | Public |
| :--- | :--- | :--- | :--- |
| **Storage** | Encrypted at rest (AES-256) | Standard secure cloud storage | Any |
| **Transmission** | Encrypted in transit (TLS 1.2+) | Encrypted in transit | Any |
| **Access** | Multi-factor Auth + Logging | Password + MFA | Any |
| **Labels** | Header/Footer "RESTRICTED" | Optional | None |
| **Sharing** | Signed NDA + Approved Vendor | Internal only | Public |
| **Disposal** | Secure Wipe/Destruction | Secure Wipe | Standard Recycle |

## 4. Labeling Data

- Digital files classified as **Restricted** must include a watermark or header/footer indicating the classification.
- Physical documents must be marked clearly on the first page and stored in a locked cabinet when not in use.

## 5. Incident Reporting

- Any unauthorized access, disclosure, or loss of **Restricted** or **Private** data must be reported immediately according to the [Incident Response Plan](../compliance-risk/Business%20Incident%20Response%20Plan.md).

---

**Effective Date**: May 12, 2026
**Next Review**: May 12, 2027
