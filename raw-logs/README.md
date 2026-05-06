# Raw pgbench logs

Three archives, one per test configuration:

| File | Configuration |
|---|---|
| `aws.tar` | AWS RDS db.m6i.large, gp3 12k IOPS / 500 MB/s |
| `serverscamp-test1.tar` | ServersCamp db-m-8g, storage 5 000 IOPS / 225 MB/s |
| `serverscamp-test2.tar` | ServersCamp db-m-8g, storage 12 000 IOPS / 500 MB/s (uplifted to AWS-equivalent) |

## Layout inside each archive

```
results/<provider>/<scale>/
    runN.txt                 pgbench summary stdout (TPS, avg latency, transactions, errors)
    runN.<pid>               per-thread aggregate log, thread 0
    runN.<pid>.1             per-thread aggregate log, thread 1
    runN.<pid>.2             per-thread aggregate log, thread 2
    runN.<pid>.3             per-thread aggregate log, thread 3
```

`<scale>` is one of `bench100`, `bench1000`, `bench10000`. Each scale has runs 1, 2, 3.

The summary `runN.txt` is human-readable. The per-thread files are pgbench's
aggregated 10-second-window logs (we ran with `--aggregate-interval=10`).

## Aggregate-log line format

Each line is one 10-second window from one thread. Fields are space-separated:

```
interval_start  num_transactions  sum_latency  sum_latency_squared  min_latency  max_latency  0 0 0 0 ...
```

- `interval_start` - UNIX epoch seconds at the start of the window.
- `num_transactions` - completed in this window by this thread.
- `sum_latency`, `sum_latency_squared`, `min_latency`, `max_latency` - in **microseconds**.
- The trailing zeros are placeholders for fields that are populated only with `--rate` or `--latency-limit` (we used neither).

To compute TPS for a 10-second window, sum `num_transactions` across all 4 threads
for the same `interval_start` and divide by 10.

To compute average per-tx latency in a window: sum `sum_latency` / sum `num_transactions`.

## Reference: parsing aggregate logs

A standalone Python script with the same parsing logic that produced the numbers
in [`../analysis/comparison-stability.md`](../analysis/comparison-stability.md):

```python
import os, re, statistics

def latest_pid_files(run_dir, prefix):
    """For runN.PID and runN.PID.{1,2,3} files in run_dir, keep only the
    files belonging to the highest PID (a re-run produces fresher PID)."""
    pat = re.compile(rf"^{re.escape(prefix)}\.(\d+)(?:\.\d+)?$")
    by_pid = {}
    for f in os.listdir(run_dir):
        m = pat.match(f)
        if m:
            by_pid.setdefault(int(m.group(1)), []).append(os.path.join(run_dir, f))
    if not by_pid:
        return []
    return by_pid[max(by_pid)]

def per_interval(files):
    """Return list of (epoch, tps, avg_latency_ms, min_lat_ms, max_lat_ms)
    aggregated across threads."""
    by_ts = {}
    for f in files:
        with open(f) as fh:
            for line in fh:
                p = line.split()
                if len(p) < 6:
                    continue
                ts, n, sl, _, mn, mx = int(p[0]), int(p[1]), int(p[2]), 0, int(p[4]), int(p[5])
                d = by_ts.setdefault(ts, {"n": 0, "sl": 0, "mn": 10**18, "mx": 0})
                d["n"] += n
                d["sl"] += sl
                d["mn"] = min(d["mn"], mn)
                d["mx"] = max(d["mx"], mx)
    out = []
    for ts, d in sorted(by_ts.items()):
        if d["n"] == 0:
            continue
        out.append((ts, d["n"]/10.0, (d["sl"]/d["n"])/1000.0, d["mn"]/1000.0, d["mx"]/1000.0))
    return out

# example use
files = latest_pid_files("results/aws/bench10000", "run2")
rows  = per_interval(files)
tps   = [r[1] for r in rows]
print(f"mean={statistics.mean(tps):.0f} CoV={statistics.stdev(tps)/statistics.mean(tps)*100:.1f}%")
```

## Why filter to "latest PID per run"

If a run was interrupted and restarted, both the orphan partial logs (lower PID)
and the final complete logs (higher PID) end up in the same folder. Mixing them
gives wrong averages. The rule is: per `runN`, take only the files belonging to
the highest PID - that's the run that actually completed.

In this repo's archives, only one PID per `runN` is present in
`serverscamp-test2.tar` (orphan files from an interrupted attempt were removed).
The filtering rule is included above for general robustness when re-running.

## Reproducing the analysis numbers

The script above produces the per-run mean/median/CoV/p5/p95 values reported in
[`../analysis/comparison-stability.md`](../analysis/comparison-stability.md). Run it on
extracted archives to verify.
