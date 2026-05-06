# ServersCamp managed PostgreSQL - instance configuration

Pair to AWS RDS db.m6i.large for the comparative test.

## General
- **Instance name:** pgbench
- **Default database (auto-created):** `<scamp-default-db>`
- **PostgreSQL version:** 18 (PGDG build, 18.3)
- **Service class:** db-m-8g
- **Status:** online
- **Max connections:** 200

## Resources
| Parameter | Value |
|---|---|
| vCPU | 2 |
| RAM | 8 GB |
| Disk allocated | 500 GB |
| Disk max extent | 1000 GB |
| Max connections | 200 |
| PG version | 18.3 |
| CPU model | Intel Xeon Gold 6132 (Skylake-SP, 1st gen Xeon Scalable, 2017) |
| CPU base / all-core turbo | 2.6 / 3.0 GHz |

## Storage
Software-defined storage (SDS), tested in two configurations:

### Test 1
- IOPS: 5 000
- Throughput: 225 MB/s

### Test 2
- IOPS: 12 000
- Throughput: 500 MB/s
- Manually uplifted to match AWS RDS gp3 baseline for 500 GiB allocations.

The two tests share all other parameters (vCPU, RAM, PG version). Each pass repeats the
full bench100 / bench1000 / bench10000 cycle from scratch, including pgbench `-i` re-init
after the storage change.

## Topology
Single instance, no replicas.

## Connection
- **HOST:** `postgres-<id>.apps.serverscamp.com`
- **DIRECT port:** 5432
- **Pooler port:** 6432 (PgBouncer; not used in this test - direct port matches AWS pattern of no RDS Proxy)
- **DATABASE:** `<scamp-default-db>` (auto-created); test databases bench100/bench1000/bench10000 created additionally
- **USER:** `<scamp-user>`
- **SSL in tests:** require

## Region
- Bucharest (eu-east-ro-1)

## Backup
Default settings, not modified.

## Features (per panel)
- Connection Pooling (PgBouncer)
- Extensions
- PITR
