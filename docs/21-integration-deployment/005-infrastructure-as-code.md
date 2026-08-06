# ChannelHub Infrastructure as Code

## Purpose

Menetapkan Infrastructure as Code (IaC) untuk deployment production ChannelHub menggunakan Terraform.

---

## AI TRIGGER

### Tujuan Task
Menetapkan IaC strategy dan implementasi untuk infrastructure production ChannelHub.

### Konteks yang Perlu Dipahami AI
- Cloud provider: AWS (primary), GCP (alternative), Azure (alternative)
- Infrastructure as Code: Terraform
- Orchestration: Kubernetes (EKS/GKE/AKS)
- Networking: VPC, subnets, security groups
- Database: RDS PostgreSQL (multi-AZ)
- Cache: ElastiCache Redis (cluster mode)
- Storage: S3 untuk static assets dan backup
- CDN: CloudFront/Cloudflare

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/12-project-foundation/004-docker-infrastructure-design.md (Docker infrastructure)
- docs/21-integration-deployment/002-docker-compose-production.md (Docker compose production)
- .channelhub/OUTPUT-CONFIG.md (output directory structure)

### File/Folder yang Perlu Diperiksa
- docs/21-integration-deployment/004-environment-deployment.md (environment deployment)
- docs/15-database-implementation/008-database-backup-recovery-standard.md (backup recovery)

### Langkah Implementasi
1. Setup Terraform workspace dan state management
2. Define VPC dan networking
3. Define EKS cluster configuration
4. Define RDS PostgreSQL with multi-AZ
5. Define ElastiCache Redis cluster
6. Define S3 buckets dan CloudFront CDN
7. Define IAM roles dan policies
8. Setup monitoring dan logging

### Kriteria Keberhasilan (Definition of Done)
- Terraform configuration terdefinisi untuk seluruh infrastructure
- VPC dan networking terkonfigurasi dengan proper subnet
- EKS cluster terkonfigurasi dengan proper node groups
- RDS dan ElastiCache terkonfigurasi dengan high availability
- CDN dan storage terkonfigurasi
- Monitoring dan logging terintegrasi

