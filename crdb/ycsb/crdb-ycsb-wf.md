# Test 3 [CRDB, CY, F, Z]

Workload F is highly contended. It performs 50% single-row lookups and 50% single-column updates expressed as multi-statement read-modify-write transactions.

## results

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+

## 100M, single node load

    $ cockroach workload run ycsb 'postgresql://root@10.0.0.203:26257?sslmode=disable' --workload F --insert-count 100000000 --concurrency 125 --tolerate-errors

12000 TPS for workload F to 3 nodes.

## 100M, 6K, 840t, 28m

    $ cockroach workload run ycsb 'postgresql://root@127.0.0.1:5000?sslmode=disable' --workload F --insert-count 100000000 --concurrency 840 --max-rate 6000 --max-ops 10000000 --tolerate-errors 

    _elapsed___errors__ops/sec(inst)___ops/sec(cum)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)
     1681.0s        0         3046.5         2962.3      3.3     19.9     52.4    134.2 read
     1681.0s        0         2936.5         2961.5     17.8     46.1     75.5    142.6 readModifyWrite
     1682.0s        0         2970.9         2962.3      3.3     16.3     41.9    113.2 read

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__total
     1688.2s        0        5001023         2962.4     12.8      3.1     24.1    335.5   2684.4  read

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__total
     1688.2s        0        4999816         2961.7     28.2     16.3     50.3    436.2   2952.8  readModifyWrite

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__result
     1688.2s        0       10000839         5924.1     20.5     10.0     41.9    385.9   2952.8

