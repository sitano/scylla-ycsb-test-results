# Test 7 [Scylla, OY, C, Z]

Workload C has no contention. It consists entirely of single-row lookups.

## results

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+

## no target, 100M, 84t, 14c, 13m

    $ bin/ycsb run scylla -P workloads/workloadc -threads 84 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=50000000 -p scylla.coreconnections=14 -p scylla.maxconnections=14 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p hosts=10.0.0.171,10.0.0.142,10.0.1.8 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 785031
    [OVERALL], Throughput(ops/sec), 127383.50460045527
    [TOTAL_GCS_G1_Young_Generation], Count, 1563
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 4825
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.6146254096971966
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 1563
    [TOTAL_GC_TIME], Time(ms), 4825
    [TOTAL_GC_TIME_%], Time(%), 0.6146254096971966
    [READ], Operations, 100000000
    [READ], AverageLatency(us), 654.76897846
    [READ], MinLatency(us), 158
    [READ], MaxLatency(us), 80831
    [READ], 95thPercentileLatency(us), 1245
    [READ], 99thPercentileLatency(us), 1907
    [READ], Return=OK, 100000000
    [CLEANUP], Operations, 84
    [CLEANUP], AverageLatency(us), 26347.369047619046
    [CLEANUP], MinLatency(us), 1
    [CLEANUP], MaxLatency(us), 2213887
    [CLEANUP], 95thPercentileLatency(us), 16
    [CLEANUP], 99thPercentileLatency(us), 28

## target 100M, 140K, 840t, 280c, 35m

    $ bin/ycsb run scylla -P workloads/workloadc -target 140000 -threads 840 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=300000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p hosts=10.0.2.51,10.0.3.133,10.0.3.67 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 2149401
    [OVERALL], Throughput(ops/sec), 139573.76962232732
    [TOTAL_GCS_G1_Young_Generation], Count, 4291
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 10696
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.4976270132934711
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 4291
    [TOTAL_GC_TIME], Time(ms), 10696
    [TOTAL_GC_TIME_%], Time(%), 0.4976270132934711
    [READ], Operations, 300000000
    [READ], AverageLatency(us), 700.5275181066667
    [READ], MinLatency(us), 151
    [READ], MaxLatency(us), 292607
    [READ], 95thPercentileLatency(us), 1604
    [READ], 99thPercentileLatency(us), 3775
    [READ], Return=OK, 300000000
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2647.417857142857
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2224127
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 5

## target 1B, 200K, 840t, 280c, 25m

    $ bin/ycsb run scylla -P workloads/workloadc  -target 200000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=300000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=false -p hosts=10.0.2.246,10.0.2.17,10.0.2.197 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 1504788
    [OVERALL], Throughput(ops/sec), 199363.63128892574
    [TOTAL_GCS_G1_Young_Generation], Count, 2911
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 15320
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 1.0180836104487807
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 2911
    [TOTAL_GC_TIME], Time(ms), 15320
    [TOTAL_GC_TIME_%], Time(%), 1.0180836104487807
    [READ], Operations, 300000000
    [READ], AverageLatency(us), 1736.3166522933334
    [READ], MinLatency(us), 152
    [READ], MaxLatency(us), 356095
    [READ], 95thPercentileLatency(us), 4407
    [READ], 99thPercentileLatency(us), 26383
    [READ], Return=OK, 300000000
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2643.141666666667
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2220031
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 13

## target 1B, 10K, 840t, 280c, LWT, 16m

    $ bin/ycsb run scylla -P workloads/workloadc -target 10000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=10000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=true -p hosts=10.0.2.246,10.0.2.17,10.0.2.197

    [OVERALL], RunTime(ms), 1237758
    [OVERALL], Throughput(ops/sec), 8079.12370592636
    [READ], Operations, 9900410
    [READ], AverageLatency(us), 28474.290777048627
    [READ], MinLatency(us), 1241
    [READ], MaxLatency(us), 1813503
    [READ], 95thPercentileLatency(us), 58975
    [READ], 99thPercentileLatency(us), 871935
    [READ], Return=OK, 9900410
    [READ], Return=ERROR, 99590
    [READ-FAILED], Operations, 99590
    [READ-FAILED], AverageLatency(us), 6656781.148177528
    [READ-FAILED], MinLatency(us), 1956
    [READ-FAILED], MaxLatency(us), 36995071
    [READ-FAILED], 95thPercentileLatency(us), 18808831
    [READ-FAILED], 99thPercentileLatency(us), 24510463

## target 1B, 180K, 1680t, 240c, 27m

    -db site.ycsb.db.scylla.ScyllaCQLClient -s -t -P workloads/workloadc -target 180000 -p operationcount=300000000 -threads 1680 -p recordcount=1000000000 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE -p fieldcount=10 -p fieldlength=128 -p insertcount=1000000000 -p scylla.coreconnections=240 -p scylla.maxconnections=240 -p scylla.username=cassandra -p scylla.password=cassandra -s -p scylla.hosts=10.0.2.178,10.0.0.145,10.0.3.125 -p scylla.tokenaware=true -t

    [OVERALL], RunTime(ms), 1671608
    [OVERALL], Throughput(ops/sec), 179467.91352996635
    [TOTAL_GCS_G1_Young_Generation], Count, 2836
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 15541
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.9297036147230691
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 2836
    [TOTAL_GC_TIME], Time(ms), 15541
    [TOTAL_GC_TIME_%], Time(%), 0.9297036147230691
    [READ], Operations, 298180544
    [READ], AverageLatency(us), 1116.6307644371325
    [READ], MinLatency(us), 152
    [READ], MaxLatency(us), 1671167
    [READ], 95thPercentileLatency(us), 2455
    [READ], 99thPercentileLatency(us), 12135
    [READ], Return=OK, 298180544
    [READ], Return=NOT_FOUND, 1819456
    [CLEANUP], Operations, 1680
    [CLEANUP], AverageLatency(us), 1406.9625
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2363391
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 6
    [READ-FAILED], Operations, 1819456
    [READ-FAILED], AverageLatency(us), 960.4810322426043
    [READ-FAILED], MinLatency(us), 152
    [READ-FAILED], MaxLatency(us), 1333247
    [READ-FAILED], 95thPercentileLatency(us), 1912
    [READ-FAILED], 99thPercentileLatency(us), 10223
