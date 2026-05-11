# Deployment Playbook

Effective Date: January 1, 2024
Last Updated: May 12, 2024

## 1. Overview

This playbook documents deployment procedures for Lutervyn services. It covers preparation, execution, monitoring, and rollback procedures.

## 2. Deployment Process

### 2.1 Pre-Deployment Checklist

- [ ] Code reviewed and approved
- [ ] All tests passing
- [ ] Code coverage adequate
- [ ] Security scan completed
- [ ] Dependencies updated
- [ ] Documentation updated
- [ ] Deployment plan reviewed
- [ ] Stakeholders notified
- [ ] Backup created
- [ ] Rollback plan prepared

### 2.2 Deployment Schedule

**Preferred Windows:**
- Tuesday-Thursday
- 10 AM - 2 PM (EST)
- Avoid Friday deployments
- Avoid during peak usage
- Maintenance windows available

### 2.3 Approval Process

1. Pull request created
2. Code review (2+ reviewers)
3. Tests must pass
4. Security review
5. Release notes prepared
6. Approval obtained
7. Ready for deployment

## 3. Deployment Methods

### 3.1 Blue-Green Deployment

1. Deploy to inactive environment (Green)
2. Run smoke tests
3. Route traffic to Green
4. Monitor for issues
5. Keep Blue as fallback
6. Rollback to Blue if needed

### 3.2 Canary Deployment

1. Deploy to small percentage (5%)
2. Monitor metrics
3. Gradually increase (10% > 25% > 50% > 100%)
4. Rollback at any stage if issues
5. Full deployment when confident

### 3.3 Rolling Deployment

1. Deploy to first instance
2. Verify working
3. Move to next instance
4. Continue until all updated
5. Monitor throughout
6. Rollback if needed

## 4. Deployment Execution

### 4.1 Pre-Deployment

```bash
# Build artifact
make build

# Run tests
make test

# Build Docker image
docker build -t lutervyn:v1.2.3 .

# Push to registry
docker push registry.example.com/lutervyn:v1.2.3
```

### 4.2 Deployment Command

```bash
# Deploy to staging
kubectl apply -f deployment-staging.yaml

# Run smoke tests
make smoke-test

# Deploy to production
kubectl apply -f deployment-prod.yaml

# Verify deployment
kubectl rollout status deployment/lutervyn-api
```

### 4.3 Verification

- Health checks passing
- Services responding
- Metrics normal
- Error rates low
- Database operations working
- External integrations functional

## 5. Monitoring During Deployment

### 5.1 Metrics to Watch

- Request latency (p50, p95, p99)
- Error rate
- CPU and memory usage
- Database connections
- Cache hit rate
- Queue depth
- Disk usage

### 5.2 Alert Thresholds

- Error rate > 1%: Warning
- Error rate > 5%: Critical
- Latency > 2x baseline: Warning
- Latency > 5x baseline: Critical
- CPU > 80%: Warning
- CPU > 95%: Critical

### 5.3 Monitoring Tools

- **Metrics**: Prometheus
- **Visualization**: Grafana
- **Logs**: ELK Stack
- **Tracing**: Jaeger
- **Alerts**: PagerDuty

## 6. Rollback Procedures

### 6.1 When to Rollback

- Error rate > 5%
- All instances down
- Data corruption detected
- Security breach detected
- Database unavailable
- Critical service failure

### 6.2 Rollback Process

```bash
# Check rollout history
kubectl rollout history deployment/lutervyn-api

# Rollback to previous version
kubectl rollout undo deployment/lutervyn-api

# Verify rollback
kubectl rollout status deployment/lutervyn-api

# Monitor for stability
# Wait 10-15 minutes
```

### 6.3 Post-Rollback

1. Assess what went wrong
2. Investigate root cause
3. Fix issues
4. Test thoroughly
5. Plan redeployment
6. Communicate with team

## 7. Release Notes

**Template:**

```markdown
# v1.2.3 Release Notes

## New Features
- Feature 1 description
- Feature 2 description

## Bug Fixes
- Bug 1 fix description
- Bug 2 fix description

## Performance Improvements
- Performance 1
- Performance 2

## Breaking Changes
- None

## Migration Guide
Steps for migration if applicable

## Known Issues
Any known issues

## Contributors
Thank you to contributors
```

## 8. Communication Plan

### 8.1 Before Deployment
- Email to team
- Slack notification
- Status page update
- Customer communication
- Stakeholder briefing