### Prompt Implementasi
```
Anda akan mengimplementasikan Infrastructure as Code untuk ChannelHub.

Baca docs/21-integration-deployment/005-infrastructure-as-code.md untuk memahami IaC strategy.

Cloud Provider Choice: AWS (primary), dengan Terraform sebagai IaC tool.

Infrastructure Components:
1. VPC & Networking:
   - VPC dengan CIDR 10.0.0.0/16
   - Public subnets (3 AZ) untuk load balancer
   - Private subnets (3 AZ) untuk EKS nodes, RDS, ElastiCache
   - NAT Gateway untuk outbound internet access
   - Security groups dengan proper ingress/egress rules

2. EKS Cluster:
   - EKS cluster version 1.28+
   - Managed node groups:
     - Backend nodes (m5.large, 3 nodes, auto-scaling 3-10)
     - Frontend nodes (t3.medium, 2 nodes, auto-scaling 2-5)
   - Fargate profiles untuk worker nodes
   - Kubernetes version upgrade strategy

3. RDS PostgreSQL:
   - PostgreSQL 15
   - Multi-AZ deployment
   - Instance class: db.r6g.xlarge (memory optimized)
   - Storage: 500GB GP3, provisioned IOPS
   - Automated backups (retention 30 days)
   - Read replicas (2 for read-heavy workloads)
   - Parameter group dengan optimized settings

4. ElastiCache Redis:
   - Redis 7 cluster mode
   - 3 node clusters (1 primary, 2 replicas)
   - Instance class: cache.r6g.large
   - Multi-AZ deployment
   - Automatic failover
   - Encryption at rest and in transit

5. S3 & CloudFront:
   - S3 buckets:
     - channelhub-static-assets (public)
     - channelhub-uploads (private)
     - channelhub-backups (private)
   - CloudFront CDN:
     - Distribution for static assets
     - WAF integration
     - Custom domain with SSL

6. Application Load Balancer:
   - ALB for EKS
   - SSL/TLS termination
   - Health checks
   - Target groups for backend dan frontend

7. IAM Roles & Policies:
   - EKS node role
   - RDS access role
   - S3 access role
   - CloudWatch logs role
   - Least privilege principle

8. Monitoring & Logging:
   - CloudWatch Logs
   - CloudWatch Metrics
   - X-Ray tracing
   - S3 for log storage

Terraform Structure:
terraform/
├── main.tf
├── variables.tf
├── outputs.tf
├── provider.tf
├── vpc/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── eks/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── rds/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── elasticache/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── s3/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
└── cloudfront/
    ├── main.tf
    ├── variables.tf
    └── outputs.tf

Terraform Best Practices:
- Use Terraform workspace untuk environment (dev, staging, prod)
- Use remote state (S3 + DynamoDB) untuk state management
- Use terraform validate sebelum apply
- Use terraform plan untuk review changes
- Lock state untuk concurrent modification prevention
- Use sensitive data handling untuk secrets

Example VPC Configuration:
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.0.0"

  name = "channelhub-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["ap-southeast-1a", "ap-southeast-1b", "ap-southeast-1c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

  enable_nat_gateway = true
  single_nat_gateway = false
  enable_vpn_gateway = false

  tags = {
    Environment = var.environment
    Project     = "channelhub"
  }
}

Example EKS Configuration:
module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "19.17.2"

  cluster_name    = "channelhub-${var.environment}"
  cluster_version = "1.28"

  vpc_id     = module.vpc.vpc_id
  subnet_ids = module.vpc.private_subnets

  cluster_endpoint_public_access  = true
  cluster_endpoint_private_access = true

  eks_managed_node_groups = {
    backend = {
      name           = "backend-nodegroup"
      instance_types = ["m5.large"]
      min_size       = 3
      max_size       = 10
      desired_size   = 3

      labels = {
        role = "backend"
      }
    }

    frontend = {
      name           = "frontend-nodegroup"
      instance_types = ["t3.medium"]
      min_size       = 2
      max_size       = 5
      desired_size   = 2

      labels = {
        role = "frontend"
      }
    }
  }
}

Deployment Workflow:
1. Initialize Terraform:
   terraform init

2. Plan infrastructure changes:
   terraform plan -out=tfplan

3. Review plan and apply:
   terraform apply tfplan

4. Destroy infrastructure (jika perlu):
   terraform destroy

Environment Management:
- Gunakan Terraform workspace:
  terraform workspace new dev
  terraform workspace new staging
  terraform workspace new prod

- Environment-specific variables:
  - dev: smaller instance, single AZ
  - staging: medium instance, multi-AZ
  - prod: large instance, multi-AZ, full HA

State Management:
- Remote state di S3:
  terraform {
    backend "s3" {
      bucket         = "channelhub-terraform-state"
      key            = "channelhub/terraform.tfstate"
      region         = "ap-southeast-1"
      encrypt        = true
      dynamodb_table = "channelhub-terraform-locks"
    }
  }

- State locking dengan DynamoDB untuk concurrent modification prevention

Pastikan IaC terkonfigurasi dengan proper high availability dan security.
```

---

---

## AI TRIGGER

### Tujuan Task
Menetapkan IaC strategy dan implementasi untuk infrastructure production ChannelHub.

### Konteks yang Perlu Dipahami AI
- Cloud provider: AWS (primary), GCP (alternative), Azure (alternative)
- Infrastructure as Code: Terraform
- Orchestration: Kubernetes (EKS/GKE/AKS)
- Networking: VPC, subnets, security groups
- Database: RDS PostgreSQL (multi-AZ)
- Cache: ElastiCache Redis (cluster mode)
- Storage: S3 untuk static assets dan backup
- CDN: CloudFront/Cloudflare

### Dependensi
- docs/00-foundation/009-global-implementation-rules.md (aturan global)
- docs/12-project-foundation/004-docker-infrastructure-design.md (Docker infrastructure)
- docs/21-integration-deployment/002-docker-compose-production.md (Docker compose production)
- .channelhub/OUTPUT-CONFIG.md (output directory structure)

### File/Folder yang Perlu Diperiksa
- docs/21-integration-deployment/004-environment-deployment.md (environment deployment)
- docs/15-database-implementation/008-database-backup-recovery-standard.md (backup recovery)

