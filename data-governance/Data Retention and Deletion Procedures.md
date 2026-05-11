# Data Retention and Deletion Procedures

Last Updated: May 12, 2026

## 1. Overview

### 1.1 Purpose
To ensure that data is only kept for as long as it is needed for legal, business, or regulatory purposes, and is disposed of securely thereafter.

### 1.2 Principles
- **Storage Limitation**: Data should not be kept longer than necessary.
- **Accountability**: Department heads are responsible for managing data within their areas.

## 2. Retention Schedule

| Data Category | Retention Period | Justification |
| :--- | :--- | :--- |
| **Customer Personal Data** | 5 years after account closure | Contractual/Legal requirement |
| **Financial/Tax Records** | 7 years | Statutory requirement |
| **Employee Files** | 6 years after termination | HR/Legal requirement |
| **Signed Contracts** | 7 years after expiry | Legal requirement |
| **System Logs** | 1 year | Security auditing |
| **Marketing Inquiries** | 2 years | Business interest |
| **Email/Slack Messages** | Permanent (or 7 years) | Internal policy |

## 3. Deletion Procedures

### 3.1 Digital Data
- Data must be deleted using methods that make recovery impossible.
- For cloud storage (e.g., AWS, GCP), use the provider's official deletion APIs.
- For physical storage, follow the [Device Management and Asset Lifecycle](../it-ops/Device%20Management%20and%20Asset%20Lifecycle.md) policy.

### 3.2 Physical Data
- Paper documents containing **Restricted** or **Private** information must be shredded using a cross-cut shredder (P-4 security level or higher).

## 4. Deletion of Customer Data

- Upon customer request (DSAR), all personal data must be deleted within 30 days, unless a legal exception applies.
- Backup data must be overwritten or deleted during the next standard backup rotation cycle.

## 5. Exceptions

- **Legal Hold**: Data subject to a legal hold or investigation must not be deleted until the hold is officially lifted by the Legal department.
- **Anonymization**: Data that has been fully anonymized (so that it no longer identifies an individual) is exempt from this policy.

---

**Effective Date**: May 12, 2026
**Next Review**: May 12, 2027
