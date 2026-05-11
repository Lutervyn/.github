# Hosting Infrastructure Documentation

Last Updated: May 12, 2026

## 1. Infrastructure Overview

### 1.1 Infrastructure Philosophy

Lutervyn maintains a secure, scalable, and redundant infrastructure supporting mission-critical operations with 99.9% uptime SLA.

### 1.2 Cloud Provider

**Primary: Amazon Web Services (AWS)**
- Region: us-east-1 (primary), us-west-2 (backup)
- Account: production account (separate dev/staging)
- Billing: consolidated billing contact
- Support: enterprise support plan

### 1.3 Infrastructure Team

- Infrastructure Lead: [Name], infrastructure@lutervyn.com
- DevOps Engineer: [Name], devops@lutervyn.com
- System Admin: [Name], sysadmin@lutervyn.com
- On-call: rotating on-call schedule

## 2. Architecture Overview

### 2.1 Architecture Diagram

**Multi-Tier Architecture**
- Web tier: CloudFront CDN + ALB
- App tier: ECS/Fargate container services
- Database tier: RDS PostgreSQL + Redis cache
- Storage tier: S3 buckets
- Data tier: data warehouse (Redshift)

### 2.2 Components

**Load Balancing**
- Service: Application Load Balancer (ALB)
- Configuration: active-active across AZs
- Health checks: every 15 seconds
- Timeout: 60 second idle timeout
- Target group: ECS services

**Container Orchestration**
- Service: ECS (Elastic Container Service)
- Launch type: Fargate
- Cluster: production cluster
- Task definition: containerized applications
- Auto-scaling: based on CPU/memory metrics

**Database**
- Service: RDS (Relational Database Service)
- Engine: PostgreSQL 14+
- Instance: db.r5.2xlarge
- Storage: 1TB gp3 storage
- Backup: automated daily snapshots

**Caching**
- Service: ElastiCache Redis
- Node type: cache.r6g.xlarge
- Cluster: 3-node cluster
- Eviction policy: allkeys-lru
- Replication: multi-AZ

**Storage**
- Service: S3 (Simple Storage Service)
- Buckets: application data, backups, logs
- Versioning: enabled for backups
- Encryption: at-rest and in-transit
- Lifecycle: transition to Glacier 30 days

**CDN**
- Service: CloudFront
- Distribution: static and dynamic content
- TTL: 1 hour default
- Caching: gzip compression enabled
- Certificate: AWS Certificate Manager

**Monitoring**
- Service: CloudWatch
- Metrics: CPU, memory, network, disk
- Alarms: threshold-based alerts
- Logs: centralized log retention
- Dashboard: operational dashboard

**Security**
- VPC: custom VPC private subnets
- Security groups: restrictive inbound rules
- NACLs: network access control lists
- WAF: Web Application Firewall enabled
- Encryption: TLS 1.2+ required

**DNS**
- Service: Route 53
- Domain: lutervyn.com
- Health checks: active monitoring
- Failover: automatic failover enabled
- TTL: 300 seconds

## 3. Network Architecture

### 3.1 VPC Configuration

**VPC Setup**
- CIDR block: 10.0.0.0/16
- Subnets: 6 subnets (3 public, 3 private)
- AZs: us-east-1a, us-east-1b, us-east-1c
- NAT gateway: in each public subnet
- Internet gateway: attached to VPC

### 3.2 Security Groups

**Web Tier**
- Inbound: HTTP (80), HTTPS (443)
- Outbound: all allowed
- Source: anywhere (0.0.0.0/0)

**App Tier**
- Inbound: application port (8080)
- Outbound: database, cache, external APIs
- Source: web tier security group

**Database Tier**
- Inbound: PostgreSQL (5432)
- Outbound: none (stateful)
- Source: app tier security group

**Cache Tier**
- Inbound: Redis (6379)
- Outbound: none (stateful)
- Source: app tier security group

### 3.3 DDoS Protection

- AWS Shield: standard protection included
- AWS WAF: rules for common attacks
- Rate limiting: CloudFront rate-based rules
- Geographic: IP blocking if needed

