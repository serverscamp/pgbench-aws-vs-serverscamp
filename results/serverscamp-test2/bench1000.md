# ServersCamp Test 2 - pgbench bench1000

Configuration: 2 vCPU / 8 GB, Xeon Gold 6132 (Skylake 2017), storage 12 000 IOPS / 500 MB/s (AWS-equivalent).

## Commands
See [`runbooks/bench1000.md`](../../runbooks/bench1000.md). Parameters: `-c 32 -j 4 -M prepared --no-vacuum`, warmup 300 s, measurement 600 s × 3.

## Raw pgbench output

```
$ ./pgbench1000.sh
=== WARMUP ===
pgbench (18.3 (Ubuntu 18.3-1.pgdg24.04+1))
transaction type: <builtin: TPC-B (sort of)>
scaling factor: 1000
query mode: prepared
number of clients: 32
number of threads: 4
maximum number of tries: 1
duration: 300 s
number of transactions actually processed: 795068
number of failed transactions: 0 (0.000%)
latency average = 12.069 ms
initial connection time = 200.064 ms
tps = 2651.374544 (without initial connection time)

=== RUN 1 ===
duration: 600 s
number of transactions actually processed: 1609756
number of failed transactions: 0 (0.000%)
latency average = 11.924 ms
initial connection time = 181.860 ms
tps = 2683.568532 (without initial connection time)

=== RUN 2 ===
duration: 600 s
number of transactions actually processed: 1690231
number of failed transactions: 0 (0.000%)
latency average = 11.356 ms
initial connection time = 205.636 ms
tps = 2817.909326 (without initial connection time)

=== RUN 3 ===
duration: 600 s
number of transactions actually processed: 1730230
number of failed transactions: 0 (0.000%)
latency average = 11.095 ms
initial connection time = 169.460 ms
tps = 2884.131092 (without initial connection time)
=== DONE bench1000 ===
```

## Per-run summary

| Phase | Duration | Transactions | TPS | Latency avg | Failed |
|---|---:|---:|---:|---:|---:|
| Warmup | 300 s | 795 068   | 2 651.37 | 12.069 ms | 0 |
| Run 1  | 600 s | 1 609 756 | 2 683.57 | 11.924 ms | 0 |
| Run 2  | 600 s | 1 690 231 | 2 817.91 | 11.356 ms | 0 |
| Run 3  | 600 s | 1 730 230 | 2 884.13 | 11.095 ms | 0 |