### Langkah Implementasi
1. Setup Terraform workspace dan state management
2. Define VPC dan networking
3. Define EKS cluster configuration
4. Define RDS PostgreSQL with multi-AZ
5. Define ElastiCache Redis cluster
6. Define S3 buckets dan CloudFront CDN
7. Define IAM roles dan policies
8. Setup monitoring dan logging

### Kriteria Keberhasilan (Definition of Done)
- Terraform configuration terdefinisi untuk seluruh infrastructure
- VPC dan networking terkonfigurasi dengan proper subnet
- EKS cluster terkonfigurasi dengan proper node groups
- RDS dan ElastiCache terkonfigurasi dengan high availability
- CDN dan storage terkonfigurasi
- Monitoring dan logging terintegrasi

### Prompt Implementasi
```
Anda akan mengimplementasikan Infrastructure as Code untuk ChannelHub.

Baca docs/21-integration-deployment/005-infrastructure-as-code.md untuk memahami IaC strategy.

Cloud Provider Choice: AWS (primary), dengan Terraform sebagai IaC tool.

Infrastructure Components:
1. VPC & Networking:
   - VPC dengan CIDR 10.0.0.0/16
   - Public subnets (3 AZ) untuk load balancer
   - Private subnets (3 AZ) untuk EKS nodes, RDS, ElastiCache
   - NAT Gateway untuk outbound internet access
   - Security groups dengan proper ingress/egress rules

2. EKS Cluster:
   - EKS cluster version 1.28+
   - Managed node groups:
     - Backend nodes (m5.large, 3 nodes, auto-scaling 3-10)
     - Frontend nodes (t3.medium, 2 nodes, auto-scaling 2-5)
   - Fargate profiles untuk worker nodes
   - Kubernetes version upgrade strategy

3. RDS PostgreSQL:
   - PostgreSQL 15
   - Multi-AZ deployment
   - Instance class: db.r6g.xlarge (memory optimized)
   - Storage: 500GB GP3, provisioned IOPS
   - Automated backups (retention 30 days)
   - Read replicas (2 for read-heavy workloads)
   - Parameter group dengan optimized settings

4. ElastiCache Redis:
   - Redis 7 cluster mode
   - 3 node clusters (1 primary, 2 replicas)
   - Instance class: cache.r6g.large
   - Multi-AZ deployment
   - Automatic failover
   - Encryption at rest and in transit

5. S3 & CloudFront:
   - S3 buckets:
     - channelhub-static-assets (public)
     - channelhub-uploads (private)
     - channelhub-backups (private)
   - CloudFront CDN:
     - Distribution for static assets
     - WAF integration
     - Custom domain with SSL

6. Application Load Balancer:
   - ALB for EKS
   - SSL/TLS termination
   - Health checks
   - Target groups for backend and frontend

7. IAM Roles & Policies:
   - EKS node role
   - RDS access role
   - S3 access role
   - CloudWatch logs role
   - Least privilege principle

8. Monitoring & Logging:
   - CloudWatch Logs
   - CloudWatch Metrics
   - X-Ray tracing
   - S3 for log storage

Terraform Structure:
terraform/
├── main.tf
├── variables.tf
├── outputs.tf
├── provider.tf
├── vpc/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── eks/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── rds/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── elasticache/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── s3/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
└── cloudfront/
    ├── main.tf
    ├── variables.tf
    └── outputs.tf

Terraform Best Practices:
- Use Terraform workspace untuk environment (dev, staging, prod)
- Use remote state (S3 + DynamoDB) untuk state management
- Use terraform validate sebelum apply
- Use terraform plan untuk review changes
- Lock state untuk concurrent modification prevention
- Use sensitive data handling untuk secrets

Example VPC Configuration:
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.0.0"

  name = "channelhub-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["ap-southeast-1a", "ap-southeast-1b", "ap-southeast-1c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

  enable_nat_gateway = true
  single_nat_gateway = false
  enable_vpn_gateway = false

  tags = {
    Environment = var.environment
    Project     = "channelhub"
  }
}

Example EKS Configuration:
module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "19.17.2"

  cluster_name    = "channelhub-${var.environment}"
  cluster_version = "1.28"

  vpc_id     = module.vpc.vpc_id
  subnet_ids = module.vpc.private_subnets

  cluster_endpoint_public_access  = true
  cluster_endpoint_private_access = true

  eks_managed_node_groups = {
    backend = {
      name           = "backend-nodegroup"
      instance_types = ["m5.large"]
      min_size       = 3
      max_size       = 10
      desired_size   = 3

      labels = {
        role = "backend"
      }
    }

    frontend = {
      name           = "frontend-nodegroup"
      instance_types = ["t3.medium"]
      min_size       = 2
      max_size       = 5
      desired_size   = 2

      labels = {
        role = "frontend"
      }
    }
  }
}

Example RDS Configuration:
module "rds" {
  source  = "terraform-aws-modules/rds/aws"
  version = "6.0.0"

  identifier = "channelhub-${var.environment}"

  engine               = "postgres"
  engine_version       = "15.4"
  family               = "postgres15"
  major_engine_version = "15"

  instance_class = "db.r6g.xlarge"
  allocated_storage = 500
  storage_encrypted = true

  db_name  = "channelhub"
  username = var.db_username
  password = var.db_password

  multi_az               = true
  db_subnet_group_name   = module.vpc.database_subnet_group
  vpc_security_group_ids = [module.security_group.rds_sg_id]

  backup_retention_period = 30
  backup_window          = "03:00-04:00"
  maintenance_window    = "Mon:04:00-Mon:05:00"

  performance_insights_enabled = true
  monitoring_interval         = 60

  read_replica_count = 2
}

Deployment Workflow:
1. Initialize Terraform:
   terraform init

2. Plan infrastructure changes:
   terraform plan -out=tfplan

3. Review plan and apply:
   terraform apply tfplan

4. Destroy infrastructure (jika perlu):
   terraform destroy

Environment Management:
- Gunakan Terraform workspace:
  terraform workspace new dev
  terraform workspace new staging
  terraform workspace new prod

- Environment-specific variables:
  - dev: smaller instance, single AZ
  - staging: medium instance, multi-AZ
  - prod: large instance, multi-AZ, full HA

State Management:
- Remote state di S3:
  terraform {
    backend "s3" {
      bucket         = "channelhub-terraform-state"
      key            = "channelhub/terraform.tfstate"
      region         = "ap-southeast-1"
      encrypt        = true
      dynamodb_table = "channelhub-terraform-locks"
    }
  }

- State locking dengan DynamoDB untuk concurrent modification prevention

Pastikan IaC terkonfigurasi dengan proper high availability dan security.
```

