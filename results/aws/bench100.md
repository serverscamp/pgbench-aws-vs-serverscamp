# AWS RDS - pgbench bench100

Configuration: 2 vCPU / 8 GiB, gp3 12 000 IOPS / 500 MB/s, scale 100 (~1.5 GB).

## Commands
See [`runbooks/bench100.md`](../../runbooks/bench100.md). Parameters: `-c 32 -j 4 -M prepared --no-vacuum`, warmup 300 s, measurement 600 s × 3.

## Raw pgbench output

```
$ ./bench100.sh
[07:19:50] CHECKPOINT + waiting for autovacuum...
[07:19:50] ready
=== WARMUP ===
pgbench (18.3 (Ubuntu 18.3-1))
transaction type: <builtin: TPC-B (sort of)>
scaling factor: 100
query mode: prepared
number of clients: 32
number of threads: 4
maximum number of tries: 1
duration: 300 s
number of transactions actually processed: 1185125
number of failed transactions: 0 (0.000%)
latency average = 8.097 ms
initial connection time = 146.725 ms
tps = 3952.077593 (without initial connection time)
[07:24:51] CHECKPOINT + waiting for autovacuum...
[07:24:53] ready
=== RUN 1 ===
pgbench (18.3 (Ubuntu 18.3-1))
transaction type: <builtin: TPC-B (sort of)>
scaling factor: 100
query mode: prepared
number of clients: 32
number of threads: 4
maximum number of tries: 1
duration: 600 s
number of transactions actually processed: 2385331
number of failed transactions: 0 (0.000%)
latency average = 8.047 ms
initial connection time = 157.934 ms
tps = 3976.442323 (without initial connection time)
[07:34:53] CHECKPOINT + waiting for autovacuum...
[07:34:55] ready
=== RUN 2 ===
pgbench (18.3 (Ubuntu 18.3-1))
transaction type: <builtin: TPC-B (sort of)>
scaling factor: 100
query mode: prepared
number of clients: 32
number of threads: 4
maximum number of tries: 1
duration: 600 s
number of transactions actually processed: 2380108
number of failed transactions: 0 (0.000%)
latency average = 8.065 ms
initial connection time = 143.238 ms
tps = 3967.670593 (without initial connection time)
[07:44:55] CHECKPOINT + waiting for autovacuum...
[07:44:57] ready
=== RUN 3 ===
pgbench (18.3 (Ubuntu 18.3-1))
transaction type: <builtin: TPC-B (sort of)>
scaling factor: 100
query mode: prepared
number of clients: 32
number of threads: 4
maximum number of tries: 1
duration: 600 s
number of transactions actually processed: 2411845
number of failed transactions: 0 (0.000%)
latency average = 7.959 ms
initial connection time = 152.182 ms
tps = 4020.635072 (without initial connection time)
=== DONE bench100 ===
```

## Per-run summary

| Phase | Duration | Transactions | TPS | Latency avg | Failed |
|---|---:|---:|---:|---:|---:|
| Warmup | 300 s | 1 185 125 | 3 952.08 | 8.097 ms | 0 |
| Run 1  | 600 s | 2 385 331 | 3 976.44 | 8.047 ms | 0 |
| Run 2  | 600 s | 2 380 108 | 3 967.67 | 8.065 ms | 0 |
| Run 3  | 600 s | 2 411 845 | 4 020.64 | 7.959 ms | 0 |
