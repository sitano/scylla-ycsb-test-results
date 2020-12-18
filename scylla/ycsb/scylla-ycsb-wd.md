# Test 7 [Scylla, OY, D, Z]

Workload D has no contention. It performs 95% single-row lookups and 5% single-row insertion.

## results

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+

## no target, 100M, 84t, 14c, 13m

    $ bin/ycsb run scylla -P workloads/workloadd -threads 84 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=50000000 -p scylla.coreconnections=14 -p scylla.maxconnections=14 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p hosts=10.0.0.171,10.0.0.142,10.0.1.8 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 786072
    [OVERALL], Throughput(ops/sec), 127214.80983930225
    [TOTAL_GCS_G1_Young_Generation], Count, 1354
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 6706
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.8531025147823609
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 1354
    [TOTAL_GC_TIME], Time(ms), 6706
    [TOTAL_GC_TIME_%], Time(%), 0.8531025147823609
    [READ], Operations, 94473762
    [READ], AverageLatency(us), 656.611916110634
    [READ], MinLatency(us), 169
    [READ], MaxLatency(us), 104895
    [READ], 95thPercentileLatency(us), 1241
    [READ], 99thPercentileLatency(us), 2034
    [READ], Return=OK, 94473762
    [READ], Return=NOT_FOUND, 524791
    [CLEANUP], Operations, 84
    [CLEANUP], AverageLatency(us), 26346.60714285714
    [CLEANUP], MinLatency(us), 1
    [CLEANUP], MaxLatency(us), 2213887
    [CLEANUP], 95thPercentileLatency(us), 10
    [CLEANUP], 99thPercentileLatency(us), 25
    [INSERT], Operations, 5001447
    [INSERT], AverageLatency(us), 630.1672955846578
    [INSERT], MinLatency(us), 185
    [INSERT], MaxLatency(us), 78911
    [INSERT], 95thPercentileLatency(us), 1108
    [INSERT], 99thPercentileLatency(us), 1757
    [INSERT], Return=OK, 5001447
    [READ-FAILED], Operations, 524791
    [READ-FAILED], AverageLatency(us), 608.5764123241443
    [READ-FAILED], MinLatency(us), 167
    [READ-FAILED], MaxLatency(us), 21423
    [READ-FAILED], 95thPercentileLatency(us), 1191
    [READ-FAILED], 99thPercentileLatency(us), 2107

## target 100M, 130K, 840t, 280c, 38m

    $ bin/ycsb run scylla -P workloads/workloadd -target 130000 -threads 840 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=300000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p hosts=10.0.2.51,10.0.3.133,10.0.3.67 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 2314192
    [OVERALL], Throughput(ops/sec), 129634.87904201553
    [TOTAL_GCS_G1_Young_Generation], Count, 4433
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 16298
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.704263086208923
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 4433
    [TOTAL_GC_TIME], Time(ms), 16298
    [TOTAL_GC_TIME_%], Time(%), 0.704263086208923
    [READ], Operations, 284840757
    [READ], AverageLatency(us), 512.9768899645214
    [READ], MinLatency(us), 149
    [READ], MaxLatency(us), 332287
    [READ], 95thPercentileLatency(us), 1244
    [READ], 99thPercentileLatency(us), 3071
    [READ], Return=OK, 284840757
    [READ], Return=NOT_FOUND, 154283
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2644.852380952381
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2222079
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 8
    [INSERT], Operations, 15004960
    [INSERT], AverageLatency(us), 456.8024569209115
    [INSERT], MinLatency(us), 170
    [INSERT], MaxLatency(us), 329215
    [INSERT], 95thPercentileLatency(us), 1119
    [INSERT], 99thPercentileLatency(us), 1809
    [INSERT], Return=OK, 15004960
    [READ-FAILED], Operations, 154283
    [READ-FAILED], AverageLatency(us), 1547.272188121828
    [READ-FAILED], MinLatency(us), 159
    [READ-FAILED], MaxLatency(us), 61151
    [READ-FAILED], 95thPercentileLatency(us), 5331
    [READ-FAILED], 99thPercentileLatency(us), 15279

## target 1B, 180K, 840t, 280c, 27m

    $ bin/ycsb run scylla -P workloads/workloadd  -target 180000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=300000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=false -p hosts=10.0.2.246,10.0.2.17,10.0.2.197 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 1673101
    [OVERALL], Throughput(ops/sec), 179307.76444458522
    [TOTAL_GCS_G1_Young_Generation], Count, 3044
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 12065
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.721116059341307
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 3044
    [TOTAL_GC_TIME], Time(ms), 12065
    [TOTAL_GC_TIME_%], Time(%), 0.721116059341307
    [READ], Operations, 284813396
    [READ], AverageLatency(us), 903.4325045055114
    [READ], MinLatency(us), 147
    [READ], MaxLatency(us), 866815
    [READ], 95thPercentileLatency(us), 2923
    [READ], 99thPercentileLatency(us), 5599
    [READ], Return=OK, 284813396
    [READ], Return=NOT_FOUND, 185391
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2652.4392857142857
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2228223
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 9
    [INSERT], Operations, 15001213
    [INSERT], AverageLatency(us), 682.7971580031561
    [INSERT], MinLatency(us), 175
    [INSERT], MaxLatency(us), 821247
    [INSERT], 95thPercentileLatency(us), 1583
    [INSERT], 99thPercentileLatency(us), 3913
    [INSERT], Return=OK, 15001213
    [READ-FAILED], Operations, 185391
    [READ-FAILED], AverageLatency(us), 1372.155579289178
    [READ-FAILED], MinLatency(us), 161
    [READ-FAILED], MaxLatency(us), 43167
    [READ-FAILED], 95thPercentileLatency(us), 3673
    [READ-FAILED], 99thPercentileLatency(us), 9183

## target 1B, 200K, 840t, 280c, 25m

    $ bin/ycsb run scylla -P workloads/workloadd  -target 200000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=300000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=false -p hosts=10.0.2.246,10.0.2.17,10.0.2.197 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 1532009
    [OVERALL], Throughput(ops/sec), 195821.3039218438
    [TOTAL_GCS_G1_Young_Generation], Count, 3044
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 12284
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.8018229657919764
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 3044
    [TOTAL_GC_TIME], Time(ms), 12284
    [TOTAL_GC_TIME_%], Time(%), 0.8018229657919764
    [READ], Operations, 284998917
    [READ], AverageLatency(us), 3650.270528996431
    [READ], MinLatency(us), 150
    [READ], MaxLatency(us), 583167
    [READ], 95thPercentileLatency(us), 10167
    [READ], 99thPercentileLatency(us), 63871
    [READ], Return=OK, 284998917
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2647.829761904762
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2224127
    [CLEANUP], 95thPercentileLatency(us), 4
    [CLEANUP], 99thPercentileLatency(us), 9
    [INSERT], Operations, 15001083
    [INSERT], AverageLatency(us), 1537.077978703271
    [INSERT], MinLatency(us), 172
    [INSERT], MaxLatency(us), 514047
    [INSERT], 95thPercentileLatency(us), 6127
    [INSERT], 99thPercentileLatency(us), 15591
    [INSERT], Return=OK, 15001083