---

## Cloud Provider Selection

### Primary: AWS

**Alasan:**
- Mature service ecosystem
- Wide availability of managed services
- Strong compliance certifications
- Good integration with Kubernetes (EKS)

### Alternative: GCP

**Alasan:**
- Superior Kubernetes (GKE)
- Better data analytics services
- Competitive pricing

### Alternative: Azure

**Alasan:**
- Enterprise integration
- Hybrid cloud support
- Good Windows support

---

## Terraform Structure

```
terraform/
├── main.tf                 # Root configuration
├── variables.tf            # Input variables
├── outputs.tf              # Output values
├── provider.tf             # Provider configuration
├── backend.tf              # State configuration
├── vpc/                    # VPC module
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── eks/                    # EKS cluster module
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── rds/                    # RDS PostgreSQL module
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── elasticache/            # ElastiCache Redis module
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── s3/                     # S3 buckets module
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── cloudfront/             # CloudFront CDN module
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── alb/                    # Application Load Balancer
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── iam/                    # IAM roles and policies
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
└── monitoring/             # CloudWatch monitoring
    ├── main.tf
    ├── variables.tf
    └── outputs.tf
```

---

## VPC Configuration

### VPC with Multi-AZ

```hcl
# vpc/main.tf
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.0.0"

  name = "channelhub-${var.environment}-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["ap-southeast-1a", "ap-southeast-1b", "ap-southeast-1c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]
  database_subnets = ["10.0.201.0/24", "10.0.202.0/24", "10.0.203.0/24"]

  enable_nat_gateway     = true
  single_nat_gateway     = false
  one_nat_gateway_per_az = true
  enable_vpn_gateway     = false

  # DNS support
  enable_dns_hostnames = true
  enable_dns_support   = true

  # VPC Flow Logs
  enable_flow_log                      = true
  flow_log_destination_arn             = aws_cloudwatch_log_group.vpc_flow_log.arn
  flow_log_destination_type           = "cloud-watch-logs"
  flow_log_destination_format_version = "1"

  tags = {
    Environment = var.environment
    Project     = "channelhub"
    ManagedBy   = "terraform"
  }
}

resource "aws_cloudwatch_log_group" "vpc_flow_log" {
  name              = "/aws/vpc/flow-logs/channelhub-${var.environment}"
  retention_in_days = 30
}
```

