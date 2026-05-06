# Runbook: bench100

In-cache workload: scale 100 (~1.5 GB), warmup 5 min, three 10-min measurement runs.

## Environment (set once per shell session)

```bash
export PGHOST="<db-endpoint>"
export PGPORT="5432"
export PGUSER="<db-user>"
export PGPASSWORD='<password>'
export PGSSLMODE="require"
```

`psql` and `pgbench` pick these libpq env vars automatically; no `-h`, `-U`, `sslmode=...` flags needed in commands below.

## Script

```bash
settle() {
  echo "[$(date +%H:%M:%S)] CHECKPOINT + waiting for autovacuum..."
  psql -d bench100 -c "CHECKPOINT;" >/dev/null
  while [ "$(psql -d bench100 -tAc "SELECT count(*) FROM pg_stat_activity WHERE backend_type='autovacuum worker'")" != "0" ]; do
    echo "  autovacuum still working..."
    sleep 10
  done
  echo "[$(date +%H:%M:%S)] ready"
}

mkdir -p ~/results/bench100 && cd ~/results/bench100

settle
echo "=== WARMUP ==="
pgbench -d bench100 -c 32 -j 4 -T 300 -M prepared --no-vacuum

for i in 1 2 3; do
  settle
  echo "=== RUN $i ==="
  pgbench -d bench100 -c 32 -j 4 -T 600 -M prepared --no-vacuum \
    --log --log-prefix=run${i} --aggregate-interval=10 \
    2>&1 | tee run${i}.txt
done

echo "=== DONE bench100 ==="
```

## Parameters

| Flag | Value | Purpose |
|---|---|---|
| `-c 32` | 32 concurrent clients | OLTP-level concurrency |
| `-j 4` | 4 worker threads on the client | matches client-side vCPU count |
| `-M prepared` | prepared statements | realistic for pooled apps |
| `-T 300` | warmup duration (s) | 5 min |
| `-T 600` | measurement duration (s) | 10 min |
| `--no-vacuum` | skip pgbench's pre-test VACUUM | preserves cache state from previous warmup |
| `--log --log-prefix=runN --aggregate-interval=10` | per-thread aggregate logs | TPS/latency every 10 s |

Repeats: 3 measurement runs.

## Output files

In `~/results/bench100/`:
- `runN.txt` - pgbench summary (TPS, avg latency, transactions, errors).
- `runN.<pid>` and `runN.<pid>.{1,2,3}` - per-thread aggregate logs.

## settle()

Before warmup and before each measurement run:
1. `CHECKPOINT` flushes dirty pages.
2. Active poll on `pg_stat_activity` until no `autovacuum worker` rows remain.

This ensures each phase starts from a comparable post-flush, post-vacuum state.
