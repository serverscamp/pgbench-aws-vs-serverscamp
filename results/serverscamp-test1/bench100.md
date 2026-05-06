# ServersCamp Test 1 - pgbench bench100

Configuration: 2 vCPU / 8 GB, Xeon Gold 6132 (Skylake 2017), storage 5 000 IOPS / 225 MB/s, client VM 1 Gbit.

## Commands
See [`runbooks/bench100.md`](../../runbooks/bench100.md). Parameters: `-c 32 -j 4 -M prepared --no-vacuum`, warmup 300 s, measurement 600 s × 3.

## Raw pgbench output

```
$ ./pgbench100.sh
[11:21:24] CHECKPOINT + waiting for autovacuum...
[11:21:31] ready
=== WARMUP ===
pgbench (18.3 (Ubuntu 18.3-1.pgdg24.04+1))
transaction type: <builtin: TPC-B (sort of)>
scaling factor: 100
query mode: prepared
number of clients: 32
number of threads: 4
maximum number of tries: 1
duration: 300 s
number of transactions actually processed: 1088642
number of failed transactions: 0 (0.000%)
latency average = 8.815 ms
initial connection time = 161.854 ms
tps = 3630.162605 (without initial connection time)

=== RUN 1 ===
duration: 600 s
number of transactions actually processed: 2227578
number of failed transactions: 0 (0.000%)
latency average = 8.618 ms
initial connection time = 201.988 ms
tps = 3713.246349 (without initial connection time)

=== RUN 2 ===
duration: 600 s
number of transactions actually processed: 2237000
number of failed transactions: 0 (0.000%)
latency average = 8.581 ms
initial connection time = 200.171 ms
tps = 3729.284525 (without initial connection time)

=== RUN 3 ===
duration: 600 s
number of transactions actually processed: 2268873
number of failed transactions: 0 (0.000%)
latency average = 8.460 ms
initial connection time = 196.469 ms
tps = 3782.450951 (without initial connection time)
=== DONE bench100 ===
```

## Per-run summary

| Phase | Duration | Transactions | TPS | Latency avg | Failed |
|---|---:|---:|---:|---:|---:|
| Warmup | 300 s | 1 088 642 | 3 630.16 | 8.815 ms | 0 |
| Run 1  | 600 s | 2 227 578 | 3 713.25 | 8.618 ms | 0 |
| Run 2  | 600 s | 2 237 000 | 3 729.28 | 8.581 ms | 0 |
| Run 3  | 600 s | 2 268 873 | 3 782.45 | 8.460 ms | 0 |
