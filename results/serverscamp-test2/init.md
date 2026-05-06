# ServersCamp Test 2 - pgbench init

Configuration: 2 vCPU / 8 GB, storage **12 000 IOPS / 500 MB/s** (matched to AWS gp3 baseline), client VM 1 Gbit.

The three databases were dropped and recreated after the storage QoS uplift, then re-initialised from scratch.

## Commands

```bash
psql -d <scamp-default-db> <<'SQL'
DROP DATABASE IF EXISTS bench100;
DROP DATABASE IF EXISTS bench1000;
DROP DATABASE IF EXISTS bench10000;
CREATE DATABASE bench100;
CREATE DATABASE bench1000;
CREATE DATABASE bench10000;
SQL

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
done in 14.97 s (drop tables 0.00 s, create tables 0.01 s, client-side generate 10.92 s, vacuum 0.23 s, primary keys 3.81 s).

real    0m15.105s
user    0m1.229s
sys     0m0.135s

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
done in 175.61 s (drop tables 0.00 s, create tables 0.01 s, client-side generate 120.03 s, vacuum 2.81 s, primary keys 52.77 s).

real    2m55.672s
user    0m12.553s
sys     0m1.674s

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
done in 1479.85 s (drop tables 0.00 s, create tables 0.01 s, client-side generate 941.12 s, vacuum 3.33 s, primary keys 535.40 s).

real    24m39.970s
user    2m8.658s
sys     0m11.364s
```

## Timings

| Database | scale | Approx size | Total | client-side generate | vacuum | primary keys |
|---|---:|---:|---:|---:|---:|---:|
| `bench100`   | 100   | ~1.5 GB  | 14.97 s   | 10.92 s  | 0.23 s | 3.81 s |
| `bench1000`  | 1000  | ~15 GB   | 175.61 s  | 120.03 s | 2.81 s | 52.77 s |
| `bench10000` | 10000 | ~150 GB  | 1479.85 s | 941.12 s | 3.33 s | 535.40 s |
