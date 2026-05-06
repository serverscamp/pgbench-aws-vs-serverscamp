# AWS RDS PostgreSQL - instance configuration

## Region and deployment
- **Region:** Frankfurt (eu-central-1)
- **Availability Zone:** eu-central-1a
- **Engine:** PostgreSQL (not Aurora)
- **Engine version:** PostgreSQL 18.3-R1
- **RDS Extended Support:** disabled
- **Template:** Production
- **Deployment:** Single-AZ DB instance (no replicas, SLA 99.5%)
- **Database creation method:** Full configuration

## Identity and credentials
- **DB instance identifier:** database-1
- **Master username:** postgres
- **Credentials management:** Self-managed
- **Authentication:** Password authentication
- **Endpoint:** `database-1.<account-id>.eu-central-1.rds.amazonaws.com:5432`
- **DB name:** postgres
- **SSL in tests:** require

## Instance class
- **Class:** Standard (m-classes), current generation
- **Instance type:** db.m6i.large
  - vCPU: 2
  - RAM: 8 GiB
  - EBS Bandwidth: up to 10 000 Mbps
  - Network: up to 12.5 Gbps
- **CPU:** Intel Xeon Platinum 8375C (Ice Lake / 3rd gen Xeon Scalable, 2021)
  - Base 2.9 GHz, all-core turbo 3.5 GHz

## Storage
- **Type:** General Purpose SSD (gp3) - software-defined storage (SDS)
- **Allocated storage:** 500 GiB
- **Provisioned IOPS:** 12 000 (baseline included for ≥400 GiB allocations)
- **Storage throughput:** 500 MiBps (baseline included for ≥400 GiB allocations)

## Connectivity
- **Compute resource:** Don't connect to an EC2 compute resource
- **VPC:** Default VPC (vpc-<id>), 3 subnets / 3 AZ
- **DB subnet group:** default-vpc-<id>
- **Public access:** Yes
- **VPC security group:** default
- **RDS Proxy:** disabled
- **Certificate authority:** rds-ca-rsa2048-g1 (default)

## Monitoring
- **Database Insights:** Standard (7-day history)
- **KMS key:** default `aws/rds`
- **Enhanced Monitoring:** enabled, 60s granularity, role: default `rds-monitoring-role`
- **Log exports to CloudWatch:** none selected
- **DevOps Guru:** off

## Backup
- Default settings, not modified.

## Cost (estimated by AWS console at creation)
- DB instance: 154.76 USD/month
- Storage: 68.50 USD/month
- **Total: 223.26 USD/month** (on-demand, excludes backup storage / IO / data transfer)
