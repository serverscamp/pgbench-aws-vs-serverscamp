# ServersCamp Test 1 - pgbench init

Configuration: 2 vCPU / 8 GB, storage **5 000 IOPS / 225 MB/s**, client VM 4/8 on 1 Gbit network.

## Commands

```bash
export PGHOST="postgres-<id>.apps.serverscamp.com"
export PGPORT="5432"
export PGUSER="<scamp-user>"
export PGPASSWORD='<serverscamp-password>'
export PGSSLMODE="require"

# Create three databases
psql -d <scamp-default-db> <<'SQL'
CREATE DATABASE bench100;
CREATE DATABASE bench1000;
CREATE DATABASE bench10000;
SQL

# Initialize each
time pgbench -d bench100   -i -s 100
time pgbench -d bench1000  -i -s 1000
time pgbench -d bench10000 -i -s 10000
```

## Raw output

```
$ time pgbench -d bench100   -i -s 100
dropping old tables...
NOTICE:  table "pgbench_accounts" does not exist, skipping
NOTICE:  table "pgbench_branches" does not exist, skipping
NOTICE:  table "pgbench_history" does not exist, skipping
NOTICE:  table "pgbench_tellers" does not exist, skipping
creating tables...
generating data (client-side)...
vacuuming...
creating primary keys...
done in 20.57 s (drop tables 0.00 s, create tables 0.01 s, client-side generate 15.52 s, vacuum 0.33 s, primary keys 4.71 s).

real    0m20.723s
user    0m1.229s
sys     0m0.180s

$ time pgbench -d bench1000  -i -s 1000
dropping old tables...
NOTICE:  table "pgbench_accounts" does not exist, skipping
NOTICE:  table "pgbench_branches" does not exist, skipping
NOTICE:  table "pgbench_history" does not exist, skipping
NOTICE:  table "pgbench_tellers" does not exist, skipping
creating tables...
generating data (client-side)...
vacuuming...
creating primary keys...
done in 241.85 s (drop tables 0.00 s, create tables 0.01 s, client-side generate 153.33 s, vacuum 4.19 s, primary keys 84.33 s).

real    4m1.978s
user    0m12.384s
sys     0m0.974s

$ time pgbench -d bench10000 -i -s 10000
dropping old tables...
NOTICE:  table "pgbench_accounts" does not exist, skipping
NOTICE:  table "pgbench_branches" does not exist, skipping
NOTICE:  table "pgbench_history" does not exist, skipping
NOTICE:  table "pgbench_tellers" does not exist, skipping
creating tables...
generating data (client-side)...
vacuuming...
creating primary keys...
done in 2280.92 s (drop tables 0.00 s, create tables 0.02 s, client-side generate 1337.61 s, vacuum 5.95 s, primary keys 937.32 s).

real    38m1.080s
user    2m6.164s
sys     0m10.418s
```

## Timings

| Database | scale | Approx size | Total | client-side generate | vacuum | primary keys |
|---|---:|---:|---:|---:|---:|---:|
| `bench100`   | 100   | ~1.5 GB  | 20.57 s   | 15.52 s   | 0.33 s | 4.71 s |
| `bench1000`  | 1000  | ~15 GB   | 241.85 s  | 153.33 s  | 4.19 s | 84.33 s |
| `bench10000` | 10000 | ~150 GB  | 2280.92 s | 1337.61 s | 5.95 s | 937.32 s |
