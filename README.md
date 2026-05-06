# pgbench: AWS RDS vs ServersCamp managed PostgreSQL - raw data & methodology

This repository is the **companion data package** for the benchmark article:

📰 **Article (canonical):** <https://serverscamp.com/docs/benchmarks/aws-rds-vs-serverscamp-postgres>

The article contains the full discussion: methodology rationale, charts, conclusions,
cost analysis, and stability/latency interpretation. This repo contains only the
underlying data: configs, exact commands, pgbench raw outputs, and per-thread
aggregate logs that the article references.

## Repo layout

```
.
├── docs/
│   └── methodology.md              Test design (what we did, in one page)
├── configs/
│   ├── aws-rds.md                  AWS RDS db.m6i.large parameters
│   ├── aws-ec2-client.md           AWS EC2 c6i.xlarge - pgbench client
│   ├── serverscamp-db.md           ServersCamp db-m-8g parameters
│   └── serverscamp-vm-client.md    ServersCamp VM - pgbench client
├── runbooks/                       Exact commands run on each provider
│   ├── bench100.md                 In-cache workload (5/10 min × 3)
│   ├── bench1000.md                Partial cache (5/10 min × 3)
│   └── bench10000.md               Disk-bound (10/20 min × 3)
├── results/
│   ├── aws/                        AWS pgbench raw outputs and summary tables
│   ├── serverscamp-test1/          Default storage QoS (5k IOPS / 225 MB/s)
│   └── serverscamp-test2/          Uplifted storage QoS (12k IOPS / 500 MB/s)
└── raw-logs/
    ├── README.md                   Aggregate-log format + parsing snippet
    ├── aws.tar                     AWS run.txt + per-thread aggregate logs
    ├── serverscamp-test1.tar
    └── serverscamp-test2.tar
```

## How to read

- For the **what / why / conclusions**: read the article.
- For the **exact hardware specs and tariffs**: see [`configs/`](configs/).
- For the **exact commands we ran**: see [`runbooks/`](runbooks/).
- For the **raw pgbench output and per-run TPS/latency tables**: see [`results/`](results/).
- For the **per-thread aggregate logs** (TPS/latency every 10s, used for stability and tail analysis): see [`raw-logs/`](raw-logs/).

## License

MIT - see [`LICENSE`](LICENSE).
