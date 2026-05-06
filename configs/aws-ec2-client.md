# AWS EC2 pgbench client

Client instance running pgbench against the RDS PostgreSQL DB.

## Basic
- **Name:** pgbench
- **AMI:** Ubuntu Server 26.04 LTS (HVM), SSD
  - Architecture: 64-bit (x86)
  - Username: `ubuntu`

## Instance type
- **Type:** c6i.xlarge
  - Family: c6i (compute-optimized, current gen)
  - vCPU: 4
  - Memory: 8 GiB
- **On-Demand Linux:** 0.194 USD/h

## Network
- **VPC:** default vpc-<id>, 172.31.0.0/16
- **Subnet:** subnet-<id>
  - AZ: eu-central-1a (same as RDS)
- **Auto-assign public IP:** Enable
- **Security group:** default sg-<id> (same SG as RDS, internal traffic permitted)
- **Network bandwidth:** up to 12.5 Gbps
- **Key pair:** `<keypair>`

## Storage
- **Root volume:** 100 GiB gp3, 3000 IOPS, not encrypted (SDS)

## OS
- Ubuntu 26.04 LTS, kernel 7.0.0-1004-aws (x86_64)

## pgbench
- pgbench (PostgreSQL) 18.3
