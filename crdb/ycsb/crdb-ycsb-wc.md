# Test 3 [CRDB, CY, C, Z]

Workload C has no contention. It consists entirely of single-row lookups.

## results

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+

## 100M, 125t, single node load

    $ cockroach workload run ycsb 'postgresql://root@10.0.0.203:26257?sslmode=disable' --workload C --insert-count 100000000 --concurrency 125

20000 TPS for workload C to a single node.

40000 TPS for workload C to 3 nodes.

## 100M, 16K, 840t, 16m

    $ cockroach workload run ycsb 'postgresql://root@127.0.0.1:5000?sslmode=disable' --workload C --insert-count 100000000 --concurrency 840 --max-rate 38000 --max-ops 40000000 --tolerate-errors

    _elapsed___errors__ops/sec(inst)___ops/sec(cum)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)
     1061.0s        0        38363.7        37563.7      3.7     31.5     54.5    104.9 read
     1062.0s        0        37539.9        37563.7      3.9     33.6     54.5    104.9 read
     1063.0s        0        38396.1        37564.5      4.1     33.6     67.1    159.4 read
     1064.0s        0        37795.1        37564.7      4.5     33.6     71.3    117.4 read

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__total
     1064.8s        0       40000839        37565.3      8.6      3.9     31.5     56.6   1140.9  read

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__result
     1064.8s        0       40000839        37565.3      8.6      3.9     31.5     56.6   1140.9

37593 on avg = ops / elapsed.
