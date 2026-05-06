# AWS RDS - pgbench bench1000

Configuration: 2 vCPU / 8 GiB, gp3 12 000 IOPS / 500 MB/s, scale 1000 (~15 GB).

## Commands
See [`runbooks/bench1000.md`](../../runbooks/bench1000.md). Parameters: `-c 32 -j 4 -M prepared --no-vacuum`, warmup 300 s, measurement 600 s × 3.

## Raw pgbench output

```
$ ./bench1000.sh
[CHECKPOINT + autovacuum drain]
=== WARMUP ===
pgbench (18.3 (Ubuntu 18.3-1))
transaction type: <builtin: TPC-B (sort of)>
scaling factor: 1000
query mode: prepared
number of clients: 32
number of threads: 4
maximum number of tries: 1
duration: 300 s
number of transactions actually processed: 754105
number of failed transactions: 0 (0.000%)
latency average = 12.725 ms
initial connection time = 148.729 ms
tps = 2514.762577 (without initial connection time)

=== RUN 1 ===
duration: 600 s
number of transactions actually processed: 1546357
number of failed transactions: 0 (0.000%)
latency average = 12.414 ms
initial connection time = 147.633 ms
tps = 2577.799610 (without initial connection time)

=== RUN 2 ===
duration: 600 s
number of transactions actually processed: 1656687
number of failed transactions: 0 (0.000%)
latency average = 11.587 ms
initial connection time = 150.050 ms
tps = 2761.722292 (without initial connection time)

=== RUN 3 ===
duration: 600 s
number of transactions actually processed: 1729727
number of failed transactions: 0 (0.000%)
latency average = 11.098 ms
initial connection time = 151.940 ms
tps = 2883.514321 (without initial connection time)
=== DONE bench1000 ===
```

## Per-run summary

| Phase | Duration | Transactions | TPS | Latency avg | Failed |
|---|---:|---:|---:|---:|---:|
| Warmup | 300 s | 754 105   | 2 514.76 | 12.725 ms | 0 |
| Run 1  | 600 s | 1 546 357 | 2 577.80 | 12.414 ms | 0 |
| Run 2  | 600 s | 1 656 687 | 2 761.72 | 11.587 ms | 0 |
| Run 3  | 600 s | 1 729 727 | 2 883.51 | 11.098 ms | 0 |
