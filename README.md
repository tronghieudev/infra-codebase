# Infrastructure Codebase

> AWS Cloud Infrastructure as Code using CDK - Production-Ready Architecture

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [AWS Services](#aws-services)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Deployment](#deployment)
- [Backup & Restore](#backup--restore)
- [Rollback Procedures](#rollback-procedures)
- [Security](#security)
- [Load Testing](#load-testing)
- [Monitoring & Alerts](#monitoring--alerts)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

---

## 🎯 Overview

This repository contains AWS Cloud Development Kit (CDK) infrastructure code for deploying scalable, secure, and highly available applications on AWS. The infrastructure supports containerized applications using ECS Fargate, with comprehensive backup, monitoring, and disaster recovery capabilities.

### Key Features

✅ **Infrastructure as Code** - Complete infrastructure defined in CDK (TypeScript/Python/Java/C#)
✅ **Multi-Environment** - Support for dev, staging, and production environments
✅ **Auto-Scaling** - Automatic scaling for compute and database resources
✅ **High Availability** - Multi-AZ deployments with automatic failover
✅ **Security First** - WAF, encryption at rest/transit, secrets management
✅ **Backup & Recovery** - Automated backups with point-in-time recovery
✅ **Monitoring** - Comprehensive CloudWatch dashboards and alarms
✅ **CI/CD Ready** - CodePipeline integration for automated deployments

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         CloudFront (CDN)                         │
│                    + WAF + Shield Standard                       │
└────────────────┬────────────────────────────┬───────────────────┘
                 │                            │
                 ▼                            ▼
        ┌────────────────┐          ┌─────────────────┐
        │   S3 Bucket    │          │  API Gateway    │
        │  (Static App)  │          │   (Optional)    │
        └────────────────┘          └────────┬────────┘
                                              │
                                              ▼
                              ┌───────────────────────────┐
                              │ Application Load Balancer │
                              │    + SSL/TLS (ACM)        │
                              └──────────┬────────────────┘
                                         │
                    ┌────────────────────┼────────────────────┐
                    ▼                    ▼                    ▼
            ┌───────────────┐    ┌───────────────┐   ┌───────────────┐
            │  ECS Fargate  │    │  ECS Fargate  │   │  ECS Fargate  │
            │   Service 1   │    │   Service 2   │   │   Service 3   │
            │  Auto-Scaling │    │  Auto-Scaling │   │  Auto-Scaling │
            └───────┬───────┘    └───────┬───────┘   └───────┬───────┘
                    │                    │                    │
                    └────────────────────┼────────────────────┘
                                         │
                    ┌────────────────────┼────────────────────┐
                    ▼                    ▼                    ▼
            ┌───────────────┐    ┌──────────────┐    ┌──────────────┐
            │  RDS Aurora   │    │ ElastiCache  │    │  DynamoDB    │
            │   Multi-AZ    │    │    Redis     │    │  (Optional)  │
            │ Read Replicas │    │   Cluster    │    └──────────────┘
            └───────────────┘    └──────────────┘
                    │
                    ▼
            ┌───────────────┐
            │  AWS Backup   │
            │   (Automated) │
            └───────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                    Supporting Services                           │
├─────────────────────────────────────────────────────────────────┤
│ Route 53 | CloudWatch | X-Ray | Secrets Manager | KMS | SNS     │
│ CloudTrail | GuardDuty | Config | Systems Manager | EventBridge │
└─────────────────────────────────────────────────────────────────┘
```

---

## ☁️ AWS Services

### 1. Compute Services
- **ECS Fargate** - Serverless container orchestration
- **ECS Fargate Spot** - Cost optimization for non-critical workloads
- **ECS Service Auto Scaling** - Dynamic scaling based on metrics
- **AWS Batch** - Batch processing workloads
- **Lambda** (Optional) - Event-driven serverless functions

### 2. Networking
- **VPC** - Isolated network with public/private/database subnets
- **Internet Gateway** - Internet connectivity for public subnets
- **NAT Gateway** - Outbound internet for private subnets
- **Application Load Balancer** - Layer 7 load balancing
- **Route 53** - DNS management and health checks
- **CloudFront** - Global CDN with edge caching
- **API Gateway** (Optional) - REST/WebSocket APIs
- **Cloud Map** - Service discovery for microservices

### 3. Storage
- **S3** - Object storage for static assets, backups, logs
- **EFS** - Shared file system for containers
- **ECR** - Private Docker image registry

### 4. Database
- **RDS Aurora** - MySQL/PostgreSQL with Multi-AZ
- **ElastiCache Redis** - In-memory caching and sessions
- **DynamoDB** (Optional) - NoSQL database
- **DocumentDB** (Optional) - MongoDB-compatible database

### 5. Security & Identity
- **IAM** - Identity and access management
- **Secrets Manager** - Secure credential storage with rotation
- **Systems Manager Parameter Store** - Configuration management
- **KMS** - Encryption key management
- **Certificate Manager** - SSL/TLS certificate management
- **WAF** - Web application firewall
- **Shield Standard** - DDoS protection
- **GuardDuty** (Optional) - Threat detection

### 6. Monitoring & Logging
- **CloudWatch** - Logs, metrics, alarms, dashboards
- **X-Ray** - Distributed tracing and performance analysis
- **CloudTrail** - API audit logging
- **AWS Config** (Optional) - Configuration compliance

### 7. CI/CD
- **CodePipeline** - Automated deployment pipelines
- **CodeBuild** - Build and test automation
- **CodeDeploy** - Blue-Green deployments
- **ECR** - Container image storage

### 8. Integration & Messaging
- **SNS** - Notifications and pub/sub messaging
- **SQS** - Message queuing for decoupling
- **EventBridge** - Event-driven automation

### 9. Management & Governance
- **CloudFormation** - CDK-generated infrastructure templates
- **AWS Backup** - Centralized backup management
- **Cost Explorer** - Cost analysis and optimization
- **Systems Manager** - Operational management

---

## 📋 Prerequisites

### Required Tools

1. **AWS CLI** (v2.x or later)
   ```bash
   # Install AWS CLI
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   unzip awscliv2.zip
   sudo ./aws/install

   # Configure credentials
   aws configure
   ```

2. **Node.js** (v18.x or later) - for CDK CLI
   ```bash
   # Install Node.js
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```

3. **AWS CDK CLI**
   ```bash
   npm install -g aws-cdk
   cdk --version
   ```

4. **Language-Specific Requirements**

   **TypeScript/JavaScript:**
   ```bash
   npm install
   ```

   **Python:**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

   **Java:**
   ```bash
   mvn package
   ```

   **C#:**
   ```bash
   dotnet restore
   dotnet build
   ```

5. **Docker** (for building container images)
   ```bash
   sudo apt-get update
   sudo apt-get install docker.io
   sudo usermod -aG docker $USER
   ```

### AWS Account Setup

1. **Bootstrap CDK** (one-time per account/region)
   ```bash
   cdk bootstrap aws://ACCOUNT-ID/REGION
   ```

2. **Required IAM Permissions**
   - AdministratorAccess (for initial setup)
   - Or custom policy with permissions for all services listed above

3. **Service Quotas**
   - Verify VPC limits (default: 5 per region)
   - ECS task limits
   - RDS instance limits
   - Load balancer limits

---

## 📁 Project Structure

```
infra-codebase/
├── README.md                      # This file
├── CLAUDE.md                      # AI assistant guide
├── cdk.json                       # CDK configuration
├── package.json                   # Node dependencies (TypeScript)
├── requirements.txt               # Python dependencies
│
├── bin/                           # CDK app entry points
│   └── app.ts                     # Main CDK app
│
├── lib/                           # CDK stack definitions
│   ├── stacks/                    # Infrastructure stacks
│   │   ├── network-stack.ts       # VPC, subnets, NAT, IGW
│   │   ├── security-stack.ts      # WAF, security groups, IAM roles
│   │   ├── database-stack.ts      # RDS, ElastiCache, DynamoDB
│   │   ├── compute-stack.ts       # ECS clusters, services, tasks
│   │   ├── storage-stack.ts       # S3, EFS, ECR
│   │   ├── cdn-stack.ts           # CloudFront, Route53
│   │   ├── monitoring-stack.ts    # CloudWatch, X-Ray, alarms
│   │   ├── cicd-stack.ts          # CodePipeline, CodeBuild
│   │   └── backup-stack.ts        # AWS Backup, disaster recovery
│   │
│   ├── constructs/                # Reusable CDK constructs
│   │   ├── ecs-service.ts         # Custom ECS service construct
│   │   ├── rds-cluster.ts         # RDS Aurora construct
│   │   ├── cache-cluster.ts       # ElastiCache construct
│   │   └── monitoring.ts          # CloudWatch dashboard construct
│   │
│   └── config/                    # Environment configurations
│       ├── dev.ts                 # Development environment
│       ├── staging.ts             # Staging environment
│       └── production.ts          # Production environment
│
├── test/                          # Unit and integration tests
│   ├── unit/                      # CDK construct tests
│   └── integration/               # Infrastructure tests
│
├── scripts/                       # Utility scripts
│   ├── deploy.sh                  # Deployment script
│   ├── backup.sh                  # Manual backup script
│   ├── restore.sh                 # Restore from backup
│   ├── rollback.sh                # Rollback deployment
│   └── load-test.sh               # Load testing script
│
├── docker/                        # Dockerfiles for applications
│   ├── api/
│   ├── frontend/
│   └── worker/
│
├── docs/                          # Documentation
│   ├── architecture.md            # Architecture decisions
│   ├── runbooks/                  # Operational runbooks
│   │   ├── deployment.md
│   │   ├── backup-restore.md
│   │   ├── incident-response.md
│   │   └── rollback.md
│   └── diagrams/                  # Architecture diagrams
│
└── .github/                       # GitHub Actions (optional)
    └── workflows/
        └── deploy.yml
```

---

## 🚀 Getting Started

### 1. Clone Repository

```bash
git clone https://github.com/tronghieudev/infra-codebase.git
cd infra-codebase
```

### 2. Install Dependencies

**TypeScript:**
```bash
npm install
```

**Python:**
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 3. Configure Environment

Create environment-specific configuration files:

```typescript
// lib/config/dev.ts
export const devConfig = {
  env: {
    account: process.env.CDK_DEFAULT_ACCOUNT,
    region: 'us-east-1'
  },
  vpcCidr: '10.0.0.0/16',
  ecsCluster: {
    containerInsights: true,
    capacityProviders: ['FARGATE', 'FARGATE_SPOT']
  },
  rds: {
    instanceType: 't3.medium',
    multiAz: false,
    backupRetention: 7
  },
  elasticache: {
    nodeType: 'cache.t3.micro',
    numNodes: 1
  }
};
```

### 4. Synthesize CloudFormation

```bash
# Generate CloudFormation templates
cdk synth

# View differences from deployed stack
cdk diff
```

### 5. Deploy Infrastructure

```bash
# Deploy all stacks
cdk deploy --all

# Deploy specific stack
cdk deploy NetworkStack

# Deploy with approval bypass (use cautiously)
cdk deploy --all --require-approval never
```

---

## 🚢 Deployment

### Deployment Strategy

This infrastructure supports multiple deployment strategies:

1. **Blue-Green Deployment** (Recommended for production)
2. **Rolling Updates** (Default for ECS services)
3. **Canary Deployments** (Using CodeDeploy)

### Environment-Specific Deployments

**Development:**
```bash
export ENV=dev
cdk deploy --all --context environment=dev
```

**Staging:**
```bash
export ENV=staging
cdk deploy --all --context environment=staging
```

**Production:**
```bash
export ENV=production
cdk deploy --all --context environment=production --require-approval broadening
```

### Deployment Checklist

Before deploying to production:

- [ ] Review `cdk diff` output
- [ ] Run security scanning (`cdk-nag` or `cfn_nag`)
- [ ] Verify backup retention policies
- [ ] Check CloudWatch alarms configuration
- [ ] Review IAM permissions
- [ ] Test in staging environment
- [ ] Schedule maintenance window (if needed)
- [ ] Notify team members
- [ ] Prepare rollback plan

### CI/CD Pipeline Deployment

The infrastructure includes CodePipeline for automated deployments:

1. **Commit** code to GitHub/CodeCommit
2. **Build** - CodeBuild runs tests and builds Docker images
3. **Deploy to Staging** - Automatic deployment to staging
4. **Manual Approval** - Require approval for production
5. **Deploy to Production** - Blue-Green deployment with automatic rollback

**Pipeline Configuration:**
```typescript
// lib/stacks/cicd-stack.ts
const pipeline = new CodePipeline(this, 'Pipeline', {
  pipelineName: 'InfraPipeline',
  crossAccountKeys: true,
  synth: new ShellStep('Synth', {
    input: CodePipelineSource.gitHub('tronghieudev/infra-codebase', 'main'),
    commands: [
      'npm ci',
      'npm run build',
      'npx cdk synth'
    ]
  })
});

// Add stages
pipeline.addStage(new DevStage(this, 'Dev'));
pipeline.addStage(new StagingStage(this, 'Staging'));
pipeline.addStage(new ProductionStage(this, 'Production'), {
  pre: [new ManualApprovalStep('PromoteToProd')]
});
```

---

## 💾 Backup & Restore

### Automated Backup Strategy

**AWS Backup Configuration:**

```typescript
// lib/stacks/backup-stack.ts
const backupPlan = new BackupPlan(this, 'BackupPlan', {
  backupPlanName: 'ProductionBackupPlan',
  backupPlanRules: [
    // Daily backups retained for 30 days
    new BackupPlanRule({
      ruleName: 'DailyBackup',
      scheduleExpression: Schedule.cron({ hour: '2', minute: '0' }),
      deleteAfter: Duration.days(30),
      moveToColdStorageAfter: Duration.days(7)
    }),
    // Weekly backups retained for 90 days
    new BackupPlanRule({
      ruleName: 'WeeklyBackup',
      scheduleExpression: Schedule.cron({ weekDay: 'SUN', hour: '3', minute: '0' }),
      deleteAfter: Duration.days(90),
      moveToColdStorageAfter: Duration.days(30)
    }),
    // Monthly backups retained for 1 year
    new BackupPlanRule({
      ruleName: 'MonthlyBackup',
      scheduleExpression: Schedule.cron({ day: '1', hour: '4', minute: '0' }),
      deleteAfter: Duration.days(365),
      moveToColdStorageAfter: Duration.days(60)
    })
  ]
});
```

### Backup Coverage

| Service | Backup Method | Retention | RPO | RTO |
|---------|---------------|-----------|-----|-----|
| RDS Aurora | Automated snapshots + AWS Backup | 30 days | 5 min | 15 min |
| DynamoDB | Point-in-time recovery | 35 days | 1 sec | 5 min |
| ElastiCache | Daily snapshots | 7 days | 24 hrs | 30 min |
| EFS | AWS Backup | 30 days | 24 hrs | 15 min |
| S3 | Versioning + Cross-region replication | Unlimited | Real-time | 1 min |

### Manual Backup

**RDS Snapshot:**
```bash
#!/bin/bash
# scripts/backup.sh

ENVIRONMENT=$1
TIMESTAMP=$(date +%Y%m%d-%H%M%S)

# Create RDS snapshot
aws rds create-db-cluster-snapshot \
  --db-cluster-snapshot-identifier "manual-backup-${ENVIRONMENT}-${TIMESTAMP}" \
  --db-cluster-identifier "${ENVIRONMENT}-aurora-cluster" \
  --tags Key=Type,Value=Manual Key=Environment,Value=${ENVIRONMENT}

echo "Backup created: manual-backup-${ENVIRONMENT}-${TIMESTAMP}"
```

**S3 Backup:**
```bash
# Sync critical S3 data to backup bucket
aws s3 sync s3://prod-app-bucket s3://prod-backup-bucket/$(date +%Y%m%d)/ \
  --storage-class GLACIER_IR
```

### Restore Procedures

**Restore RDS from Snapshot:**

```bash
#!/bin/bash
# scripts/restore.sh

SNAPSHOT_ID=$1
TARGET_ENV=$2

# Restore cluster from snapshot
aws rds restore-db-cluster-from-snapshot \
  --db-cluster-identifier "${TARGET_ENV}-aurora-cluster-restored" \
  --snapshot-identifier "${SNAPSHOT_ID}" \
  --engine aurora-mysql \
  --engine-version 8.0.mysql_aurora.3.04.0 \
  --vpc-security-group-ids sg-xxxxx \
  --db-subnet-group-name "${TARGET_ENV}-db-subnet-group"

# Wait for cluster to be available
aws rds wait db-cluster-available \
  --db-cluster-identifier "${TARGET_ENV}-aurora-cluster-restored"

echo "Restore completed. Update application endpoints."
```

**Restore DynamoDB Table:**
```bash
# Point-in-time recovery
aws dynamodb restore-table-to-point-in-time \
  --source-table-name prod-table \
  --target-table-name prod-table-restored \
  --restore-date-time 2024-01-15T10:00:00Z
```

### Disaster Recovery Plan

**RTO/RPO Targets:**
- **Production RTO**: 15 minutes
- **Production RPO**: 5 minutes
- **Staging RTO**: 1 hour
- **Staging RPO**: 24 hours

**DR Runbook:** See [docs/runbooks/disaster-recovery.md](docs/runbooks/disaster-recovery.md)

---

## 🔄 Rollback Procedures

### CDK Stack Rollback

**Automatic Rollback:**
CDK automatically rolls back failed deployments. Monitor:

```bash
# Watch stack events
aws cloudformation describe-stack-events \
  --stack-name ProductionStack \
  --max-items 20
```

**Manual Rollback to Previous Version:**

```bash
#!/bin/bash
# scripts/rollback.sh

STACK_NAME=$1
PREVIOUS_VERSION=$2

# Option 1: Rollback to previous CDK version
git checkout ${PREVIOUS_VERSION}
cdk deploy ${STACK_NAME}

# Option 2: Use CloudFormation to continue update rollback
aws cloudformation continue-update-rollback \
  --stack-name ${STACK_NAME}
```

### ECS Service Rollback

**Using AWS Console:**
1. Go to ECS Console → Services
2. Select the service → Update
3. Choose previous task definition revision
4. Update service

**Using CLI:**
```bash
# List task definition revisions
aws ecs list-task-definitions --family-prefix api-service

# Rollback to previous revision
aws ecs update-service \
  --cluster production-cluster \
  --service api-service \
  --task-definition api-service:42 \
  --force-new-deployment
```

### Database Rollback

**Schema Changes:**
```bash
# Use database migration tool (e.g., Flyway, Liquibase)
flyway undo -target=version_42

# Or restore from backup (if data loss acceptable)
./scripts/restore.sh snapshot-id production
```

### Application Version Rollback

**Using CodeDeploy:**
```bash
# Stop current deployment
aws deploy stop-deployment --deployment-id d-XXXXX

# Rollback to previous version
aws deploy create-deployment \
  --application-name MyApp \
  --deployment-group-name production \
  --s3-location bucket=deployments,key=app-v1.2.3.zip,bundleType=zip
```

### Rollback Decision Matrix

| Severity | Impact | Action | Timeline |
|----------|--------|--------|----------|
| P0 - Critical | Production down | Immediate rollback | < 5 min |
| P1 - High | Degraded performance | Rollback after quick fix attempt | < 15 min |
| P2 - Medium | Feature broken | Fix forward or rollback | < 1 hour |
| P3 - Low | Minor issue | Fix forward | Next release |

---

## 🔐 Security

### Security Layers

**1. Network Security**

```typescript
// Multi-layer network security
const vpc = new Vpc(this, 'VPC', {
  maxAzs: 3,
  natGateways: 3,
  subnetConfiguration: [
    {
      name: 'Public',
      subnetType: SubnetType.PUBLIC,
      cidrMask: 24
    },
    {
      name: 'Private',
      subnetType: SubnetType.PRIVATE_WITH_EGRESS,
      cidrMask: 24
    },
    {
      name: 'Database',
      subnetType: SubnetType.PRIVATE_ISOLATED,
      cidrMask: 24
    }
  ]
});

// Security Groups with least privilege
const albSecurityGroup = new SecurityGroup(this, 'ALBSecurityGroup', {
  vpc,
  description: 'Security group for ALB',
  allowAllOutbound: false
});

albSecurityGroup.addIngressRule(
  Peer.anyIpv4(),
  Port.tcp(443),
  'Allow HTTPS from internet'
);
```

**2. Application Security - WAF Rules**

```typescript
// WAF with managed rules
const webAcl = new CfnWebACL(this, 'WebACL', {
  scope: 'CLOUDFRONT',
  defaultAction: { allow: {} },
  rules: [
    // AWS Managed Rules
    {
      name: 'AWSManagedRulesCommonRuleSet',
      priority: 1,
      statement: {
        managedRuleGroupStatement: {
          vendorName: 'AWS',
          name: 'AWSManagedRulesCommonRuleSet'
        }
      },
      overrideAction: { none: {} },
      visibilityConfig: {
        sampledRequestsEnabled: true,
        cloudWatchMetricsEnabled: true,
        metricName: 'CommonRuleSetMetric'
      }
    },
    // SQL Injection protection
    {
      name: 'SQLInjectionProtection',
      priority: 2,
      statement: {
        managedRuleGroupStatement: {
          vendorName: 'AWS',
          name: 'AWSManagedRulesSQLiRuleSet'
        }
      },
      overrideAction: { none: {} },
      visibilityConfig: {
        sampledRequestsEnabled: true,
        cloudWatchMetricsEnabled: true,
        metricName: 'SQLInjectionMetric'
      }
    },
    // Rate limiting
    {
      name: 'RateLimitRule',
      priority: 3,
      statement: {
        rateBasedStatement: {
          limit: 2000,
          aggregateKeyType: 'IP'
        }
      },
      action: { block: {} },
      visibilityConfig: {
        sampledRequestsEnabled: true,
        cloudWatchMetricsEnabled: true,
        metricName: 'RateLimitMetric'
      }
    }
  ],
  visibilityConfig: {
    sampledRequestsEnabled: true,
    cloudWatchMetricsEnabled: true,
    metricName: 'WebACLMetric'
  }
});
```

**3. Data Encryption**

```typescript
// Encryption at rest
const database = new DatabaseCluster(this, 'Database', {
  engine: DatabaseClusterEngine.auroraMysql({
    version: AuroraMysqlEngineVersion.VER_3_04_0
  }),
  storageEncrypted: true,
  storageEncryptionKey: new Key(this, 'DBEncryptionKey', {
    enableKeyRotation: true,
    description: 'KMS key for RDS encryption'
  })
});

// S3 encryption
const bucket = new Bucket(this, 'AppBucket', {
  encryption: BucketEncryption.KMS,
  encryptionKey: new Key(this, 'S3EncryptionKey', {
    enableKeyRotation: true
  }),
  enforceSSL: true,
  versioned: true
});
```

**4. Secrets Management**

```typescript
// Store database credentials in Secrets Manager
const dbSecret = new Secret(this, 'DBSecret', {
  secretName: `${environment}/rds/credentials`,
  generateSecretString: {
    secretStringTemplate: JSON.stringify({ username: 'admin' }),
    generateStringKey: 'password',
    excludePunctuation: true,
    passwordLength: 32
  }
});

// Automatic rotation
dbSecret.addRotationSchedule('RotationSchedule', {
  automaticallyAfter: Duration.days(30),
  hostedRotation: HostedRotation.mysqlSingleUser()
});
```

**5. IAM Least Privilege**

```typescript
// ECS Task role with minimal permissions
const taskRole = new Role(this, 'ECSTaskRole', {
  assumedBy: new ServicePrincipal('ecs-tasks.amazonaws.com'),
  description: 'Role for ECS tasks'
});

// Grant specific permissions
bucket.grantRead(taskRole);
dbSecret.grantRead(taskRole);

// Deny permissions explicitly
taskRole.addToPolicy(new PolicyStatement({
  effect: Effect.DENY,
  actions: ['s3:DeleteBucket', 's3:DeleteObject'],
  resources: ['*']
}));
```

### Security Compliance Checklist

- [ ] All data encrypted at rest (KMS)
- [ ] All data encrypted in transit (TLS 1.2+)
- [ ] Secrets stored in Secrets Manager
- [ ] IAM roles follow least privilege
- [ ] Security groups are restrictive
- [ ] CloudTrail enabled for audit logs
- [ ] GuardDuty enabled for threat detection
- [ ] AWS Config rules for compliance
- [ ] VPC Flow Logs enabled
- [ ] WAF rules active on ALB/CloudFront
- [ ] Regular security scanning (AWS Inspector)
- [ ] Automated secret rotation
- [ ] Multi-factor authentication enabled
- [ ] Regular access reviews

### Security Scanning

**Pre-deployment Security Scan:**
```bash
# CDK Nag - security checks for CDK
npm install -g cdk-nag
cdk synth --app "npx ts-node --prefer-ts-exts bin/app.ts" --plugin cdk-nag

# cfn_nag - CloudFormation template security
gem install cfn-nag
cfn_nag_scan --input-path cdk.out/

# Checkov - infrastructure security
pip install checkov
checkov -d cdk.out/
```

---

## ⚡ Load Testing

### Load Testing Strategy

**Objectives:**
- Identify performance bottlenecks
- Validate auto-scaling configuration
- Determine system capacity limits
- Ensure SLA compliance under load

### Tools

1. **Apache JMeter** - Web application load testing
2. **Locust** - Python-based load testing
3. **Artillery** - Modern load testing toolkit
4. **AWS Distributed Load Testing** - CloudFormation solution

### Load Testing Setup

**Using Locust:**

```python
# load-tests/locustfile.py
from locust import HttpUser, task, between

class WebsiteUser(HttpUser):
    wait_time = between(1, 5)

    @task(3)
    def view_homepage(self):
        self.client.get("/")

    @task(2)
    def view_api(self):
        self.client.get("/api/v1/items")

    @task(1)
    def create_item(self):
        self.client.post("/api/v1/items", json={
            "name": "Test Item",
            "description": "Load test"
        })

# Run load test
# locust -f load-tests/locustfile.py --host=https://api.example.com
```

**Using Artillery:**

```yaml
# load-tests/artillery-config.yml
config:
  target: 'https://api.example.com'
  phases:
    # Warm-up phase
    - duration: 60
      arrivalRate: 10
      name: "Warm-up"

    # Ramp-up phase
    - duration: 300
      arrivalRate: 10
      rampTo: 100
      name: "Ramp-up"

    # Sustained load
    - duration: 600
      arrivalRate: 100
      name: "Sustained load"

    # Spike test
    - duration: 60
      arrivalRate: 200
      name: "Spike"

scenarios:
  - name: "User flow"
    flow:
      - get:
          url: "/"
      - get:
          url: "/api/v1/items"
      - post:
          url: "/api/v1/items"
          json:
            name: "Test"
            description: "Load test"
      - think: 3

# Run: artillery run load-tests/artillery-config.yml
```

### Load Testing Script

```bash
#!/bin/bash
# scripts/load-test.sh

ENVIRONMENT=$1
DURATION=${2:-300}  # Default 5 minutes
VUS=${3:-100}       # Default 100 virtual users

case $ENVIRONMENT in
  dev)
    TARGET="https://dev-api.example.com"
    ;;
  staging)
    TARGET="https://staging-api.example.com"
    ;;
  production)
    echo "WARNING: Load testing production requires approval!"
    read -p "Are you sure? (yes/no): " confirm
    if [ "$confirm" != "yes" ]; then
      exit 1
    fi
    TARGET="https://api.example.com"
    ;;
  *)
    echo "Invalid environment: $ENVIRONMENT"
    exit 1
    ;;
esac

echo "Starting load test:"
echo "  Target: $TARGET"
echo "  Duration: ${DURATION}s"
echo "  Virtual Users: $VUS"

# Run load test with Locust
locust -f load-tests/locustfile.py \
  --host=$TARGET \
  --users=$VUS \
  --spawn-rate=10 \
  --run-time=${DURATION}s \
  --headless \
  --html=reports/load-test-$(date +%Y%m%d-%H%M%S).html

echo "Load test completed. Check reports/ directory for results."
```

### Performance Benchmarks

**Target Metrics:**

| Metric | Target | Acceptable | Action Needed |
|--------|--------|------------|---------------|
| Response Time (p50) | < 200ms | < 500ms | > 500ms |
| Response Time (p95) | < 500ms | < 1000ms | > 1000ms |
| Response Time (p99) | < 1000ms | < 2000ms | > 2000ms |
| Error Rate | < 0.1% | < 1% | > 1% |
| Throughput | > 1000 req/s | > 500 req/s | < 500 req/s |
| CPU Utilization | < 70% | < 85% | > 85% |
| Memory Utilization | < 80% | < 90% | > 90% |

### Auto-Scaling Validation

Monitor during load test:

```bash
# Watch ECS service scaling
watch -n 5 'aws ecs describe-services \
  --cluster production-cluster \
  --services api-service \
  --query "services[0].{desired:desiredCount,running:runningCount}" \
  --output table'

# Watch RDS CPU
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name CPUUtilization \
  --dimensions Name=DBClusterIdentifier,Value=production-aurora-cluster \
  --start-time $(date -u -d '10 minutes ago' +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 60 \
  --statistics Average
```

### Load Test Reports

Generate comprehensive reports:

```bash
# Generate JMeter dashboard
jmeter -g results.jtl -o dashboard/

# Generate Artillery report
artillery report load-test-results.json --output report.html

# Custom analysis
python scripts/analyze-load-test.py reports/latest.html
```

---

## 📊 Monitoring & Alerts

### CloudWatch Dashboards

```typescript
// lib/stacks/monitoring-stack.ts
const dashboard = new Dashboard(this, 'MainDashboard', {
  dashboardName: `${environment}-dashboard`
});

// ECS Metrics
dashboard.addWidgets(
  new GraphWidget({
    title: 'ECS CPU & Memory',
    left: [ecsService.metricCpuUtilization()],
    right: [ecsService.metricMemoryUtilization()]
  }),
  new GraphWidget({
    title: 'ECS Task Count',
    left: [
      ecsService.metric('RunningTaskCount'),
      ecsService.metric('DesiredTaskCount')
    ]
  })
);

// ALB Metrics
dashboard.addWidgets(
  new GraphWidget({
    title: 'ALB Request Count',
    left: [alb.metricRequestCount()],
    right: [alb.metricTargetResponseTime()]
  }),
  new GraphWidget({
    title: 'ALB Errors',
    left: [
      alb.metricHttpCodeTarget(HttpCodeTarget.TARGET_4XX_COUNT),
      alb.metricHttpCodeTarget(HttpCodeTarget.TARGET_5XX_COUNT)
    ]
  })
);

// RDS Metrics
dashboard.addWidgets(
  new GraphWidget({
    title: 'RDS CPU & Connections',
    left: [database.metricCPUUtilization()],
    right: [database.metricDatabaseConnections()]
  })
);
```

### CloudWatch Alarms

```typescript
// Critical alarms
const highCpuAlarm = new Alarm(this, 'HighCPUAlarm', {
  metric: ecsService.metricCpuUtilization(),
  threshold: 80,
  evaluationPeriods: 2,
  datapointsToAlarm: 2,
  comparisonOperator: ComparisonOperator.GREATER_THAN_THRESHOLD,
  treatMissingData: TreatMissingData.NOT_BREACHING
});

const high5xxAlarm = new Alarm(this, 'High5xxAlarm', {
  metric: alb.metricHttpCodeTarget(HttpCodeTarget.TARGET_5XX_COUNT),
  threshold: 10,
  evaluationPeriods: 1,
  comparisonOperator: ComparisonOperator.GREATER_THAN_THRESHOLD
});

// SNS notifications
const alertTopic = new Topic(this, 'AlertTopic', {
  displayName: 'Production Alerts'
});

alertTopic.addSubscription(new EmailSubscription('team@example.com'));
alertTopic.addSubscription(new SmsSubscription('+1234567890'));

// Connect alarms to SNS
highCpuAlarm.addAlarmAction(new SnsAction(alertTopic));
high5xxAlarm.addAlarmAction(new SnsAction(alertTopic));
```

### Log Aggregation

```typescript
// Centralized logging
const logGroup = new LogGroup(this, 'ApplicationLogs', {
  logGroupName: `/aws/ecs/${environment}/${serviceName}`,
  retention: RetentionDays.ONE_MONTH,
  removalPolicy: RemovalPolicy.DESTROY
});

// Log insights queries
new CfnQueryDefinition(this, 'ErrorQuery', {
  name: 'Find Errors',
  queryString: `
    fields @timestamp, @message
    | filter @message like /ERROR/
    | sort @timestamp desc
    | limit 100
  `
});
```

### Distributed Tracing (X-Ray)

```typescript
// Enable X-Ray on ECS
const taskDefinition = new FargateTaskDefinition(this, 'TaskDef', {
  cpu: 512,
  memoryLimitMiB: 1024
});

taskDefinition.addContainer('xray-daemon', {
  image: ContainerImage.fromRegistry('amazon/aws-xray-daemon'),
  cpu: 32,
  memoryLimitMiB: 256,
  portMappings: [{
    containerPort: 2000,
    protocol: Protocol.UDP
  }]
});
```

### Alert Severity Levels

| Level | Response Time | Notification | Example |
|-------|---------------|--------------|---------|
| P0 - Critical | Immediate | Phone + SMS + Email | Production down |
| P1 - High | < 15 min | SMS + Email | High error rate |
| P2 - Medium | < 1 hour | Email | Degraded performance |
| P3 - Low | Next business day | Email | Non-critical warning |

---

## 🐛 Troubleshooting

### Common Issues

**Issue 1: CDK Deployment Fails**

```bash
# Check CloudFormation events
aws cloudformation describe-stack-events \
  --stack-name YourStackName \
  --max-items 10

# Check CDK version compatibility
cdk --version
npm list aws-cdk-lib

# Clear CDK context
rm cdk.context.json
cdk synth
```

**Issue 2: ECS Tasks Not Starting**

```bash
# Check task status
aws ecs describe-tasks \
  --cluster production-cluster \
  --tasks task-id

# Check CloudWatch logs
aws logs tail /aws/ecs/production/api-service --follow

# Common causes:
# - Insufficient memory/CPU
# - Missing secrets/environment variables
# - IAM permission issues
# - ECR image pull failures
```

**Issue 3: Database Connection Issues**

```bash
# Test connectivity from ECS task
aws ecs execute-command \
  --cluster production-cluster \
  --task task-id \
  --container api-container \
  --interactive \
  --command "/bin/bash"

# Inside container:
nc -zv database-endpoint 3306
nslookup database-endpoint

# Check security groups
aws ec2 describe-security-groups --group-ids sg-xxxxx
```

**Issue 4: High Latency**

```bash
# Check ALB target health
aws elbv2 describe-target-health \
  --target-group-arn arn:aws:elasticloadbalancing:...

# Check RDS performance insights
# AWS Console → RDS → Performance Insights

# Check ElastiCache metrics
aws cloudwatch get-metric-statistics \
  --namespace AWS/ElastiCache \
  --metric-name EngineCPUUtilization \
  --dimensions Name=CacheClusterId,Value=redis-cluster \
  --start-time 2024-01-01T00:00:00Z \
  --end-time 2024-01-01T23:59:59Z \
  --period 300 \
  --statistics Average
```

### Debug Mode

Enable verbose logging:

```bash
# CDK debug mode
cdk deploy --verbose

# AWS CLI debug
aws s3 ls --debug

# Terraform debug (if using)
TF_LOG=DEBUG terraform apply
```

### Getting Help

1. **Check Documentation**: [AWS Documentation](https://docs.aws.amazon.com/)
2. **CloudWatch Logs**: Review application and service logs
3. **AWS Support**: Open support ticket for AWS-specific issues
4. **Community**: AWS Forums, Stack Overflow
5. **Internal**: Check runbooks in `docs/runbooks/`

---

## 🤝 Contributing

### Development Workflow

1. **Create Feature Branch**
   ```bash
   git checkout -b feature/description
   ```

2. **Make Changes**
   - Follow CDK best practices
   - Add unit tests
   - Update documentation

3. **Test Locally**
   ```bash
   npm run test
   cdk synth
   cdk diff
   ```

4. **Commit**
   ```bash
   git add .
   git commit -m "feat: description of changes"
   ```

5. **Push and Create PR**
   ```bash
   git push -u origin feature/description
   ```

### Code Review Guidelines

- All changes require code review
- Run security scans before submitting PR
- Include `cdk diff` output in PR description
- Document any breaking changes
- Update CLAUDE.md if architecture changes

### Coding Standards

**TypeScript:**
- Use ESLint and Prettier
- Follow AWS CDK best practices
- Use constructs for reusable patterns

**Python:**
- Follow PEP 8
- Use type hints
- Document with docstrings

---

## 📚 Additional Resources

### Documentation
- [AWS CDK Documentation](https://docs.aws.amazon.com/cdk/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [ECS Best Practices](https://docs.aws.amazon.com/AmazonECS/latest/bestpracticesguide/)
- [RDS Best Practices](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_BestPractices.html)

### Internal Docs
- [Architecture Decisions](docs/architecture.md)
- [Runbooks](docs/runbooks/)
- [Deployment Guide](docs/runbooks/deployment.md)
- [Incident Response](docs/runbooks/incident-response.md)

### Tools
- [AWS CLI](https://aws.amazon.com/cli/)
- [AWS CDK Toolkit](https://docs.aws.amazon.com/cdk/latest/guide/cli.html)
- [Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)

---

## 📝 License

This infrastructure code is proprietary and confidential.

---

## 👥 Support

For questions or issues:
- **Email**: team@example.com
- **Slack**: #infrastructure
- **On-call**: PagerDuty rotation

---

**Last Updated**: 2025-11-18
**Maintained By**: Infrastructure Team
**Version**: 1.0.0
