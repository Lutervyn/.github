# Tech Stack Documentation

Last Updated: May 12, 2026

## 1. Technology Stack Overview

### 1.1 Stack Philosophy

Lutervyn's technology stack is built on modern, scalable, and maintainable technologies that enable rapid development while ensuring reliability and security.

### 1.2 Stack Principles

- **Best-of-breed**: choose best tool for each task
- **Open source**: leverage community when possible
- **Proven**: tested technologies with strong communities
- **Scalable**: support growth without major rewrites
- **Maintainable**: clear code, good documentation
- **Vendor-neutral**: avoid lock-in when possible

## 2. Backend Services

### 2.1 Primary Application

**Technology: Python 3.11 + FastAPI**
- Framework: FastAPI (modern, fast, ASGI web framework)
- Python version: 3.11.x (latest LTS)
- Async: full async/await support
- Performance: 50,000+ requests/second capable

**Justification**
- Rapid development: Django ecosystem maturity
- Type safety: Python type hints reduce bugs
- Performance: async-first architecture
- Community: large community and third-party packages

**Core Dependencies**
- Web: FastAPI, Pydantic, Starlette
- Database: SQLAlchemy, Alembic
- Async: asyncio, aiohttp
- Testing: pytest, pytest-asyncio
- Development: Black, Flake8, MyPy

### 2.2 Microservices

**Service: API Gateway**
- Technology: AWS API Gateway + Lambda
- Purpose: request routing, rate limiting, authentication
- Scalability: AWS managed, auto-scaling

**Service: Background Jobs**
- Technology: Celery + Redis
- Tasks: async job processing
- Queues: multiple queues by priority
- Scheduling: scheduled jobs via Celery Beat

**Service: File Processing**
- Technology: Python workers + S3
- Purpose: document processing, image resizing
- Scale: horizontal scaling with SQS

## 3. Frontend

### 3.1 Web Application

**Technology: React 18.2 + TypeScript**
- Framework: React (component-based UI)
- Language: TypeScript (type safety)
- Build: Vite (fast build tool)
- Node: 18.16.x LTS

**Justification**
- React: largest community, extensive ecosystem
- TypeScript: catches bugs, improves developer experience
- Vite: faster development cycles than Webpack
- Performance: optimized code splitting

**Core Dependencies**
- State: Redux Toolkit, React Query
- UI: React Router, Material UI
- Forms: React Hook Form, Formik
- Charts: Recharts, D3.js
- Testing: Vitest, React Testing Library

### 3.2 Mobile (Future)

**Technology: React Native / Expo**
- Platform: iOS and Android
- Framework: React Native for code sharing
- Tool: Expo for rapid development
- Status: planned for phase 2

## 4. Database

### 4.1 Primary Database

**PostgreSQL 14.7**
- Type: Relational SQL database
- Version: 14.7 (LTS release)
- Purpose: all application data storage
- Scalability: vertical scaling (larger instances)

**Key Features**
- ACID compliance: transactional integrity
- Indexing: sophisticated index strategies
- Replication: streaming replication (Multi-AZ)
- Extensions: PostGIS, JSONb, etc.

**Schema**
- Version control: Alembic migrations
- Backward compatible: migrations test both up/down
- Deployment: automated during CI/CD

### 4.2 Caching

**Redis 7.0**
- Type: In-memory data store
- Purpose: session storage, caching, real-time analytics
- Mode: cluster mode for high availability
- Persistence: RDB snapshots + AOF

**Use Cases**
- Session storage: user sessions
- API caching: cache frequently accessed data
- Rate limiting: token bucket algorithm
- Real-time: pub/sub for notifications
- Job queues: Celery task queues

### 4.3 Data Warehouse

**Amazon Redshift**
- Type: Columnar data warehouse
- Purpose: analytics, reporting, OLAP
- Scale: 10 nodes (1TB) initially
- Querying: SQL compatible interface

**Integration**
- Source: daily snapshots from PostgreSQL
- ETL: Airflow DAG pipeline
- Access: BI tools via JDBC/ODBC

## 5. Message Queues and Async

### 5.1 Job Queues

**Technology: Celery + Redis**
- Broker: Redis (message broker)
- Workers: Python workers processing tasks
- Queue types: default, high-priority, low-priority
- Monitoring: Flower (Celery monitoring UI)

