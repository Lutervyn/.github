# Disaster Recovery Runbooks

Last Updated: May 12, 2026

## 1. Disaster Recovery Overview

### 1.1 Purpose

Provide step-by-step procedures for responding to and recovering from disasters affecting Lutervyn's systems and operations.

### 1.2 Recovery Objectives

**RTO (Recovery Time Objective)**
- Most systems: 1 hour
- Critical systems: 30 minutes
- Non-critical: 4 hours

**RPO (Recovery Point Objective)**
- Databases: 1 hour (last backup)
- Files: 1 hour
- Logs: 1 day

### 1.3 Incident Team

- Incident Commander: CEO or VP Ops (decision authority)
- Infrastructure Lead: technical lead
- DevOps: implementation
- Communications: customer/employee updates
- Legal: if needed

## 2. Database Failure Recovery

### 2.1 Scenario: RDS Failover

**Situation**
- Primary database unresponsive
- Replication lag or failover needed
- Application unable to connect

**Detection**
- CloudWatch alarm: database connection failure
- Application alerts: connection pool exhausted
- Monitor: CPU 100% or high latency

**Response Timeline**

**Immediate (T+0)**
1. Incident Commander declares incident
2. Open incident channel in Slack (#incident)
3. Document issue: time detected, symptoms, impact
4. Notify customer success team

**Within 5 minutes (T+5)**
1. Infrastructure Lead checks RDS console
2. Verify Multi-AZ failover status
3. Check event logs for errors
4. Attempt automatic failover if enabled

**Within 10 minutes (T+10)**
1. If automatic failover: monitor standby promotion
2. Monitor: verify standby promotion complete
3. Update: connection strings in application if needed
4. Notify: customer of status

**Recovery Steps**

1. **Verify Failure**
   - Check RDS console: primary status
   - Check events: failure reason
   - Check CloudWatch: error messages

2. **Initiate Failover**
   ```
   AWS CLI: aws rds failover-db-instance --db-instance-identifier prod-postgres
   OR
   AWS Console: RDS > Instances > prod-postgres > Actions > Failover
   ```

3. **Wait for Failover**
   - Standby promotion: 1-2 minutes typical
   - DNS propagation: 1-5 minutes
   - Application reconnection: automatic

4. **Verify Recovery**
   - Database connectivity: test query
   - Application health: verify endpoints
   - CloudWatch: check metrics
   - Logs: review error logs

5. **Post-Failover**
   - Document: failover reason and duration
   - Notify: all stakeholders
   - Monitor: 30-minute window
   - Investigate: root cause analysis

**Communication**
- Customer notification: within 15 minutes
- Status updates: every 15 minutes during incident
- Post-incident: within 1 hour root cause

## 3. Application Failure Recovery

### 3.1 Scenario: ECS Task Failures

**Situation**
- Multiple ECS tasks failing
- Auto-scaling unable to replace tasks
- Application unhealthy

**Detection**
- CloudWatch alarm: task crash rate high
- Health check: repeated failures
- ALB target: marked unhealthy

**Response Timeline**

**Immediate (T+0)**
1. Alert triggered in Slack
2. Infrastructure Lead investigates
3. Check CloudWatch logs: error messages
4. Check ECS cluster: task status

**Within 5 minutes (T+5)**
1. Identify root cause: deployment issue vs. resource issue
2. Decision: rollback vs. scale resources
3. Begin recovery action

**Recovery Steps**

**Option 1: Rollback Deployment**
1. Access ECS console
2. View task definition history
3. Select previous working version
4. Update service: use previous task definition
5. Verify: new tasks launching
6. Monitor: error rate decreasing

**Option 2: Scale Cluster**
1. Check capacity: EC2 instances available
2. Increase task count: manual adjustment
3. Update: desired count increased
4. Verify: new tasks launching
5. Monitor: recovery proceeding

**Option 3: Deploy Hotfix**
1. Developer creates hotfix
2. Build: new Docker image
3. Push: to ECR
4. Update: ECS task definition
5. Deploy: rolling deployment
6. Monitor: health improving

### 3.2 Scenario: API Performance Degradation

**Situation**
- High latency responses
- Timeout errors increasing
- User experience impacted

**Detection**
- CloudWatch: response latency >5 seconds
- ALB: HTTP 504 errors
- Application logs: timeout exceptions

**Response Steps**

1. **Verify Issue**
   - Check dashboard: see current latency
   - Check logs: identify bottleneck (DB vs. cache vs. external API)
   - Check metrics: CPU, memory, network

2. **Identify Root Cause**
   - Database slow queries: check Aurora dashboard
   - Cache issues: check Redis metrics
   - External API: check third-party status
   - Resource constraints: check CPU/memory

3. **Apply Solution**
   - Database: kill long-running queries, scale instance
   - Cache: clear cache if corrupted, scale cluster
   - External API: implement retry logic, fallback
   - Resources: auto-scaling should trigger

4. **Monitor Recovery**
   - Latency: should decrease over 5-10 minutes
   - Error rate: should return to normal
   - User experience: metrics should improve

## 4. Data Corruption Recovery

### 4.1 Scenario: Accidental Data Deletion

**Situation**
- User reports missing data
- Bulk deletion occurred
- Database integrity questioned

**Detection**
- User report: "My data is gone"
- Database anomaly: unexpected row count decrease
- Audit log: suspicious DELETE statements

**Response Timeline**

**Immediate (T+0)**
1. Declare incident
2. Assess: scope of deletion (how many records, which tables)
3. Determine: point-in-time (when data was deleted)
4. Estimate: customer impact

**Within 30 minutes (T+30)**
1. Access backup snapshots
2. Identify latest good snapshot (before deletion)
3. Prepare: staging database for restore
4. Begin: point-in-time recovery

**Recovery Steps**

1. **Locate Backup**
   - View RDS snapshots in console
   - Identify last good backup (e.g., before 10 AM when deletion occurred)
   - Verify: timestamp and backup type

2. **Restore to Staging**
   ```
   AWS RDS > Snapshots > Select snapshot > Restore
   Instance identifier: staging-restore-prod-<date>
   Wait for restore: 15-30 minutes
   ```

3. **Validate Data**
   - Connect to staging database
   - Query affected tables: verify data present
   - Compare: row counts vs. production (before deletion)
   - Checksum: verify data integrity

4. **Extract Missing Data**
   - Write query: SELECT from staging where updated_at < deletion_time
   - Export: data to CSV or SQL
   - Transform: if needed for re-import
   - Backup: save recovery data

5. **Restore Data**
   - Option A (preferred): INSERT from staging to production
   - Option B: Restore entire database (more downtime)
   - Option C: Restore and test, customer applies updates manually

6. **Verify Recovery**
   - Application testing: verify functionality
   - User verification: customer confirms data restored
   - Monitoring: watch for errors
   - Documentation: recovery completed

## 5. Infrastructure Failure Recovery

### 5.1 Scenario: Database Server Hardware Failure

**Situation**
- Physical RDS host failure
- Multi-AZ failover to standby
- Potential downtime during failover

**Detection**
- AWS notification: hardware failure alert
- CloudWatch: database unreachable
- Application: connection errors

**Response**

1. **Automatic Recovery**
   - RDS automatically promotes standby
   - DNS updated (no connection string change needed)
   - Standby: new copy launched
   - Expected duration: 1-2 minutes

2. **Monitor**
   - CloudWatch: verify promotion complete
   - Application health: verify connections
   - Error logs: check for reconnection issues
   - Customer impact: minimal with Multi-AZ

3. **Post-Recovery**
   - Monitor: 1-hour window
   - AWS: will launch new standby (automatic)
   - Infrastructure: coordinate RDS patching if needed

### 5.2 Scenario: ECS Cluster Failure (AZ Down)

**Situation**
- Availability Zone down (AWS maintenance or outage)
- ECS cluster in AZ becomes unavailable
- Application still running in other AZs

**Detection**
- AWS alert: AZ maintenance/outage
- CloudWatch: tasks in AZ unhealthy
- Health checks: failing

**Response**

1. **Automatic Response**
   - Auto-scaling: launches replacement tasks in other AZs
   - ALB: removes unhealthy targets
   - Traffic: routes to healthy AZs
   - Expected duration: 1-2 minutes

2. **Manual Verification**
   - Check ECS cluster: see task distribution
   - Verify: tasks in multiple AZs
   - Health checks: all healthy
   - Load balancer: verify routing

3. **Post-Recovery**
   - Monitor: verify stability
   - AWS: waits for AZ to recover
   - Auto-scaling: adjusts as needed
   - Capacity: ensure sufficient for normal operations

## 6. Network and Connectivity Failures

### 6.1 Scenario: BGP Route Leak or ISP Outage

**Situation**
- Internet connectivity lost or degraded
- Unable to reach external APIs
- Customer connectivity may be affected

**Detection**
- CloudWatch: external API timeouts
- Monitoring tool: Internet connectivity test failed
- Customer report: access issues

**Response**

1. **Verify Issue**
   - Test external connectivity: curl example.com
   - Check ISP status: AWS status page
   - Check DNS: nslookup lutervyn.com
   - Check firewall: outbound rules

2. **Failover if Applicable**
   - Route 53: health checks detect outage
   - Failover: routes to secondary region (if configured)
   - Monitor: traffic flowing through backup

3. **Wait for Recovery**
   - ISP/AWS: recovers connectivity
   - Monitor: verify stability
   - Test: external connections work

4. **Communication**
   - Customers: notify of connectivity issue
   - Status page: update status
   - Resolution: communicate when restored

## 7. Security Incident Response

### 7.1 Scenario: Suspected Data Breach

**Situation**
- Unauthorized access detected
- Potential data exfiltration
- Compliance implications

**Detection**
- Security alert: unusual access pattern
- Audit log: suspicious activity
- Third-party: notification of breach

**Response**

**Immediate**
1. Incident commander: declare security incident
2. Security team: activate incident response
3. Isolate: suspected compromised systems
4. Preserve: evidence and logs
5. Notify: legal and compliance

**First Hour**
1. Assess: breach scope and impact
2. Identify: affected data/systems
3. Determine: customer notification timing
4. Begin: forensic investigation

**Communication**
1. Internal: leadership notification
2. Legal: attorney consultation
3. Customers: per legal advice (typically 72 hours)
4. Regulators: if required by law
5. Media: if public disclosure needed

**Recovery**
1. Remediation: patch vulnerabilities
2. Password reset: affected users
3. Monitoring: enhanced monitoring
4. Audit: full security audit
5. Communication: keep stakeholders updated

## 8. General Incident Response Procedures

### 8.1 Incident Lifecycle

**1. Detection**
- Alert triggered
- Team notified
- Incident severity assessed

**2. Response**
- Incident commander: assumes leadership
- Incident channel: opened in Slack
- Team assembled: necessary personnel
- Initial assessment: quick diagnosis

**3. Mitigation**
- Root cause: identified
- Action plan: developed
- Implementation: recovery steps executed
- Progress: monitored continuously

**4. Recovery**
- Service: restored to normal
- Verification: functionality confirmed
- Monitoring: enhanced monitoring
- Stability: verified for 30 minutes

**5. Communication**
- Team: kept informed throughout
- Customers: periodic updates
- Status page: real-time updates
- Leadership: informed

**6. Post-Incident**
- Analysis: root cause analysis performed
- Documentation: incident documented
- Improvements: preventive measures identified
- Team: debriefing and training

### 8.2 Incident Communication

**Templates**

**Initial Alert (T+0)**
"🚨 INCIDENT: [Service] is experiencing [issue]. Status: [investigating/mitigating]. Updates every 15 minutes. #incident"

**Status Update (T+15, T+30, etc.)**
"UPDATE: We've identified [root cause]. Recovery: [action taken]. ETA: [time estimate]. More updates soon."

**Resolution (T+X)**
"RESOLVED: [Service] is fully recovered. Root cause: [cause]. Prevention: [preventive measures]. Incident review Thursday."

### 8.3 Escalation

**Level 1: Partial Impact**
- Response: on-call engineer
- SLA: 30 minutes

**Level 2: Significant Impact**
- Response: + infrastructure lead
- SLA: 15 minutes

**Level 3: Major Impact**
- Response: + CTO + CEO
- SLA: 5 minutes

**Level 4: Critical Impact**
- Response: all-hands + external comms
- SLA: immediate

## 9. Contact Information

**Incident Response**
- Incident Commander: CEO
- Infrastructure Lead: [name], [phone]
- On-Call Engineer: PagerDuty
- Emergency Hotline: [phone]
- Slack Channel: #incident

**Customer Communication**
- Status Page: status.lutervyn.com
- Email: support@lutervyn.com
- Phone: [phone]

---

**Effective Date**: May 12, 2026
**Last Updated**: May 12, 2026
**Review**: Quarterly

*Runbooks should be tested quarterly in DR drills. Keep printed copy in office.*