### Security Groups

```hcl
# vpc/security-groups.tf
resource "aws_security_group" "eks_nodes" {
  name        = "channelhub-${var.environment}-eks-nodes"
  description = "Security group for EKS nodes"
  vpc_id      = module.vpc.vpc_id

  ingress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = [module.vpc.vpc_cidr_block]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "channelhub-${var.environment}-eks-nodes"
  }
}

resource "aws_security_group" "rds" {
  name        = "channelhub-${var.environment}-rds"
  description = "Security group for RDS"
  vpc_id      = module.vpc.vpc_id

  ingress {
    from_port       = 5432
    to_port         = 5432
    protocol        = "tcp"
    security_groups = [aws_security_group.eks_nodes.id]
  }

  tags = {
    Name = "channelhub-${var.environment}-rds"
  }
}
```

---

## EKS Configuration

### EKS Cluster

```hcl
# eks/main.tf
module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "19.17.2"

  cluster_name    = "channelhub-${var.environment}"
  cluster_version = "1.28"

  vpc_id     = module.vpc.vpc_id
  subnet_ids = module.vpc.private_subnets

  cluster_endpoint_public_access  = true
  cluster_endpoint_private_access = true

  # Cluster IAM role
  cluster_iam_role_name = "channelhub-${var.environment}-cluster-role"

  # Cluster addons
  cluster_addons = {
    coredns = {
      most_recent = true
    }
    kube-proxy = {
      most_recent = true
    }
    vpc-cni = {
      most_recent = true
    }
    aws-ebs-csi-driver = {
      most_recent = true
    }
  }

  # Managed node groups
  eks_managed_node_groups = {
    backend = {
      name           = "backend-nodegroup"
      instance_types = ["m5.large"]
      min_size       = 3
      max_size       = 10
      desired_size   = 3

      labels = {
        role = "backend"
      }

      taints = []
    }

    frontend = {
      name           = "frontend-nodegroup"
      instance_types = ["t3.medium"]
      min_size       = 2
      max_size       = 5
      desired_size   = 2

      labels = {
        role = "frontend"
      }

      taints = []
    }

    worker = {
      name           = "worker-nodegroup"
      instance_types = ["m5.large"]
      min_size       = 2
      max_size       = 8
      desired_size   = 2

      labels = {
        role = "worker"
      }

      taints = [{
        key    = "dedicated"
        value = "worker"
        effect = "NO_SCHEDULE"
      }]
    }
  }

  tags = {
    Environment = var.environment
    Project     = "channelhub"
  }
}
```

### EKS Addons

```hcl
# eks/addons.tf
resource "aws_eks_addon" "metrics_server" {
  cluster_name = module.eks.cluster_name
  addon_name   = "metrics-server"
  addon_version = "v1.11.1-eksbuild.1"
}

resource "aws_eks_addon" "autoscaler" {
  cluster_name = module.eks.cluster_name
  addon_name   = "cluster-autoscaler"
  addon_version = "v9.32.0-eksbuild.1"
}

resource "aws_eks_addon" "cert_manager" {
  cluster_name = module.eks.cluster_name
  addon_name   = "cert-manager"
  addon_version = "v1.13.0-eksbuild.1"
}
```

---

## RDS Configuration

### PostgreSQL Multi-AZ

