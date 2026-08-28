# Deployment Architecture

```mermaid
graph TB
    subgraph CI_CD[🚀 CI/CD Pipeline]
        Source[📂 Source Control<br/>GitHub<br/>Trunk-based Dev<br/>Branch Protection<br/>Signed Commits]
        Build[🔨 Build & Test<br/>Docker Multi-stage<br/>Unit/Integration Tests<br/>Security Scanning<br/>Dependency Check<br/>Image Signing]
        Staging[🎭 Staging Environment<br/>Full Replica<br/>E2E Tests<br/>Load Testing<br/>Chaos Engineering<br/>Feature Flags]
        Deploy[🚀 Progressive Deploy<br/>ArgoCD / Flux<br/>Canary: 10% → 50% → 100%<br/>Auto Rollback<br/>Health Verification<br/>Smoke Tests]
    end

    subgraph Cloud[☁️ Cloud Provider (AWS/GCP/Azure)]
        Region1[🌍 Primary Region<br/>(e.g., Mumbai ap-south-1)]
        Region2[🌍 Secondary Region<br/>(e.g., Singapore ap-southeast-1<br/>Disaster Recovery)]
    end

    subgraph Edge[🌐 Edge Layer]
        CDN[📦 CDN (CloudFront/CloudFlare)<br/>Static Assets • Media Delivery<br/>DDoS Protection • WAF<br/>Edge Caching • Geo-routing]
        DNS[🔍 DNS (Route53/Cloud DNS)<br/>Health Checks • Failover<br/>Latency-based Routing]
    end

    subgraph Network[🔒 Network Layer]
        VPC[🏗️ VPC (10.0.0.0/16)<br/>Multi-AZ • Private/Public Subnets<br/>NAT Gateways • VPC Endpoints]
        
        subgraph PublicSN[Public Subnets (per AZ)]
            ALB[⚖️ ALB<br/>WAF • SSL Termination<br/>Health Checks • Access Logs]
            NAT[🌐 NAT Gateway<br/>Outbound Internet • HA]
            Bastion[🏰 Bastion Host<br/>SSH/Session Manager<br/>Audit Logging]
        end
        
        subgraph PrivateSN[Private Subnets (per AZ)]
            AppSN[📱 App Subnet<br/>EKS/ECS/GKE Nodes<br/>Auto-scaling Groups]
            DataSN[💾 Data Subnet<br/>RDS/ElastiCache<br/>No Internet Access]
            MgmtSN[⚙️ Mgmt Subnet<br/>Monitoring • Logging • Backup]
        end
        
        TGW[🔗 Transit Gateway<br/>Inter-region • VPN/Direct Connect]
    end

    subgraph Compute[⚙️ Compute Layer]
        K8s[☸️ Kubernetes (EKS/ECS/GKE)<br/>Multi-AZ Node Groups<br/>Cluster Autoscaler<br/>Pod Disruption Budgets]
        
        subgraph Services[Microservices (Containers)]
            GW[🚪 API Gateway<br/>Kong/Envoy • 3+ replicas<br/>HPA: CPU>70%]
            Auth[🔐 Auth Service<br/>3+ replicas • Redis Sessions]
            User[👤 User/Profile Service<br/>3+ replicas • Read Replicas]
            Discovery[🔍 Discovery Service<br/>3+ replicas • Search Sync]
            Feed[📰 Feed Service<br/>3+ replicas • Fan-out Workers]
            Msg[💬 Messaging Service<br/>3+ replicas • WebSocket]
            Meetup[☕ Meetup Service<br/>3+ replicas]
            Community[👥 Community Service<br/>3+ replicas]
            Event[📅 Event Service<br/>3+ replicas]
            Notif[🔔 Notification Service<br/>3+ replicas • Worker Pools]
            Safety[🛡️ Safety Service<br/>3+ replicas]
            Location[📍 Location Service<br/>3+ replicas • Geospatial]
            Mod[⚖️ Moderation Service<br/>2+ replicas • Admin Only]
        end
        
        Workers[👷 Background Workers<br/>Notifications • Media • Search<br/>Analytics ETL • Cleanup<br/>Spot Instances OK]
    end

    subgraph Data[💾 Data Layer]
        PG[(PostgreSQL 15+<br/>Multi-AZ (Primary+Standby)<br/>db.r6g.xlarge • 500GB gp3<br/>Backups: 30d • PITR<br/>Read Replicas: 2<br/>Encryption at Rest)]
        Redis[(Redis 7 Cluster<br/>Multi-AZ • cache.r6g.large<br/>Shards: 3 • Replicas: 1<br/>TTL Policies • Encryption)]
        Search[(OpenSearch<br/>Multi-AZ • 3 Data + 3 Master<br/>Warm/Cold Storage<br/>Index Lifecycle)]
        S3[(S3/GCS<br/>Buckets: avatars, posts, chat, events<br/>Versioning • Lifecycle: IA 30d<br/>CloudFront Origin • SSE-S3)]
        Analytics[(Redshift/BigQuery<br/>Columnar • Partitioned by Date<br/>Daily Incremental Loads)]
    end

    subgraph Messaging[📨 Messaging & Events]
        Kafka[🚌 Kafka (MSK)/EventBridge<br/>Topics per Domain<br/>Retention: 7d<br/>Compacted for State<br/>Schema Registry]
        SQS[📋 SQS/RabbitMQ<br/>DLQ • Priority Queues<br/>Delay Queues • Visibility Timeout]
        WS[🔌 WebSocket Manager<br/>API Gateway WS/AppSync<br/>Connection Pooling<br/>Presence • Fan-out]
    end

    subgraph Observability[📊 Observability]
        Metrics[📈 Prometheus/Grafana<br/>Infra • App • Business Metrics<br/>SLI/SLO Dashboards • Alerting]
        Logs[📝 ELK/CloudWatch<br/>Structured JSON • Centralized<br/>90d Retention • PII Redaction]
        Traces[🔍 OpenTelemetry/X-Ray<br/>Service Map • Latency Analysis<br/>Error Tracking • 10% Sampling]
        Alerts[🚨 PagerDuty/OpsGenie<br/>Tiered Escalation • On-call<br/>Runbook Links • Auto-resolve]
        Health[❤️ Health Checks<br/>Liveness/Readiness • Deps<br/>Synthetic Monitoring • Status Page]
    end

    subgraph Security[🔐 Security]
        Secrets[🔑 Secrets Manager<br/>DB Passwords • API Keys<br/>JWT Secrets • Rotation • Audit]
        Certs[📜 Cert Manager (ACM)<br/>Auto-renewal • Wildcard • CT Logs]
        IAM[👤 IAM/RBAC<br/>Least Privilege • Service Accounts<br/>Role Assumptions • Boundaries]
        NetPol[🛡️ Network Policies<br/>Pod-to-Pod Encryption • Egress<br/>Service Mesh (Istio) • mTLS]
        Compliance[📋 Compliance<br/>SOC 2 Type II • GDPR Ready<br/>Data Residency • Audit Trails]
    end

    Source --> Build
    Build --> Staging
    Staging --> Deploy
    Deploy --> K8s
    
    DNS --> CDN
    CDN --> ALB
    ALB --> VPC
    ALB --> GW
    
    VPC --> PublicSN & PrivateSN & TGW
    TGW --> Region2
    
    K8s --> Services & Workers
    
    Services --> PG & Redis & Search & S3 & Kafka & SQS & WS
    Workers --> Kafka & SQS & PG & Redis & S3 & Analytics
    
    PG -.->|Replication| Region2
    Redis -.->|Replication| Region2
    S3 -.->|Cross-region Replication| Region2
    
    Services --> Observability
    Workers --> Observability
    Data --> Observability
    Messaging --> Observability
    
    Security -.->|Applies to All| Cloud
```