**Use Cases**
- Email: send emails asynchronously
- Notifications: push/SMS notifications
- Reports: generate reports in background
- Analytics: track analytics events

### 5.2 Pub/Sub

**Technology: Redis Pub/Sub**
- Purpose: real-time messaging
- Use: WebSocket notifications, live updates
- Scale: single Redis instance

**Alternative: AWS SNS/SQS**
- For larger scale or decoupling

## 6. Search and Analytics

### 6.1 Full-Text Search

**Technology: Elasticsearch 8.0**
- Purpose: full-text search over documents
- Scale: 3-node cluster
- Indexing: daily refresh of indices
- Query: Kibana for visualization

**Integration**
- Source: PostgreSQL documents
- Indexing: Logstash pipeline
- Query: direct Elasticsearch API

### 6.2 Event Analytics

**Technology: Analytics.js + Segment**
- Purpose: user behavior tracking
- Collection: JavaScript SDK
- Destination: multiple analytics tools
- Privacy: GDPR-compliant tracking

**Integrations**
- Google Analytics: conversion tracking
- Amplitude: cohort analysis
- Mixpanel: funnel analysis
- Custom: data warehouse

## 7. Authentication & Authorization

### 7.1 Authentication

**Technology: OAuth 2.0 + JWT**
- Protocol: OAuth 2.0 flows
- Token: JWT (JSON Web Tokens)
- Expiry: 1 hour access, 30-day refresh
- Signing: RS256 with RSA keys

**Providers**
- Google: OAuth via Google
- GitHub: OAuth via GitHub
- Email/Password: local authentication
- Multi-factor: TOTP/Authy for MFA

### 7.2 Authorization

**Technology: Role-Based Access Control (RBAC)**
- Roles: Admin, User, Viewer
- Permissions: fine-grained per feature
- Implementation: Casbin policy engine

**API Security**
- Rate limiting: 1000 req/minute per user
- API keys: long-lived for integrations
- CORS: restricted origins
- HTTPS: TLS 1.2+ only

## 8. Infrastructure

### 8.1 Container Orchestration

**AWS ECS (Elastic Container Service)**
- Orchestration: AWS ECS Fargate
- Containers: Docker containers
- Task definition: describes container(s)
- Auto-scaling: CPU/memory based

**Deployment**
- CI/CD: GitHub Actions
- Registry: AWS ECR (Elastic Container Registry)
- Versioning: semver tags (v1.2.3)
- Rollback: previous task definition

### 8.2 Infrastructure as Code

**Technology: Terraform**
- State: remote state in S3
- Version: 1.x
- Modules: reusable infrastructure components
- Linting: TFLint validation

**Coverage**
- Network: VPC, subnets, security groups
- Compute: ECS clusters, ALB
- Database: RDS, ElastiCache
- Storage: S3 buckets
- Monitoring: CloudWatch dashboards

### 8.3 CI/CD Pipeline

**GitHub Actions**
- Trigger: on push to main branch
- Build: Docker image build
- Test: unit tests, integration tests
- Scan: security scanning
- Push: push to ECR
- Deploy: automatic to staging, manual to production

**Pipeline Steps**
1. Lint: code style check (2 min)
2. Test: pytest suite (5 min)
3. Build: Docker image build (3 min)
4. Scan: vulnerability scan (1 min)
5. Push: to ECR (1 min)
6. Deploy: to ECS (5 min)
Total: ~15 minutes

## 9. Monitoring and Observability

### 9.1 Logging

**Technology: CloudWatch Logs**
- Aggregation: centralized log aggregation
- Retention: 30 days default (configurable)
- Query: CloudWatch Insights SQL queries
- Export: export to S3 for analysis

**Log Format**
- JSON structured logging
- Fields: timestamp, level, message, context
- Sampling: 1% for debug level, 100% for error

### 9.2 Monitoring

**Technology: CloudWatch Metrics + Alarms**
- Metrics: CPU, memory, network, business metrics
- Collection: 1-minute granularity
- Dashboards: operational dashboards
- Alarms: threshold-based alerts

**Key Metrics**
- Application: request rate, latency, error rate
- Database: connections, queries, replication lag
- Infrastructure: CPU, memory, disk space

### 9.3 Tracing

**Technology: AWS X-Ray**
- Tracing: distributed request tracing
- Service map: visual architecture
- Performance: latency analysis
- Sampling: 10% of requests traced

