# Reference: Cloud Service Map

_Last reviewed: 2026-07-02 · Review cadence: quarterly_

AWS ↔ Azure ↔ GCP equivalents by category. Use it to translate when you move between accounts/clouds — the *concept* is the same, only the brand name changes.

> Names map roughly, not exactly — services differ in detail. This is for **"what's the equivalent of X?"**, not a feature-by-feature spec.

## Compute

| Concept | AWS | Azure | GCP |
|---------|-----|-------|-----|
| Virtual machines | EC2 | Virtual Machines | Compute Engine |
| Containers (managed) | ECS / Fargate | Container Apps / Container Instances | Cloud Run |
| Kubernetes | EKS | AKS | GKE |
| Serverless functions | Lambda | Functions | Cloud Functions |
| PaaS web hosting | Elastic Beanstalk | App Service | App Engine |

## Storage & databases

| Concept | AWS | Azure | GCP |
|---------|-----|-------|-----|
| Object storage | S3 | Blob Storage | Cloud Storage |
| Block storage | EBS | Managed Disks | Persistent Disk |
| File storage | EFS | Azure Files | Filestore |
| Relational (managed) | RDS / Aurora | Azure SQL / PostgreSQL Flexible Server | Cloud SQL / AlloyDB |
| NoSQL (document/key-value) | DynamoDB | Cosmos DB | Firestore / Bigtable |
| In-memory cache | ElastiCache | Cache for Redis | Memorystore |
| Data warehouse | Redshift | Synapse Analytics | BigQuery |

## Networking & delivery

| Concept | AWS | Azure | GCP |
|---------|-----|-------|-----|
| DNS | Route 53 | Azure DNS | Cloud DNS |
| CDN | CloudFront | Front Door / CDN | Cloud CDN |
| Load balancer | ELB / ALB / NLB | Load Balancer / App Gateway | Cloud Load Balancing |
| Virtual network | VPC | Virtual Network (VNet) | VPC |
| Web app firewall | WAF | WAF (Front Door / App Gateway) | Cloud Armor |
| API gateway | API Gateway | API Management | API Gateway / Apigee |

## Messaging & integration

| Concept | AWS | Azure | GCP |
|---------|-----|-------|-----|
| Queue | SQS | Service Bus Queues / Storage Queues | Pub/Sub |
| Pub/sub events | SNS / EventBridge | Event Grid | Pub/Sub |
| Streaming | Kinesis / MSK (Kafka) | Event Hubs | Pub/Sub / Dataflow |
| Workflow orchestration | Step Functions | Logic Apps / Durable Functions | Workflows |

## Data & analytics

| Concept | AWS | Azure | GCP |
|---------|-----|-------|-----|
| ETL / data integration | Glue | Data Factory (ADF) | Dataflow / Data Fusion |
| Big-data processing (Spark) | EMR | Synapse / Databricks | Dataproc / Databricks |
| Streaming analytics | Managed Service for Apache Flink (was Kinesis Data Analytics) | Stream Analytics | Dataflow |
| BI | QuickSight | Power BI | Looker |
| Catalog / governance | Glue Data Catalog / Lake Formation | Purview | Dataplex / Data Catalog |

## Identity, security & secrets

| Concept | AWS | Azure | GCP |
|---------|-----|-------|-----|
| Identity & access mgmt | IAM | Entra ID (Azure AD) + RBAC | Cloud IAM |
| Workload identity | IAM Roles | Managed Identity | Service Accounts / Workload Identity |
| Secrets | Secrets Manager / Parameter Store | Key Vault | Secret Manager |
| Key management | KMS | Key Vault | Cloud KMS |
| Threat detection | GuardDuty | Defender for Cloud | Security Command Center |

## Observability & ops

| Concept | AWS | Azure | GCP |
|---------|-----|-------|-----|
| Metrics/logs | CloudWatch | Monitor / Log Analytics | Cloud Monitoring / Logging |
| Tracing | X-Ray | Application Insights | Cloud Trace |
| Infrastructure as code (native) | CloudFormation / CDK | Bicep / ARM | Deployment Manager |
| Cross-cloud IaC | **Terraform** | **Terraform** | **Terraform** |

## ML & AI

| Concept | AWS | Azure | GCP |
|---------|-----|-------|-----|
| ML platform | SageMaker | Azure Machine Learning | Vertex AI |
| Managed LLM access | Bedrock | Azure OpenAI / AI Foundry | Vertex AI (Gemini) |
| Vector search | OpenSearch / Aurora pgvector | AI Search | Vertex AI Vector Search |

> **For the Azure→AWS bridge specifically:** ADF→Glue/Step Functions, Synapse→Redshift, Blob→S3, Azure SQL→RDS/Aurora, Functions→Lambda, App Service→Beanstalk/ECS, Key Vault→Secrets Manager, Entra ID→IAM, App Insights→CloudWatch+X-Ray, Azure OpenAI→Bedrock. **Terraform spans all three**, which is why it's the pragmatic default for IaC in any multi-cloud or cloud-portable estate — one toolchain and skill set instead of three provider-native ones.

[← Back to index](../README.md)
