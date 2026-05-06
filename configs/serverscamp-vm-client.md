# ServersCamp VM - pgbench client

Client VM running pgbench against the ServersCamp managed PostgreSQL DB.

## Parameters
- **Hostname:** pgbench
- **VM Class:** burst-l (burst limits manually disabled - sustained CPU without throttling)
- **vCPU:** 4
- **RAM:** 8 GB
- **Image:** Ubuntu 25.04
- **Storage:** 100 GB standart-storage (SDS)
- **Network:** 1 Gbit (initial), later upgraded to 10 Gbit
- **Public IP:** IPv4 + IPv6
- **SSH Key:** main

## pgbench
- pgbench (PostgreSQL) 18.3 (PGDG build for Ubuntu 24.04)

## Cost (estimated)
- Compute (burst-l): €6.70/month
- Storage (100 GB): €6.48/month
- Network out (20 TB included): €7.78/month
- Public IP: €0.50/month
- **Total: €21.45/month** (≈ €0.03/h)

## Placement
Client VM and the managed DB share the same VPC, so traffic to
`postgres-<id>.apps.serverscamp.com` stays on the internal network -
the same setup pattern as the AWS EC2 client and RDS in a shared VPC.
