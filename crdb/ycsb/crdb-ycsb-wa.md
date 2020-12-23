# Test 3 [CRDB, CY, A, Z]

Workload A is highly contended. It performs 50% single-row lookups and 50% single-column updates. Using column families breaks the contention between all updates to different columns of the same row, so we use them by default.

## results

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+

## 100M, single node load

    $ cockroach workload run ycsb 'postgresql://root@10.0.0.203:26257?sslmode=disable' --workload A --insert-count 100000000 --concurrency 125

    _elapsed___errors__ops/sec(inst)___ops/sec(cum)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)
       11.0s        0         6623.5         6252.8      2.4      9.4     56.6    142.6 read
       11.0s        0         6333.6         6227.2      6.8     17.8    318.8    939.5 update
       12.0s        0         6484.1         6271.9      2.4     10.0     50.3    104.9 read
       12.0s        0         6446.1         6245.2      6.6     15.7    453.0   1006.6 update
       13.0s        0         6427.5         6283.8      2.4     11.0     56.6    113.2 read
       13.0s        0         6458.5         6261.6      6.6     16.8    260.0   1208.0 update

## 100M, 16K, 840t, 33m

With HAProxy in round robin mode and --max-rate:

    $ cockroach workload run ycsb 'postgresql://root@127.0.0.1:5000?sslmode=disable' --workload A --insert-count 100000000 --concurrency 840 --max-rate 16000 --max-ops 30000000 --tolerate-errors

    _elapsed___errors__ops/sec(inst)___ops/sec(cum)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)
       31.0s        0         7951.3         7859.5      3.3     24.1     48.2    100.7 read
       31.0s        0         7916.3         7828.2      7.3     39.8     60.8    130.0 update
       32.0s        0         8020.8         7864.8      3.1     16.8     39.8     75.5 read
       32.0s        0         7974.9         7833.0      7.1     30.4     44.0     79.7 update
       33.0s        0         8040.0         7870.1      3.1     18.9     41.9     83.9 read
       33.0s        0         8072.0         7840.3      7.1     33.6     50.3    113.2 update

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__total
     2012.2s        0       15001693         7455.5      5.8      3.1     19.9     46.1   7784.6  read

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__total
     2012.2s        0       14999146         7454.2     11.1      7.1     32.5     56.6  20401.1  update

    _elapsed___errors_____ops(total)___ops/sec(cum)__avg(ms)__p50(ms)__p95(ms)__p99(ms)_pMax(ms)__result
     2012.2s        0       30000839        14909.7      8.5      4.5     27.3     52.4  20401.1

840 threads gives maximum stable result at 16000 TPS for workload A.
