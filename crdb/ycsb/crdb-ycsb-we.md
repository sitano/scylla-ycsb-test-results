# Test 3 [CRDB, CY, E, Z]

Workload E has moderate contention. It performs 95% multi-row scans and 5% single-row insertion.

## results

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+

## 100M, single node load

    $ cockroach workload run ycsb 'postgresql://root@10.0.0.203:26257?sslmode=disable' --workload E --insert-count 100000000 --concurrency 125 --tolerate-errors

2000 TPS for workload E to a single node. Does not scale.

## 100M, 2K, 840t, 17m

    $ cockroach workload run ycsb 'postgresql://root@127.0.0.1:5000?sslmode=disable' --workload E --insert-count 100000000 --concurrency 840 --max-rate 2000 --max-ops 2000000 --tolerate-errors

    _elapsed___errors__ops/sec(inst)___ops/sec(cum)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)
     1041.0s   103852         1933.7         1900.1     58.7    318.8    469.8    704.6 scan
     1053.0s   105043         1891.3         1900.1     48.2    369.1    570.4    671.1 scan

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__total
     1053.2s   105049        2000839         1899.8     91.7     39.8    352.3    536.9   1811.9  scan

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__result
     1053.2s   105049        2000839         1899.8     91.7     39.8    352.3    536.9   1811.9

