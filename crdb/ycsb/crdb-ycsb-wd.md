# Test 3 [CRDB, CY, D, Z]

Workload D has no contention. It performs 95% single-row lookups and 5% single-row insertion.

## results

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+

## 100M, single node load

    $ cockroach workload run ycsb 'postgresql://root@10.0.0.203:26257?sslmode=disable' --workload D --insert-count 100000000 --concurrency 125

20000 TPS for workload D to a single node.

40000 TPS for workload D to 3 nodes

## 100M, 40K, 840t, 23m

    $ cockroach workload run ycsb 'postgresql://root@127.0.0.1:5000?sslmode=disable' --workload D --insert-count 100000000 --concurrency 840 --max-rate 40000 --max-ops 50000000 --tolerate-errors

    _elapsed___errors__ops/sec(inst)___ops/sec(cum)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)
     1411.0s        0         1889.5         1759.1     25.2     96.5    142.6    251.7 insert
     1411.0s        0        35797.8        33407.5      8.1     56.6    104.9    184.5 read
     1412.0s        0         1719.4         1759.1     19.9     92.3    125.8    159.4 insert
    _elapsed___errors__ops/sec(inst)___ops/sec(cum)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)
     1421.0s        0         1844.9         1759.2     24.1     96.5    167.8    285.2 insert
     1421.0s        0        33531.9        33408.2      8.1     62.9    142.6    243.3 read

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__total
     1421.8s        0        2501292         1759.3     34.9     24.1    100.7    176.2   3489.7  insert

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__total
     1421.8s        0       47499547        33409.0     16.9      7.6     58.7    125.8   3489.7  read

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__result
     1421.8s        0       50000839        35168.3     17.8      8.1     62.9    130.0   3489.7