```hcl
# rds/main.tf
module "rds" {
  source  = "terraform-aws-modules/rds/aws"
  version = "6.0.0"

  identifier = "channelhub-${var.environment}"

  engine               = "postgres"
  engine_version       = "15.4"
  family               = "postgres15"
  major_engine_version = "15"

  instance_class = "db.r6g.xlarge"
  allocated_storage = 500
  storage_encrypted = true
  storage_type      = "gp3"
  iops              = 3000

  db_name  = "channelhub"
  username = var.db_username
  password = var.db_password

  multi_az               = true
  db_subnet_group_name   = module.vpc.database_subnet_group
  vpc_security_group_ids = [aws_security_group.rds.id]

  # Backup
  backup_retention_period = 30
  backup_window          = "03:00-04:00"
  final_snapshot_identifier = "channelhub-${var.environment}-final-snapshot"

  # Maintenance
  maintenance_window    = "Mon:04:00-Mon:05:00"
  auto_minor_version_upgrade = false

  # Performance
  performance_insights_enabled = true
  monitoring_interval         = 60

  # Read replicas
  read_replica_count = 2

  # Parameters
  parameters = [
    {
      name  = "max_connections"
      value = "200"
    },
    {
      name  = "shared_buffers"
      value = "256MB"
    },
    {
      name  = "effective_cache_size"
      value = "12GB"
    },
    {
      name  = "maintenance_work_mem"
      value = "128MB"
    },
    {
      name  = "checkpoint_completion_target"
      value = "0.9"
    },
    {
      name  = "wal_buffers"
      value = "16MB"
    },
    {
      name  = "default_statistics_target"
      value = "100"
    },
    {
      name  = "random_page_cost"
      value = "1.1"
    },
    {
      name  = "effective_io_concurrency"
      value = "200"
    },
    {
      name  = "work_mem"
      value = "64MB"
    }
  ]

  tags = {
    Environment = var.environment
    Project     = "channelhub"
  }
}
```

---

## ElastiCache Configuration

### Redis Cluster Mode

```hcl
# elasticache/main.tf
module "elasticache" {
  source  = "terraform-aws-modules/elasticache/aws"
  version = "1.5.0"

  cluster_id     = "channelhub-${var.environment}"
  engine_version = "7.0"

  node_type            = "cache.r6g.large"
  num_cache_nodes      = 3
  parameter_group_name = aws_elasticache_parameter_group.redis.name
  engine               = "redis"
  port                 = 6379

  cluster_mode_enabled = true
  num_node_groups      = 1
  replicas_per_node_group = 2

  subnet_group_name  = module.vpc.elasticache_subnet_group
  security_group_ids = [aws_security_group.elasticache.id]

  at_rest_encryption_enabled = true
  transit_encryption_enabled = true
  auth_token               = var.redis_auth_token

  automatic_failover_enabled = true
  multi_az_enabled           = true

  snapshot_retention_limit = 7
  snapshot_window          = "02:00-03:00"

  tags = {
    Environment = var.environment
    Project     = "channelhub"
  }
}

resource "aws_elasticache_parameter_group" "redis" {
  family = "redis7"
  name   = "channelhub-${var.environment}-redis"

  parameter {
    name  = "maxmemory-policy"
    value = "allkeys-lru"
  }

  parameter {
    name  = "timeout"
    value = "300"
  }
}
```

---

## S3 & CloudFront Configuration

### S3 Buckets

```hcl
# s3/main.tf
resource "aws_s3_bucket" "static_assets" {
  bucket = "channelhub-${var.environment}-static-assets"

  tags = {
    Environment = var.environment
    Project     = "channelhub"
  }
}

resource "aws_s3_bucket_versioning" "static_assets" {
  bucket = aws_s3_bucket.static_assets.id
  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_public_access_block" "static_assets" {
  bucket = aws_s3_bucket.static_assets.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_website_configuration" "static_assets" {
  bucket = aws_s3_bucket.static_assets.id

  index_document {
    suffix = "index.html"
  }

  error_document {
    key = "error.html"
  }
}

resource "aws_s3_bucket" "uploads" {
  bucket = "channelhub-${var.environment}-uploads"

  tags = {
    Environment = var.environment
    Project     = "channelhub"
  }
}

resource "aws_s3_bucket_server_side_encryption_configuration" "uploads" {
  bucket = aws_s3_bucket.uploads.id

  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}
```

### CloudFront Distribution

