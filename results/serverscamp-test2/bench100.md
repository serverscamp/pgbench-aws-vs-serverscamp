# ServersCamp Test 2 - pgbench bench100

Configuration: 2 vCPU / 8 GB, Xeon Gold 6132 (Skylake 2017), storage 12 000 IOPS / 500 MB/s (AWS-equivalent).

## Commands
See [`runbooks/bench100.md`](../../runbooks/bench100.md). Parameters: `-c 32 -j 4 -M prepared --no-vacuum`, warmup 300 s, measurement 600 s × 3.

## Raw pgbench output

```
$ ./pgbench100.sh
=== WARMUP ===
pgbench (18.3 (Ubuntu 18.3-1.pgdg24.04+1))
transaction type: <builtin: TPC-B (sort of)>
scaling factor: 100
query mode: prepared
number of clients: 32
number of threads: 4
maximum number of tries: 1
duration: 300 s
number of transactions actually processed: 1069420
number of failed transactions: 0 (0.000%)
latency average = 8.972 ms
initial connection time = 200.063 ms
tps = 3566.573388 (without initial connection time)

=== RUN 1 ===
duration: 600 s
number of transactions actually processed: 2246325
number of failed transactions: 0 (0.000%)
latency average = 8.545 ms
initial connection time = 183.938 ms
tps = 3744.748138 (without initial connection time)

=== RUN 2 ===
duration: 600 s
number of transactions actually processed: 2199119
number of failed transactions: 0 (0.000%)
latency average = 8.729 ms
initial connection time = 165.478 ms
tps = 3665.942477 (without initial connection time)

=== RUN 3 ===
duration: 600 s
number of transactions actually processed: 2254345
number of failed transactions: 0 (0.000%)
latency average = 8.515 ms
initial connection time = 198.904 ms
tps = 3758.203930 (without initial connection time)
=== DONE bench100 ===
```

## Per-run summary

| Phase | Duration | Transactions | TPS | Latency avg | Failed |
|---|---:|---:|---:|---:|---:|
| Warmup | 300 s | 1 069 420 | 3 566.57 | 8.972 ms | 0 |
| Run 1  | 600 s | 2 246 325 | 3 744.75 | 8.545 ms | 0 |
| Run 2  | 600 s | 2 199 119 | 3 665.94 | 8.729 ms | 0 |
| Run 3  | 600 s | 2 254 345 | 3 758.20 | 8.515 ms | 0 |
