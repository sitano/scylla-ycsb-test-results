# Test 7 [Scylla, OY, F, Z]

Workload F is highly contended. It performs 50% single-row lookups and 50% single-column updates expressed as multi-statement read-modify-write transactions.

## results

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+

## 100M, 80K, 840t, 280c, 47m

    $ bin/ycsb run scylla -P workloads/workloadf -target 80000 -threads 840 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=200000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p hosts=10.0.2.51,10.0.3.133,10.0.3.67 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 2863606
    [OVERALL], Throughput(ops/sec), 69842.01038830062
    [TOTAL_GCS_G1_Young_Generation], Count, 3836
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 11483
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.40099790264442803
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 3836
    [TOTAL_GC_TIME], Time(ms), 11483
    [TOTAL_GC_TIME_%], Time(%), 0.40099790264442803
    [READ], Operations, 200000000
    [READ], AverageLatency(us), 752.76909722
    [READ], MinLatency(us), 150
    [READ], MaxLatency(us), 1917951
    [READ], 95thPercentileLatency(us), 1867
    [READ], 99thPercentileLatency(us), 4005
    [READ], Return=OK, 200000000
    [READ-MODIFY-WRITE], Operations, 100005807
    [READ-MODIFY-WRITE], AverageLatency(us), 1239.7325867686864
    [READ-MODIFY-WRITE], MinLatency(us), 334
    [READ-MODIFY-WRITE], MaxLatency(us), 1907711
    [READ-MODIFY-WRITE], 95thPercentileLatency(us), 3079
    [READ-MODIFY-WRITE], 99thPercentileLatency(us), 5951
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2642.542857142857
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2220031
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 6
    [UPDATE], Operations, 100005807
    [UPDATE], AverageLatency(us), 484.8831648946146
    [UPDATE], MinLatency(us), 146
    [UPDATE], MaxLatency(us), 1903615
    [UPDATE], 95thPercentileLatency(us), 1229
    [UPDATE], 99thPercentileLatency(us), 2777
    [UPDATE], Return=OK, 100005807

## target 1B, 155K, 840t, 280c, 30m

could not keep up with target

    $ bin/ycsb run scylla -P workloads/workloadf -target 155000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=200000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=false -p hosts=10.0.2.246,10.0.2.17,10.0.2.197 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 1828064
    [OVERALL], Throughput(ops/sec), 109405.3599873965
    [TOTAL_GCS_G1_Young_Generation], Count, 3136
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 12674
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.6933017662401316
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 3136
    [TOTAL_GC_TIME], Time(ms), 12674
    [TOTAL_GC_TIME_%], Time(%), 0.6933017662401316
    [READ], Operations, 199996147
    [READ], AverageLatency(us), 6375.491727573131
    [READ], MinLatency(us), 161
    [READ], MaxLatency(us), 5017599
    [READ], 95thPercentileLatency(us), 20799
    [READ], 99thPercentileLatency(us), 127167
    [READ], Return=OK, 199996147
    [READ], Return=ERROR, 3853
    [READ-MODIFY-WRITE], Operations, 99999552
    [READ-MODIFY-WRITE], AverageLatency(us), 8669.133066726139
    [READ-MODIFY-WRITE], MinLatency(us), 341
    [READ-MODIFY-WRITE], MaxLatency(us), 5054463
    [READ-MODIFY-WRITE], 95thPercentileLatency(us), 28063
    [READ-MODIFY-WRITE], 99thPercentileLatency(us), 135423
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2645.265476190476
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2222079
    [CLEANUP], 95thPercentileLatency(us), 4
    [CLEANUP], 99thPercentileLatency(us), 7
    [UPDATE], Operations, 99999552
    [UPDATE], AverageLatency(us), 2197.708654894774
    [UPDATE], MinLatency(us), 133
    [UPDATE], MaxLatency(us), 278271
    [UPDATE], 95thPercentileLatency(us), 9807
    [UPDATE], 99thPercentileLatency(us), 16279
    [UPDATE], Return=OK, 99999552
    [READ-FAILED], Operations, 3853
    [READ-FAILED], AverageLatency(us), 5011966.2725149235
    [READ-FAILED], MinLatency(us), 4988928
    [READ-FAILED], MaxLatency(us), 5070847
    [READ-FAILED], 95thPercentileLatency(us), 5025791
    [READ-FAILED], 99thPercentileLatency(us), 5029887

## target 1B, 100K, 840t, 280c, 33m

this will generate 150K OPS out of 100TPS target (50% reads + 50% RMW)

    $ bin/ycsb run scylla -P workloads/workloadf -target 100000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=200000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p hosts=10.0.0.116,10.0.1.102,10.0.0.12 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 2006725
    [OVERALL], Throughput(ops/sec), 99664.87685158654
    [TOTAL_GCS_G1_Young_Generation], Count, 3902
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 15640
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.7793793369794068
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 3902
    [TOTAL_GC_TIME], Time(ms), 15640
    [TOTAL_GC_TIME_%], Time(%), 0.7793793369794068
    [READ], Operations, 199999693
    [READ], AverageLatency(us), 2270.0136341009284
    [READ], MinLatency(us), 156
    [READ], MaxLatency(us), 4993023
    [READ], 95thPercentileLatency(us), 5935
    [READ], 99thPercentileLatency(us), 20255
    [READ], Return=OK, 199999693
    [READ], Return=ERROR, 307
    [READ-MODIFY-WRITE], Operations, 100006030
    [READ-MODIFY-WRITE], AverageLatency(us), 3431.2099666890085
    [READ-MODIFY-WRITE], MinLatency(us), 327
    [READ-MODIFY-WRITE], MaxLatency(us), 5054463
    [READ-MODIFY-WRITE], 95thPercentileLatency(us), 10255
    [READ-MODIFY-WRITE], 99thPercentileLatency(us), 26559
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2650.209523809524
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2226175
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 9
    [UPDATE], Operations, 100006030
    [UPDATE], AverageLatency(us), 1153.6654244149079
    [UPDATE], MinLatency(us), 145
    [UPDATE], MaxLatency(us), 190079
    [UPDATE], 95thPercentileLatency(us), 4171
    [UPDATE], 99thPercentileLatency(us), 10335
    [UPDATE], Return=OK, 100006030
    [READ-FAILED], Operations, 307
    [READ-FAILED], AverageLatency(us), 5009201.198697069
    [READ-FAILED], MinLatency(us), 4993024
    [READ-FAILED], MaxLatency(us), 5021695
    [READ-FAILED], 95thPercentileLatency(us), 5017599
    [READ-FAILED], 99thPercentileLatency(us), 501759
