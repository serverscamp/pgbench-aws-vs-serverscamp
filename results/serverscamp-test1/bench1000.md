# ServersCamp Test 1 - pgbench bench1000

Configuration: 2 vCPU / 8 GB, Xeon Gold 6132 (Skylake 2017), storage 5 000 IOPS / 225 MB/s, client VM 1 Gbit.

## Commands
See [`runbooks/bench1000.md`](../../runbooks/bench1000.md). Parameters: `-c 32 -j 4 -M prepared --no-vacuum`, warmup 300 s, measurement 600 s × 3.

## Raw pgbench output

```
$ ./pgbench1000.sh
[CHECKPOINT + autovacuum drain]
=== WARMUP ===
pgbench (18.3 (Ubuntu 18.3-1.pgdg24.04+1))
transaction type: <builtin: TPC-B (sort of)>
scaling factor: 1000
query mode: prepared
number of clients: 32
number of threads: 4
maximum number of tries: 1
duration: 300 s
number of transactions actually processed: 638582
number of failed transactions: 0 (0.000%)
latency average = 15.025 ms
initial connection time = 218.284 ms
tps = 2129.755819 (without initial connection time)

=== RUN 1 ===
duration: 600 s
number of transactions actually processed: 1236180
number of failed transactions: 0 (0.000%)
latency average = 15.528 ms
initial connection time = 191.502 ms
tps = 2060.844743 (without initial connection time)

=== RUN 2 ===
duration: 600 s
number of transactions actually processed: 1358697
number of failed transactions: 0 (0.000%)
latency average = 14.128 ms
initial connection time = 187.422 ms
tps = 2265.009882 (without initial connection time)

=== RUN 3 ===
duration: 600 s
number of transactions actually processed: 1467869
number of failed transactions: 0 (0.000%)
latency average = 13.077 ms
initial connection time = 204.172 ms
tps = 2447.135228 (without initial connection time)
=== DONE bench1000 ===
```

## Per-run summary

| Phase | Duration | Transactions | TPS | Latency avg | Failed |
|---|---:|---:|---:|---:|---:|
| Warmup | 300 s | 638 582   | 2 129.76 | 15.025 ms | 0 |
| Run 1  | 600 s | 1 236 180 | 2 060.84 | 15.528 ms | 0 |
| Run 2  | 600 s | 1 358 697 | 2 265.01 | 14.128 ms | 0 |
| Run 3  | 600 s | 1 467 869 | 2 447.14 | 13.077 ms | 0 |
