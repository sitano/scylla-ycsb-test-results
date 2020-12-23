# Test 3 [CRDB, CY, B, Z]

Workload B is less contended than Workload A, but still bottlenecks on contention as concurrency grows. It performs 95% single-row lookups and 5% single-column updates.

## results

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+

## 100M, 125t, single node load

    $ cockroach workload run ycsb 'postgresql://root@10.0.0.203:26257?sslmode=disable' --workload B --insert-count 100000000 --concurrency 125

20000 TPS for workload B to a single node.

## 100M, 35K, 840t, 16m

    $ cockroach workload run ycsb 'postgresql://root@127.0.0.1:5000?sslmode=disable' --workload B --insert-count 100000000 --concurrency 840 --max-rate 35000 --max-ops 30000000 --tolerate-errors

    _elapsed___errors__ops/sec(inst)___ops/sec(cum)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)
       11.0s        0        32886.8        31782.7      5.5     50.3    117.4    302.0 read
       11.0s        0         1715.0         1690.1     17.8     67.1    121.6    469.8 update
       12.0s        0        32565.9        31848.1      5.8     62.9    121.6    260.0 read
       12.0s        0         1716.9         1692.3     21.0     88.1    125.8    352.3 update
       13.0s        0        33168.3        31948.7      5.8     52.4    113.2    318.8 read
       …
    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__total
     1003.8s        0       28500417        28391.5     12.7      4.5     46.1    125.8   7247.8  read

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__total
     1003.8s        0        1500422         1494.7    165.8     14.2     75.5    209.7 103079.2  update

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__result
     1003.8s        0       30000839        29886.2     20.4      4.7     50.3    125.8 103079.2

35000 TPS for workload B to 3 nodes.

Did not keep up.