## Numbered Deployment Flow

### CI/CD Pipeline
1. **Source Control** — GitHub, trunk-based development, branch protection, signed commits
2. **Build & Test** — Docker multi-stage, unit/integration tests, security scanning (SAST/DAST), dependency check, image signing
3. **Staging Environment** — Full production replica, E2E tests, load testing, chaos engineering, feature flags
4. **Progressive Deploy** — ArgoCD/Flux GitOps, canary 10% → 50% → 100%, automated rollback on health check failure, smoke tests

### Cloud Infrastructure (Multi-region)
5. **Primary Region** — Mumbai (ap-south-1) for India launch
6. **Secondary Region** — Singapore (ap-southeast-1) for disaster recovery

### Edge Layer
7. **CDN** — CloudFront/CloudFlare: static assets, media delivery, DDoS protection, WAF, edge caching, geo-routing
8. **DNS** — Route53/Cloud DNS: health checks, failover routing, latency-based routing

### Network Layer
9. **VPC** — 10.0.0.0/16, multi-AZ, private/public subnets, NAT gateways, VPC endpoints
10. **Public Subnets** (per AZ) — ALB (WAF, SSL termination, health checks), NAT Gateway (HA), Bastion Host (SSH/Session Manager)
11. **Private Subnets** (per AZ) — App Subnet (K8s nodes, auto-scaling), Data Subnet (RDS/ElastiCache, no internet), Mgmt Subnet (monitoring/logging/backup)
12. **Transit Gateway** — Inter-region connectivity, VPN/Direct Connect