## 10. Development Tools

### 10.1 Version Control

**Technology: Git + GitHub**
- Repository: GitHub organization
- Workflow: Git Flow (main, develop branches)
- Branch protection: PR review required
- History: maintained indefinitely

### 10.2 Local Development

**Technology: Docker Compose**
- Services: PostgreSQL, Redis, API, Worker
- Configuration: docker-compose.yml
- Database: pre-seeded with test data
- Setup: one command `docker-compose up`

**IDE: VS Code**
- Extensions: Python, Docker, Git
- Debugger: Python Debugger
- Testing: pytest plugin
- Linting: Pylint, Black

### 10.3 Testing

**Unit Tests**
- Framework: pytest
- Coverage: 80% minimum target
- Mocking: unittest.mock
- CI: fail if coverage <80%

**Integration Tests**
- Framework: pytest with fixtures
- Database: test database
- Fixtures: shared test data
- Coverage: critical paths

**End-to-End Tests**
- Framework: Cypress
- Target: critical user flows
- Frequency: before each release
- Environment: staging environment

## 11. Security Tools

### 11.1 Dependency Scanning

**Technology: Dependabot**
- Source: GitHub Dependabot alerts
- Frequency: daily scans
- Action: automatic PRs for updates
- Review: developer reviews and merges

### 11.2 Code Scanning

**Technology: GitHub Advanced Security**
- Scanner: CodeQL
- Frequency: on every push
- Results: security alerts
- Reports: trends and metrics

### 11.3 Secrets Management

**Technology: AWS Secrets Manager**
- Storage: encrypted secret storage
- Rotation: automatic rotation capable
- Access: IAM-based access control
- Audit: CloudTrail logs

## 12. Documentation

### 12.1 API Documentation

**Technology: OpenAPI / Swagger**
- Format: OpenAPI 3.0 spec
- Generation: FastAPI automatic
- Interactive: Swagger UI at /docs
- Client generation: API clients auto-generated

### 12.2 Code Documentation

**Technology: Docstrings + Sphinx**
- Format: Google-style docstrings
- Generation: Sphinx documentation
- Hosting: Read the Docs
- Version: per release

## 13. Tech Stack Summary

| Layer | Technology | Notes |
|-------|-----------|-------|
| Web Framework | FastAPI + Python 3.11 | Async, modern, fast |
| Frontend | React 18 + TypeScript | Type-safe, component-based |
| Primary DB | PostgreSQL 14 | Relational, ACID, replication |
| Cache | Redis 7 | Session, caching, real-time |
| Job Queue | Celery + Redis | Async task processing |
| Search | Elasticsearch 8 | Full-text search |
| Orchestration | AWS ECS Fargate | Container orchestration |
| Infrastructure | Terraform | Infrastructure as code |
| CI/CD | GitHub Actions | Automated pipeline |
| Monitoring | CloudWatch | Metrics, logs, alarms |
| Tracing | AWS X-Ray | Distributed tracing |
| Testing | pytest + Cypress | Unit, integration, E2E |
| Secrets | AWS Secrets Manager | Encrypted storage |

## 14. Technology Decisions

### 14.1 Why These Technologies?

1. **FastAPI**: Modern, fast, automatic API docs
2. **PostgreSQL**: Mature, reliable, powerful
3. **React**: Largest ecosystem, best community
4. **AWS**: Market leader, most services
5. **Terraform**: Most popular IaC tool
6. **Docker**: Standard containerization

### 14.2 Future Considerations

- **GraphQL**: evaluate for complex queries
- **Kotlin**: evaluate for Android app
- **Kubernetes**: evaluate at higher scale
- **ClickHouse**: evaluate for analytics

## 15. Technology Roadmap

**Q3 2026**
- React Native mobile app development
- GraphQL API layer
- Kafka for event streaming

**Q4 2026**
- Kubernetes migration planning
- Edge computing evaluation
- ML pipeline integration

**2027**
- Full Kubernetes implementation
- ML features in product
- Real-time collaboration

## 16. Questions & Contact

- Tech Architecture: tech@lutervyn.com
- Infrastructure: infrastructure@lutervyn.com
- DevOps: devops@lutervyn.com

---

**Effective Date**: May 12, 2026
**Last Updated**: May 12, 2026
**Review**: Quarterly

*Tech stack decisions require CTO approval. Minor version updates can be auto-approved.*
