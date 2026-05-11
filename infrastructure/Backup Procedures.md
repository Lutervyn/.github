# Backup Procedures

Last Updated: May 12, 2026

## 1. Backup Strategy Overview

### 1.1 Purpose

Maintain reliable backups of all critical systems and data to enable recovery from data loss, corruption, or disaster.

### 1.2 Backup Philosophy

- **Redundancy**: multiple backup copies maintained
- **Geographic**: backups in multiple regions
- **Frequent**: backups taken regularly
- **Tested**: backups verified through restoration tests
- **Encrypted**: backups encrypted at-rest and in-transit

### 1.3 Responsible Parties

- Infrastructure Lead: backup planning and strategy
- DevOps Engineer: backup implementation and automation
- DBA: database backup verification
- Security: encryption and access control

## 2. Backup Classification

### 2.1 Backup Tiers

**Tier 1: Critical Systems (RTO 1 hour, RPO 1 hour)**
- Production database (PostgreSQL)
- Production Redis cache (configuration)
- Application configuration files
- Encryption keys and secrets

**Tier 2: Important Systems (RTO 4 hours, RPO 4 hours)**
- User uploads and files (S3)
- Application logs (CloudWatch)
- Business analytics data (Redshift)
- Monitoring and alerting configuration

**Tier 3: Standard Systems (RTO 1 day, RPO 1 day)**
- Development environment data
- Staging environment data
- Historical logs (>30 days)
- Non-critical configuration

## 3. Database Backups

### 3.1 RDS PostgreSQL Backups

**Automated Backups**
- Frequency: daily at 3:00 AM UTC
- Retention: 30 days (can restore to any point)
- Type: full backup + transaction logs
- Location: multi-AZ (us-east-1)
- Cross-region: replicated to us-west-2

**Manual Snapshots**
- Timing: before major changes
- Retention: indefinite (until manually deleted)
- Naming: descriptive (e.g., pre-migration-2026-05-12)
- Archive: annually archive to long-term storage

**Backup Verification**
- Frequency: weekly
- Method: restore to staging, verify data
- Automated: CloudFormation test recovery
- Documented: results logged and reviewed

### 3.2 Backup Encryption

**Encryption Details**
- Algorithm: AES-256
- Key management: AWS Key Management Service (KMS)
- Key rotation: annual
- Access: restricted to authorized personnel

**Key Management**
- Primary key: prod-database-key
- Backup key: backup-archive-key
- Rotation: December annually
- Documentation: key inventory maintained

### 3.3 Backup Retention

**Retention Policy**
- Daily backups: 30 days
- Monthly archives: 7 years
- Compliance: GDPR requires 7-year retention
- Purge: automatic deletion per policy

**Archival**
- Timing: last day of each month
- Destination: S3 Glacier
- Naming: YYYY-MM archive format
- Verification: checksums stored

## 4. Application Backups

### 4.1 Code Repository Backups

**GitHub Repository**
- Primary: GitHub organization repository
- Backup: weekly mirror to GitLab
- Retention: indefinite (full history)
- Encryption: HTTPS only, SSH keys secured

**Backup Procedure**
1. GitHub Actions triggered weekly
2. Mirror clone pushed to GitLab
3. Verification: commit count verified
4. Notification: slack alert if failed

### 4.2 Application Configuration Backups

**Backup Targets**
- Docker images: ECR repository (immutable)
- Infrastructure code: GitHub repository
- Environment variables: AWS Secrets Manager
- SSL certificates: AWS Certificate Manager

**Backup Schedule**
- Docker images: every deployment
- IaC: every commit (version controlled)
- Secrets: daily snapshot to S3
- Certificates: upon renewal/update

## 5. Data and Storage Backups

### 5.1 S3 Backups

**User Data Buckets**
- Versioning: enabled (all versions kept)
- Replication: cross-region replication to us-west-2
- Lifecycle: archive to Glacier after 30 days
- Retention: 7 years minimum

**Backup Buckets**
- Purpose: database snapshot storage
- Retention: 7 years
- Encryption: enabled
- Access: restricted to backup user

**Logging and Audit**
- Bucket logging: enabled
- Access logging: S3 access logs
- CloudTrail: all API calls logged
- Review: monthly access review

### 5.2 File-Level Backups

**Application Files**
- Location: /opt/application
- Backup: daily tar.gz archive
- Destination: S3 backup bucket
- Retention: 30 days

**Configuration Files**
- Location: /etc/config
- Backup: daily with application
- Version: tracked with git
- Retention: indefinite

## 6. Email and Communication Backups

### 6.1 Email Backups

**Gmail Backup (Google Workspace)**
- Method: IMAP backup via Spanning Cloud Apps
- Frequency: daily
- Retention: indefinite
- Encryption: encrypted via Spanning

**Email Archival**
- Purpose: compliance and legal hold
- Retention: 7 years
- Search: indexed for searchability
- Access: restricted to legal/compliance

## 7. Backup Restore Procedures

### 7.1 Database Restore

**Full Database Restore**
1. Determine restore point (date/time)
2. Create new RDS instance from snapshot
3. Verify schema and data integrity
4. Update application connection string
5. Perform data validation
6. Monitor for errors
7. Backup old database (if needed)
8. Delete old instance

**Point-in-Time Restore**
1. Determine recovery point object (RPO)
2. Initiate point-in-time recovery
3. Select specific date/time
4. Create restored database instance
5. Verify restoration success
6. Validation queries
7. Switch to restored database
8. Document recovery details

**Partial Restore**
1. Restore to temporary database
2. Extract specific tables/data
3. Restore to production
4. Verify relationships
5. Cleanup temporary database