### 8.2 During Deployment
- Real-time status updates
- Issues communicated promptly
- Status page updated
- Slack channel active

### 8.3 After Deployment
- Success announcement
- Metrics shared
- Thank you to team
- Retrospective scheduled

## 9. Database Migrations

### 9.1 Pre-Deployment

1. Write migration script
2. Test on staging data
3. Backup production database
4. Review for backward compatibility
5. Plan rollback procedure

### 9.2 Deployment

```bash
# Run migration
docker exec lutervyn_db migrations run

# Verify success
docker exec lutervyn_db migrations status

# Check data integrity
select count(*) from table_name;
```

### 9.2 Rollback

```bash
# Rollback migration
docker exec lutervyn_db migrations rollback

# Verify rollback
docker exec lutervyn_db migrations status
```

## 10. Configuration Updates

### 10.1 Environment Variables

```bash
# Update in Kubernetes
kubectl set env deployment/lutervyn-api \
  FEATURE_FLAG_NEW_FEATURE=true

# Verify update
kubectl get deployment lutervyn-api -o yaml
```

### 10.2 Secrets Management

```bash
# Update secret
kubectl create secret generic lutervyn-secrets \
  --from-literal=api-key=$NEW_KEY \
  --dry-run -o yaml | kubectl apply -f -

# Redeploy with new secrets
kubectl rollout restart deployment/lutervyn-api
```

## 11. Scaling

### 11.1 During Deployment

- Don't scale down to zero
- Maintain rolling updates
- Monitor resource usage
- Auto-scaling enabled

### 11.2 Post-Deployment

- Verify scaling works
- Monitor resource usage
- Adjust auto-scaling if needed
- Test failover

## 12. Compliance and Auditing

### 12.1 Deployment Audit

- Log all deployments
- Record who deployed
- Document when
- Capture changes
- Version tracking

### 12.2 Deployment Logs

```bash
# View deployment history
kubectl rollout history deployment/lutervyn-api

# View specific revision
kubectl rollout history deployment/lutervyn-api --revision=3

# View pod events
kubectl describe pods deployment-lutervyn-api-xyz
```

## 13. Disaster Recovery

### 13.1 Backup Before Deployment

```bash
# Backup persistent data
kubectl get pvc
kubectl exec pod-name -- tar -czf /tmp/backup.tar.gz /data

# Backup database
mysql-dump --all-databases > backup.sql
```

### 13.2 Recovery Procedures

```bash
# Restore from backup
kubectl exec pod-name -- tar -xzf /tmp/backup.tar.gz

# Restore database
mysql < backup.sql
```

## 14. Troubleshooting

### 14.1 Deployment Stuck

```bash
# Check pod status
kubectl get pods

# View pod logs
kubectl logs deployment/lutervyn-api

# Describe deployment
kubectl describe deployment lutervyn-api
```

### 14.2 Slow Rollout

- Check resource availability
- Check pod pull time
- Check readiness probes
- Review startup scripts

### 14.3 Failed Deployment

1. Check logs immediately
2. Assess impact
3. Decide to rollback or fix
4. Execute decision
5. Verify stability
6. Investigate root cause

## 15. Emergency Procedures

### 15.1 Critical Issue Detected

1. **Immediate**: Stop routing traffic
2. **Quick**: Assess scope and impact
3. **Decision**: Rollback or fix forward
4. **Execute**: Implement decision quickly
5. **Communicate**: Update all stakeholders
6. **Monitor**: Verify stability

### 15.2 Hotfix Process

```bash
# Create hotfix branch
git checkout -b hotfix/critical-issue

# Make fix
# Test thoroughly

# Merge and deploy
git push origin hotfix/critical-issue
# Create PR for urgent review
# Deploy immediately upon approval
```

## 16. Post-Deployment Tasks

- [ ] Send success notification
- [ ] Update status page
- [ ] Close deployment ticket
- [ ] Schedule retrospective
- [ ] Update documentation
- [ ] Thank the team
- [ ] Log metrics
- [ ] Archive logs

## 17. Resources

- **Deployment Docs**: https://docs.lutervyn.pages.dev/deployment
- **Runbooks**: https://docs.lutervyn.pages.dev/runbooks
- **Status Page**: https://status.lutervyn.pages.dev
- **On-Call**: PagerDuty
- **Communication**: #deployments Slack channel
