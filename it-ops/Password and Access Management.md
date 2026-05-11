# Password and Access Management

Last Updated: May 12, 2026

## 1. Overview

### 1.1 Purpose
To protect Lutervyn's systems and data by ensuring only authorized individuals have access to the resources they need to perform their jobs.

### 1.2 Principles
- **Least Privilege**: Users are granted the minimum level of access required.
- **Need to Know**: Access is based on specific job requirements.
- **Accountability**: Every action in a system must be traceable to a specific user.

## 2. Password Standards

### 2.1 Complexity Requirements
- Minimum 14 characters.
- Must include: uppercase, lowercase, numbers, and special characters.
- Must not contain: usernames, dictionary words, or common sequences (e.g., "123456").

### 2.2 Multi-Factor Authentication (MFA)
- MFA is **mandatory** for all Lutervyn accounts (SSO, VPN, Cloud Portals).
- Preferred methods: Authenticator apps (e.g., Okta, Google Authenticator) or Hardware Keys (e.g., YubiKey).
- SMS-based MFA is discouraged and only used as a last resort.

### 2.3 Password Managers
- Employees must use the company-approved password manager (**1Password/LastPass**) for all business credentials.
- Storing passwords in browsers or unencrypted files is strictly prohibited.

## 3. User Access Lifecycle

### 3.1 Provisioning (Onboarding)
- HR triggers access requests based on role.
- IT provisions accounts through the centralized Identity Provider (IdP).

### 3.2 Access Reviews
- Managers must review their team's access levels quarterly.
- Unused accounts or excessive permissions will be revoked.

### 3.3 De-provisioning (Offboarding)
- Upon termination, all access is revoked **immediately** (within 1 hour of the departure time).
- Critical accounts (e.g., SysAdmin) are rotated immediately upon the departure of a privileged user.

## 4. Privileged Access

- Privileged accounts (e.g., root, admin) must be kept to an absolute minimum.
- Use of "shared" admin accounts is prohibited.
- Privileged actions should be logged and audited regularly.

## 5. Remote Access

- Remote access to internal systems must be via the **Company VPN**.
- VPN sessions require MFA and are subject to time-outs after periods of inactivity.

---

**Effective Date**: May 12, 2026
**Next Review**: May 12, 2027