### Compute Layer
13. **Kubernetes** — EKS/ECS/GKE, multi-AZ node groups, cluster autoscaler, pod disruption budgets
14. **Microservices** (14 services, containerized):
    - API Gateway (Kong/Envoy, 3+ replicas, HPA CPU>70%)
    - Auth Service (3+ replicas, Redis sessions)
    - User/Profile Service (3+ replicas, read replicas)
    - Discovery Service (3+ replicas, search index sync)
    - Feed Service (3+ replicas, fan-out workers)
    - Messaging Service (3+ replicas, WebSocket support)
    - Meetup Service (3+ replicas)
    - Community Service (3+ replicas)
    - Event Service (3+ replicas)
    - Notification Service (3+ replicas, worker pools)
    - Safety Service (3+ replicas)
    - Location Service (3+ replicas, geospatial index)
    - Moderation Service (2+ replicas, admin only)
15. **Background Workers** — Notifications, media processing, search indexing, analytics ETL, cleanup (spot instances OK)

### Data Layer
16. **PostgreSQL** — 15+, Multi-AZ (primary+standby), db.r6g.xlarge, 500GB gp3, 30d backups, PITR, 2 read replicas, encryption at rest
17. **Redis Cluster** — 7, Multi-AZ, cache.r6g.large, 3 shards, 1 replica, TTL policies, encryption in transit
18. **OpenSearch** — Multi-AZ, 3 data + 3 master nodes, warm/cold storage, index lifecycle
19. **S3/GCS** — Buckets: avatars, posts, chat, events; versioning, lifecycle (IA after 30d), CloudFront origin, SSE-S3
20. **Analytics Warehouse** — Redshift/BigQuery, columnar, partitioned by date, daily incremental loads

### Messaging & Events
21. **Kafka/EventBridge** — Topics per domain, 7d retention, compacted topics for state, schema registry
22. **Task Queue** — SQS/RabbitMQ, DLQ, priority queues, delay queues, visibility timeout
23. **WebSocket Manager** — API Gateway WS/AppSync, connection pooling, presence tracking, message fan-out

### Observability
24. **Metrics** — Prometheus/Grafana: infra, app, business metrics, SLI/SLO dashboards, alerting
25. **Logs** — ELK/CloudWatch: structured JSON, centralized, 90d retention, PII redaction
26. **Traces** — OpenTelemetry/X-Ray: service map, latency analysis, error tracking, 10% sampling
27. **Alerting** — PagerDuty/OpsGenie: tiered escalation, on-call rotations, runbook links, auto-resolution
28. **Health Checks** — Liveness/readiness, dependency checks, synthetic monitoring, status page

### Security
29. **Secrets Manager** — DB passwords, API keys, JWT secrets, rotation policies, audit access
30. **Certificate Manager** — ACM, auto-renewal, wildcard certs, CT logging
31. **IAM/RBAC** — Least privilege, service accounts, role assumptions, permission boundaries
32. **Network Policies** — Pod-to-pod encryption, egress controls, service mesh (Istio/Linkerd), mTLS
33. **Compliance** — SOC 2 Type II, GDPR ready, data residency (India), audit trails, pen testing