### 7.2 File System Restore

**S3 Restore**
1. Identify missing/corrupted files
2. List versions in S3
3. Download previous version
4. Verify file integrity (checksum)
5. Restore to application
6. Verify functionality
7. Document restore

**EBS Restore**
1. Create volume from snapshot
2. Attach to running instance
3. Mount filesystem
4. Verify data integrity
5. Detach if temporary
6. Document restore

## 8. Backup Testing and Verification

### 8.1 Backup Verification Schedule

**Daily Verification**
- Backup completion: monitoring alerts
- Size check: verify file size reasonable
- Encryption: verify encrypted status
- Location: confirm in backup storage

**Weekly Verification**
- Restore test: restore sample data
- Checksum: verify file integrity
- Documentation: log test results
- Alerts: any issues notify team

**Monthly Verification**
- Full restore test: restore entire database
- Performance: verify restore speed
- Integrity: run data validation
- Documentation: document findings

**Quarterly Verification**
- Full RTO/RPO test: simulate real scenario
- Documentation: create/update runbooks
- Team training: practice procedures
- Metrics: review backup metrics

### 8.2 Restoration Testing

**Test Procedure**
1. Identify backup to test
2. Restore to test environment
3. Run validation queries
4. Verify all data present
5. Check data integrity (row counts, checksums)
6. Verify file sizes
7. Document results
8. Cleanup test environment

**Validation Queries**
- Row counts: verify record counts match
- Checksums: compare data checksums
- Relationships: verify foreign keys
- Indexes: verify index integrity
- Performance: verify query performance

## 9. Disaster Recovery Procedures

### 9.1 Full System Recovery

**Recovery from Total Failure**
1. Assess damage scope
2. Retrieve latest backup
3. Restore infrastructure from IaC
4. Restore database from snapshot
5. Restore application files
6. Verify system functionality
7. Update DNS if region change
8. Monitor for issues
9. Post-incident review

**Timeline**
- T+0: Issue detected
- T+30min: Backup selected, restoration started
- T+60min: Database restored
- T+90min: Application running
- T+120min: Verification complete

### 9.2 Partial Recovery

**Scenario: Database Corruption**
1. Identify corruption scope
2. Determine last clean backup
3. Restore database
4. Identify lost data
5. Determine recovery action
6. Document incident

**Scenario: User Account Compromise**
1. Identify affected account
2. Restore user data from backup
3. Reset credentials
4. Verify no system compromise
5. Document incident

## 10. Backup Storage and Security

### 10.1 Storage Locations

**Primary Location**
- Location: AWS S3 (us-east-1)
- Redundancy: automatic 3-copy redundancy
- Availability: 99.99%
- Cost: $X per month

**Secondary Location (Disaster Recovery)**
- Location: AWS S3 (us-west-2)
- Replication: cross-region enabled
- Availability: 99.99%
- Cost: $X per month

**Archive Location (Long-term)**
- Location: AWS S3 Glacier (us-east-1)
- Retention: 7 years
- Cost: $X per month
- Retrieval time: 1-5 hours

### 10.2 Security Controls

**Access Control**
- IAM policy: least privilege
- MFA: required for deletion
- Encryption: required
- Audit: access logged

**Encryption**
- At-rest: AES-256 via KMS
- In-transit: TLS 1.2+
- Key management: AWS KMS
- Rotation: annual

**Compliance**
- GDPR: compliant retention
- HIPAA: if applicable, encrypted at-rest
- SOC 2: audit trail maintained
- PCI-DSS: if applicable, separated

## 11. Backup Lifecycle Management

### 11.1 Retention Policies

**Daily Backups**
- Retention: 30 days
- Deletion: automatic after 30 days
- Cost: $X per backup

**Monthly Archives**
- Retention: 7 years
- Deletion: automatic after 7 years
- Cost: $X per archive

**Special Retention**
- Compliance holds: indefinite
- Legal holds: indefinite
- SEC hold: 7 years minimum
- Deletion: manual only

### 11.2 Backup Cleanup

**Automated Cleanup**
- Lifecycle policy: automatic expiration
- Frequency: daily
- Verification: manual spot check
- Logging: cleanup logged

**Manual Cleanup**
- Approval: infrastructure lead
- Request: via ticket system
- Verification: before deletion
- Documentation: reason recorded

## 12. Backup Monitoring and Alerting

### 12.1 Monitoring

**Backup Metrics**
- Duration: backup completion time
- Size: backup file size
- Success rate: percentage succeeded
- Restore time: RTO verification

**Alerts**
- Failed backup: immediate alert
- Late backup: if not completed by 5 AM UTC
- Large backup: if growth >20%
- Storage: if usage >80%

### 12.2 Reporting

**Daily Report**
- Backup status: all backups listed
- Success/failure: count of each
- Issues: any problems noted

**Weekly Report**
- Backup summary: overview
- Verification results: testing outcome
- Issues: any issues encountered
- Action items: items for resolution

**Monthly Report**
- Backup trends: trend analysis
- Verification testing: results
- Incidents: any incidents
- Recommendations: improvements

## 13. Disaster Recovery Contact

- Infrastructure Lead: [name], infrastructure@lutervyn.com, [phone]
- On-Call: [phone], PagerDuty escalation
- Emergencies: incidents@lutervyn.com, [phone]
- Backup Questions: backups@lutervyn.com

---

**Effective Date**: May 12, 2026
**Last Updated**: May 12, 2026
**Review**: Quarterly

*All employees responsible for understanding backup procedures. Infrastructure team maintains backup systems.*
