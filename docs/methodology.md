# Methodology

## Test stand

Two instances per side, equivalent on vCPU/RAM/disk:

| Role | AWS | ServersCamp |
|---|---|---|
| Database | managed PostgreSQL (RDS) | managed PostgreSQL |
| pgbench client | EC2 in the same AZ/network as the DB | VM in the same network as the DB |

Both clients are co-located with their database to keep network latency out of the result.

## Principle: nothing is tuned

- The database runs out of the box, exactly as the provider ships it.
- No `ALTER SYSTEM`, no custom parameter groups, no `postgresql.conf` tuning.
- Provider defaults are part of the product. If one defaults to `synchronous_commit=off` and the other to `on`, this is measured, not equalised.
- The client uses stock `pgbench` from the standard `postgresql-client` package.

## Test parameters

- **Tool:** pgbench (TPC-B-like).
- **Three database sizes** for different cache regimes:
  - `scale=100` (~1.5 GB) - fits in page cache.
  - `scale=1000` (~15 GB) - partially in cache.
  - `scale=10000` (~150 GB) - disk-bound.
- **pgbench flags:** `-c 32 -j 4 -M prepared --no-vacuum`.
- **Three repeats** per size. Before each repeat: `CHECKPOINT` plus active poll for `autovacuum worker` to drain.
- **Warmup before the series:** 5 min for bench100/bench1000, 10 min for bench10000.
- **Measurement:** 10 min for bench100/bench1000, 20 min for bench10000.
- **Per-thread aggregate logs** (`--log --aggregate-interval=10`) record TPS/latency in 10-second windows.

## Two ServersCamp configurations

ServersCamp is tested in two passes, differing only in storage QoS:

| | Test 1 | Test 2 |
|---|---|---|
| Storage IOPS | 5 000 | 12 000 |
| Storage throughput | 225 MB/s | 500 MB/s |
| Origin | first-run configuration | manually uplifted to match AWS RDS gp3 baseline |

All other parameters (vCPU, RAM, PG version, pg_hba) are identical between Test 1 and Test 2.

## What is recorded

- pgbench summary stdout (TPS, avg latency, transactions, errors).
- Per-thread aggregate logs (10-second windows: num_transactions, sum_latency, min_latency, max_latency).
- Provider-side metrics during runs (CPU, IOPS, throughput, connections) where available.
