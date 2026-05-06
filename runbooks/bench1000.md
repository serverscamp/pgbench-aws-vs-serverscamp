# Runbook: bench1000

Partial-cache workload: scale 1000 (~15 GB), warmup 5 min, three 10-min measurement runs.

Same parameters as `bench100`, only the database name changes.

## Environment

```bash
export PGHOST="<db-endpoint>"
export PGPORT="5432"
export PGUSER="<db-user>"
export PGPASSWORD='<password>'
export PGSSLMODE="require"
```

## Script

```bash
settle() {
  echo "[$(date +%H:%M:%S)] CHECKPOINT + waiting for autovacuum..."
  psql -d bench1000 -c "CHECKPOINT;" >/dev/null
  while [ "$(psql -d bench1000 -tAc "SELECT count(*) FROM pg_stat_activity WHERE backend_type='autovacuum worker'")" != "0" ]; do
    echo "  autovacuum still working..."
    sleep 10
  done
  echo "[$(date +%H:%M:%S)] ready"
}

mkdir -p ~/results/bench1000 && cd ~/results/bench1000

settle
echo "=== WARMUP ==="
pgbench -d bench1000 -c 32 -j 4 -T 300 -M prepared --no-vacuum

for i in 1 2 3; do
  settle
  echo "=== RUN $i ==="
  pgbench -d bench1000 -c 32 -j 4 -T 600 -M prepared --no-vacuum \
    --log --log-prefix=run${i} --aggregate-interval=10 \
    2>&1 | tee run${i}.txt
done

echo "=== DONE bench1000 ==="
```

## Parameters
Identical to [bench100](bench100.md): `-c 32 -j 4 -M prepared --no-vacuum`, warmup 300s, measurement 600s × 3.
