# Test 7 [Scylla, OY, E, Z]

Workload E has moderate contention. It performs 95% multi-row scans and 5% single-row insertion.

## results

Workload E does not scale on Scylla regardless of dataset size beyond the 10K TPS. Range scan is implemented as CQL WHERE token(PK) >= ? LIMIT RANDOM(1, 1000) that must read partitions sequentially. That means that even sequential token reads do not scale.

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+

## no target, 100M, 84t, 14c, 4.5s

    $ bin/ycsb run scylla -P workloads/workloade -threads 84 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=50000000 -p scylla.coreconnections=14 -p scylla.maxconnections=14 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p hosts=10.0.0.171,10.0.0.142,10.0.1.8 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 4576
    [OVERALL], Throughput(ops/sec), 2185.3146853146854
    [TOTAL_GCS_G1_Young_Generation], Count, 8
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 55
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 1.201923076923077
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 8
    [TOTAL_GC_TIME], Time(ms), 55
    [TOTAL_GC_TIME_%], Time(%), 1.201923076923077
    [CLEANUP], Operations, 84
    [CLEANUP], AverageLatency(us), 26369.369047619046
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2215935
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 11
    [INSERT], Operations, 532
    [INSERT], AverageLatency(us), 8054.2199248120305
    [INSERT], MinLatency(us), 313
    [INSERT], MaxLatency(us), 88063
    [INSERT], 95thPercentileLatency(us), 25711
    [INSERT], 99thPercentileLatency(us), 46815
    [INSERT], Return=OK, 532
    [SCAN], Operations, 9468
    [SCAN], AverageLatency(us), 13042.187473595268
    [SCAN], MinLatency(us), 543
    [SCAN], MaxLatency(us), 137855
    [SCAN], 95thPercentileLatency(us), 33375
    [SCAN], 99thPercentileLatency(us), 56895
    [SCAN], Return=OK, 9468

## target 100M, 10K, 840t, 280c, 13m

    $ bin/ycsb run scylla -P workloads/workloade -target 10000 -threads 840 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=30000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p hosts=10.0.2.51,10.0.3.133,10.0.3.67 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 804786
    [OVERALL], Throughput(ops/sec), 9940.530774640712
    [TOTAL_GCS_G1_Young_Generation], Count, 1440
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 2993
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.37190010760624564
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 1440
    [TOTAL_GC_TIME], Time(ms), 2993
    [TOTAL_GC_TIME_%], Time(%), 0.37190010760624564
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2645.075
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2222079
    [CLEANUP], 95thPercentileLatency(us), 3
    [CLEANUP], 99thPercentileLatency(us), 7
    [INSERT], Operations, 400157
    [INSERT], AverageLatency(us), 476.0808707582274
    [INSERT], MinLatency(us), 184
    [INSERT], MaxLatency(us), 213759
    [INSERT], 95thPercentileLatency(us), 1030
    [INSERT], 99thPercentileLatency(us), 1730
    [INSERT], Return=OK, 400157
    [SCAN], Operations, 7599843
    [SCAN], AverageLatency(us), 5456.48627833496
    [SCAN], MinLatency(us), 261
    [SCAN], MaxLatency(us), 334591
    [SCAN], 95thPercentileLatency(us), 12767
    [SCAN], 99thPercentileLatency(us), 18767
    [SCAN], Return=OK, 7599843

## target 1B, 10K, 840t, 280c, 25m

    $ bin/ycsb run scylla -P workloads/workloade  -target 10000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=30000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=false -p hosts=10.0.2.246,10.0.2.17,10.0.2.197 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 1504702
    [OVERALL], Throughput(ops/sec), 9968.751287630375
    [TOTAL_GCS_G1_Young_Generation], Count, 3442
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 6778
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.45045464151705783
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 3442
    [TOTAL_GC_TIME], Time(ms), 6778
    [TOTAL_GC_TIME_%], Time(%), 0.45045464151705783
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2641.9904761904763
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2220031
    [CLEANUP], 95thPercentileLatency(us), 1
    [CLEANUP], 99thPercentileLatency(us), 5
    [INSERT], Operations, 750512
    [INSERT], AverageLatency(us), 512.5631408958151
    [INSERT], MinLatency(us), 182
    [INSERT], MaxLatency(us), 513023
    [INSERT], 95thPercentileLatency(us), 1094
    [INSERT], 99thPercentileLatency(us), 1894
    [INSERT], Return=OK, 750512
    [SCAN], Operations, 14249488
    [SCAN], AverageLatency(us), 6244.357648218659
    [SCAN], MinLatency(us), 284
    [SCAN], MaxLatency(us), 770047
    [SCAN], 95thPercentileLatency(us), 14175
    [SCAN], 99thPercentileLatency(us), 21455
    [SCAN], Return=OK, 14249488