## 4. Database Configuration

### 4.1 RDS PostgreSQL

**Configuration**
- Version: PostgreSQL 14.7
- Instance: db.r5.2xlarge (8 vCPU, 64 GB RAM)
- Storage: 1TB gp3
- IOPS: 3000 provisioned
- Throughput: 125 MB/s

**High Availability**
- Multi-AZ: enabled
- Standby: automatic failover
- RPO: 1 minute
- RTO: 1-2 minutes

**Backups**
- Retention: 30 days
- Frequency: daily automated
- Window: 3:00-4:00 AM UTC
- Manual: operator-initiated allowed

**Security**
- Encryption: at-rest enabled
- SSL: required for connections
- KMS: AWS Key Management Service
- Parameter groups: custom secure settings
- Performance Insights: enabled

### 4.2 Database Optimization

**Indexes**
- Primary keys: all tables
- Foreign keys: relational integrity
- Unique: email, username
- Composite: common queries
- Maintenance: analyze and reindex quarterly

**Query Performance**
- Slow log: queries >1 second logged
- EXPLAIN: plan analysis before deployment
- Vacuum: automated weekly
- Statistics: updated regularly

**Connection Pooling**
- Tool: pgBouncer
- Pool size: 100 connections
- Mode: transaction pooling
- Timeout: 30 second idle

### 4.3 Data Warehouse

**Redshift Cluster**
- Purpose: business analytics
- Node type: dc2.large (100GB each)
- Cluster size: 10 nodes
- Storage: 1TB total
- Update cadence: daily snapshots

## 5. Container Configuration

### 5.1 Docker Images

**Base Images**
- Language: Python 3.11 or Node.js 18
- OS: Amazon Linux 2
- Size: <500MB per image
- Registry: AWS ECR (Elastic Container Registry)

**Build Process**
- CI/CD: GitHub Actions
- Build: automated on push to main
- Scan: security scan before push
- Tag: semver versioning (v1.2.3)
- Push: to ECR repository

### 5.2 ECS Tasks

**Task Definition**
- CPU: 2048 units (2 vCPU)
- Memory: 4096 MB (4 GB)
- Container port: 8080
- Health check: HTTP /health
- Logging: CloudWatch logs

**Auto-Scaling**
- Min tasks: 3
- Max tasks: 20
- Target: 70% CPU utilization
- Scale-up: add 1 task per minute
- Scale-down: remove 1 task per 5 minutes

## 6. Application Deployment

### 6.1 Deployment Process

**Pipeline**
1. Developer pushes code to GitHub
2. CI/CD builds Docker image
3. Image scanned for vulnerabilities
4. Image pushed to ECR
5. ECS task updated with new image
6. Rolling deployment (1 at a time)
7. Health checks verify
8. Old tasks terminated

**Deployment Time**
- Typical: 5-10 minutes
- Rollback: available if issues
- Zero-downtime: rolling deployment

### 6.2 Environment Configuration

**Development**
- Cluster: dev-cluster
- Instances: 1-2 small instances
- Data: test data only
- Access: engineers only

**Staging**
- Cluster: staging-cluster
- Instances: 2 medium instances
- Data: production replica (anonymized)
- Access: team leads only

**Production**
- Cluster: prod-cluster
- Instances: 3+ large instances
- Data: production
- Access: restricted

## 7. Storage and Backup

### 7.1 S3 Storage

**Buckets**
- Application data: encryption enabled
- User uploads: versioning enabled
- Backups: MFA delete enabled
- Logs: retention policy 90 days

**Bucket Policies**
- Public access: blocked
- Encryption: required AES-256
- Versioning: enabled
- Lifecycle: transition to Glacier

### 7.2 Backup Strategy

**RDS Backups**
- Automated: daily backups (30-day retention)
- Manual: operator-initiated
- Location: multi-region snapshots
- Retention: production 30 days, archived 7 years

**Database Snapshots**
- Frequency: daily at 3 AM UTC
- Retention: 30 days
- Cross-region: replicated to us-west-2
- Encryption: same KMS key