```hcl
# cloudfront/main.tf
resource "aws_cloudfront_distribution" "static_assets" {
  enabled             = true
  is_ipv6_enabled     = true
  comment             = "ChannelHub ${var.environment} static assets"
  default_root_object = "index.html"

  origin {
    domain_name = aws_s3_bucket.static_assets.bucket_regional_domain_name
    origin_id   = "S3-${aws_s3_bucket.static_assets.id}"

    s3_origin_config {
      origin_access_identity = aws_cloudfront_origin_access_identity.static_assets.cloud_front_access_identity_path
    }
  }

  default_cache_behavior {
    allowed_methods  = ["GET", "HEAD", "OPTIONS"]
    cached_methods   = ["GET", "HEAD"]
    target_origin_id = "S3-${aws_s3_bucket.static_assets.id}"

    forwarded_values {
      query_string = false
      cookies {
        forward = "none"
      }
    }

    viewer_protocol_policy = "redirect-to-https"
    min_ttl                = 0
    default_ttl            = 86400
    max_ttl                = 31536000
    compress               = true
  }

  restrictions {
    geo_restriction {
      restriction_type = "none"
    }
  }

  viewer_certificate {
    acm_certificate_arn      = var.acm_certificate_arn
    ssl_support_method       = "sni-only"
    minimum_protocol_version = "TLSv1.2_2021"
  }

  tags = {
    Environment = var.environment
    Project     = "channelhub"
  }
}

resource "aws_cloudfront_origin_access_identity" "static_assets" {
  comment = "ChannelHub ${var.environment} static assets OAI"
}
```

---

## State Management

### Remote State Configuration

```hcl
# backend.tf
terraform {
  required_version = ">= 1.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  backend "s3" {
    bucket         = "channelhub-terraform-state"
    key            = "channelhub/${var.environment}/terraform.tfstate"
    region         = "ap-southeast-1"
    encrypt        = true
    dynamodb_table = "channelhub-terraform-locks"
  }
}
```

### DynamoDB Lock Table

```hcl
# resources.tf
resource "aws_dynamodb_table" "terraform_locks" {
  name         = "channelhub-terraform-locks"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "LockID"

  attribute {
    name = "LockID"
    type = "S"
  }

  tags = {
    Project = "channelhub"
  }
}
```

---

## Environment-Specific Configuration

### Variables

```hcl
# variables.tf
variable "environment" {
  description = "Environment name (dev, staging, prod)"
  type        = string
}

variable "region" {
  description = "AWS region"
  type        = string
  default     = "ap-southeast-1"
}

variable "db_username" {
  description = "Database username"
  type        = string
  sensitive   = true
}

variable "db_password" {
  description = "Database password"
  type        = string
  sensitive   = true
}

variable "redis_auth_token" {
  description = "Redis auth token"
  type        = string
  sensitive   = true
}

variable "acm_certificate_arn" {
  description = "ACM certificate ARN for CloudFront"
  type        = string
}
```

### Environment Variables Files

```hcl
# terraform.tfvars.dev
environment = "dev"
db_username = "channelhub_dev"
db_password = "dev_password_123"
redis_auth_token = "dev_redis_token_123"
acm_certificate_arn = "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"

# terraform.tfvars.staging
environment = "staging"
db_username = "channelhub_staging"
db_password = "staging_password_123"
redis_auth_token = "staging_redis_token_123"
acm_certificate_arn = "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"

# terraform.tfvars.prod
environment = "prod"
db_username = "channelhub_prod"
db_password = "prod_password_secure"
redis_auth_token = "prod_redis_token_secure"
acm_certificate_arn = "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
```

---

## Deployment Workflow

### Terraform Commands

```bash
# Initialize
terraform init

# Select workspace
terraform workspace select dev

# Plan changes
terraform plan -out=tfplan

# Apply changes
terraform apply tfplan

# Destroy
terraform destroy

# Import existing resources
terraform import aws_vpc.main vpc-12345678
```

### CI/CD Integration

```yaml
# .github/workflows/infrastructure.yml
name: Infrastructure

on:
  push:
    branches: [main]
  pull_request:

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: 1.5.0
      
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-region: ap-southeast-1
          role-to-assume: ${{ secrets.AWS_ROLE_ARN }}
      
      - name: Terraform Init
        run: terraform init
      
      - name: Terraform Plan
        run: terraform plan -out=tfplan
        env:
          TF_VAR_environment: staging
      
      - name: Terraform Apply
        if: github.ref == 'refs/heads/main'
        run: terraform apply tfplan
        env:
          TF_VAR_environment: staging
```

END OF INFRASTRUCTURE AS CODE