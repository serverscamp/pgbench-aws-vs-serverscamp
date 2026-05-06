# AWS RDS - pgbench init

`pgbench -i -s N` to load test tables for the three database sizes.

## Commands

```bash
export PGHOST="<aws-rds-endpoint>"
export PGPORT="5432"
export PGUSER="postgres"
export PGPASSWORD='<aws-master-password>'
export PGSSLMODE="require"

# Create three databases
psql -d postgres <<'SQL'
CREATE DATABASE bench100;
CREATE DATABASE bench1000;
CREATE DATABASE bench10000;
SQL

# Initialize each (run in screen/tmux - bench10000 takes ~26 min)
time pgbench -d bench100   -i -s 100
time pgbench -d bench1000  -i -s 1000
time pgbench -d bench10000 -i -s 10000
```

`pgbench -i` default steps (`-I dtgvp`): drop -> create tables -> generate (client-side COPY) -> vacuum -> primary keys.

## Raw output

```
$ time pgbench -h $H -U postgres -d bench100   -i -s 100
dropping old tables...
NOTICE:  table "pgbench_accounts" does not exist, skipping
NOTICE:  table "pgbench_branches" does not exist, skipping
NOTICE:  table "pgbench_history" does not exist, skipping
NOTICE:  table "pgbench_tellers" does not exist, skipping
creating tables...
generating data (client-side)...
vacuuming...
creating primary keys...
done in 12.69 s (drop tables 0.00 s, create tables 0.01 s, client-side generate 8.28 s, vacuum 0.15 s, primary keys 4.25 s).

real    0m12.727s
user    0m0.941s
sys     0m0.068s

$ time pgbench -h $H -U postgres -d bench1000  -i -s 1000
dropping old tables...
NOTICE:  table "pgbench_accounts" does not exist, skipping
NOTICE:  table "pgbench_branches" does not exist, skipping
NOTICE:  table "pgbench_history" does not exist, skipping
NOTICE:  table "pgbench_tellers" does not exist, skipping
creating tables...
generating data (client-side)...
vacuuming...
creating primary keys...
done in 157.74 s (drop tables 0.00 s, create tables 0.01 s, client-side generate 96.58 s, vacuum 5.53 s, primary keys 55.62 s).

real    2m37.782s
user    0m9.594s
sys     0m0.523s

$ time pgbench -h $H -U postgres -d bench10000 -i -s 10000
dropping old tables...
NOTICE:  table "pgbench_accounts" does not exist, skipping
NOTICE:  table "pgbench_branches" does not exist, skipping
NOTICE:  table "pgbench_history" does not exist, skipping
NOTICE:  table "pgbench_tellers" does not exist, skipping
creating tables...
generating data (client-side)...
vacuuming...
creating primary keys...
done in 1561.67 s (drop tables 0.00 s, create tables 0.01 s, client-side generate 876.29 s, vacuum 1.59 s, primary keys 683.78 s).

real    26m1.711s
user    1m38.980s
sys     0m5.377
```

## Timings

| Database | scale | Approx size | Total | client-side generate | vacuum | primary keys |
|---|---:|---:|---:|---:|---:|---:|
| `bench100`   | 100   | ~1.5 GB  | 12.69 s   | 8.28 s   | 0.15 s | 4.25 s |
| `bench1000`  | 1000  | ~15 GB   | 157.74 s  | 96.58 s  | 5.53 s | 55.62 s |
| `bench10000` | 10000 | ~150 GB  | 1561.67 s | 876.29 s | 1.59 s | 683.78 s |