**Application Data**
- S3 versioning: enabled
- Lifecycle: archive after 30 days
- Backup: daily copy to backup S3 bucket
- Retention: 7 years for compliance

**Disaster Recovery**
- RTO: 1 hour for most systems
- RPO: 1 hour for data
- Test: quarterly DR drills
- Documentation: runbooks maintained

## 8. Monitoring and Alerting

### 8.1 CloudWatch Monitoring

**Metrics**
- CPU: >80% alert, >90% critical
- Memory: >80% alert, >90% critical
- Network: >80% alert, >90% critical
- Disk: >80% alert, >90% critical
- Database: connections, latency, throughput

**Dashboards**
- Overview: all systems status
- Application: application metrics
- Database: database health
- Infrastructure: EC2 and resource metrics

### 8.2 Alerting

**Alert Channels**
- Email: ops@lutervyn.com
- Slack: #infrastructure channel
- PagerDuty: on-call escalation
- SMS: critical alerts only

**Alert Thresholds**
- Critical (P1): immediate response required
- High (P2): within 30 minutes
- Medium (P3): within 2 hours
- Low (P4): non-urgent investigation

### 8.3 Log Management

**Centralized Logging**
- CloudWatch Logs: application logs
- S3: log file storage
- Retention: 30 days in CloudWatch, archived to S3
- Encryption: logs encrypted at-rest

## 9. Security Configuration

### 9.1 IAM Policies

**Role-Based Access**
- Developer: read access to logs, dev environment
- DevOps: full infrastructure access
- DBA: database access only
- Security: audit and compliance access

**Access Keys**
- Rotation: every 90 days
- MFA: required for sensitive operations
- Passwords: 12+ characters, complex
- Storage: AWS Secrets Manager

### 9.2 Encryption

**At-Rest**
- RDS: AWS KMS encryption
- S3: AES-256 encryption
- EBS: encrypted snapshots
- Backups: encrypted

**In-Transit**
- TLS 1.2+: all connections
- Certificates: AWS ACM managed
- HTTPS: enforced (redirect HTTP)
- VPN: required for admin access

### 9.3 Network Security

**VPC Security**
- Private subnets: application tier
- Public subnets: load balancers only
- NAT gateways: for outbound internet
- Security groups: least privilege

**WAF Rules**
- SQL injection: protection enabled
- XSS: cross-site scripting blocked
- Rate limiting: 2000 requests per minute
- Geographic: block high-risk regions if applicable

## 10. Cost Optimization

### 10.1 Cost Management

**Cost Allocation**
- Tags: project and department tagging
- Budget alerts: monthly budget monitoring
- Reserved instances: 30% savings
- Spot instances: non-critical workloads

**Monthly Costs**
- Target: $X per month
- Compute: 40% of budget
- Database: 30% of budget
- Storage: 20% of budget
- Networking: 10% of budget

### 10.2 Right-Sizing

**Review Process**
- Frequency: quarterly
- Tools: AWS Compute Optimizer
- Assessment: utilization trends
- Action: downsize if underutilized
- Savings: track cost reductions

## 11. Disaster Recovery

### 11.1 RTO and RPO

**Recovery Objectives**
- RTO (Recovery Time): 1 hour
- RPO (Recovery Point): 1 hour data loss maximum
- Frequency: test quarterly
- Documentation: runbooks maintained

### 11.2 Failover Procedures

**Database Failover**
1. Promote read replica to primary
2. Update application connection string
3. Verify data integrity
4. Monitor for errors
5. Perform detailed review

**Application Failover**
1. Route traffic to backup region
2. Verify service functionality
3. Monitor error rates
4. Resume normal operations
5. Document incident

## 12. Questions & Contact

- Infrastructure: infrastructure@lutervyn.com
- DevOps: devops@lutervyn.com
- On-Call: [phone number]
- Emergencies: incidents@lutervyn.com

---

**Effective Date**: May 12, 2026
**Last Updated**: May 12, 2026
