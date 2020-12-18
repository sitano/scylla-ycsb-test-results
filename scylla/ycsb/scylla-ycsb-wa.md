# Test 7 [Scylla, OY, A, Z]

Workload A is highly contended. It performs 50% single-row lookups and 50% single-column updates. Using column families breaks the contention between all updates to different columns of the same row, so we use them by default.

## results

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+
| no target, 100M, 84t, 14c, 7m      | 120000 | ((120000\*0.5)\*3)/42+(120000\*0.5)/42=5714 uOPS/vCPU | 60% | 2   | 4   |
| 100M, CO, 10m, 840t, 240c per host | 120000 | ((120000\*0.5)\*3)/42+(120000\*0.5)/42=5714 uOPS/vCPU | 60% | 2.5 | 4   |
| 100M, CO, 30m, 840t, 240c per host | 120000 | ((120000\*0.5)\*3)/42+(120000\*0.5)/42=5714 uOPS/vCPU | 60% | 3.6 | 8.5 |
| 1B, CO, 40m, 840t, 280c/h          | 120000 | ((120000\*0.5)\*3)/42+(120000\*0.5)/42=5714 uOPS/vCPU | 75% | 3   | 4.6 |
| 1B, CO, 20m, 840t, 280c/h          | 150000 | ((150000\*0.5)\*3)/42+(150000\*0.5)/42=7142 uOPS/vCPU | 80% | 4   | 12  |

## no target, 100M, 84t, 14c, 7m

    $ bin/ycsb run scylla -P workloads/workloada -threads 84 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=50000000 -p scylla.coreconnections=14 -p scylla.maxconnections=14 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.hosts=10.0.0.171,10.0.0.142,10.0.1.8 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 425155
    [OVERALL], Throughput(ops/sec), 117604.16789171008
    [TOTAL_GCS_G1_Young_Generation], Count, 643
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 3057
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.7190318824899155
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 643
    [TOTAL_GC_TIME], Time(ms), 3057
    [TOTAL_GC_TIME_%], Time(%), 0.7190318824899155
    [READ], Operations, 24998319
    [READ], AverageLatency(us), 796.5582386959699
    [READ], MinLatency(us), 175
    [READ], MaxLatency(us), 77887
    [READ], 95thPercentileLatency(us), 1655
    [READ], 99thPercentileLatency(us), 3165
    [READ], Return=OK, 24998319
    [CLEANUP], Operations, 84
    [CLEANUP], AverageLatency(us), 26370.809523809523
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2215935
    [CLEANUP], 95thPercentileLatency(us), 8
    [CLEANUP], 99thPercentileLatency(us), 18
    [UPDATE], Operations, 25001681
    [UPDATE], AverageLatency(us), 616.0169595396405
    [UPDATE], MinLatency(us), 163
    [UPDATE], MaxLatency(us), 58975
    [UPDATE], 95thPercentileLatency(us), 1234
    [UPDATE], 99thPercentileLatency(us), 1813
    [UPDATE], Return=OK, 25001681

## target 100M, 120K, 840t, 280c, 14m

    $ bin/ycsb run scylla -P workloads/workloada -target 120000 -threads 840 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=100000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.hosts=10.0.2.51,10.0.3.133,10.0.3.67 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 840367
    [OVERALL], Throughput(ops/sec), 118995.62929053616
    [TOTAL_GCS_G1_Young_Generation], Count, 1498
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 5887
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.7005272696333864
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 1498
    [TOTAL_GC_TIME], Time(ms), 5887
    [TOTAL_GC_TIME_%], Time(%), 0.7005272696333864
    [READ], Operations, 49997770
    [READ], AverageLatency(us), 751.7650151996779
    [READ], MinLatency(us), 169
    [READ], MaxLatency(us), 272127
    [READ], 95thPercentileLatency(us), 1921
    [READ], 99thPercentileLatency(us), 4115
    [READ], Return=OK, 49997770
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2650.079761904762
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2226175
    [CLEANUP], 95thPercentileLatency(us), 3
    [CLEANUP], 99thPercentileLatency(us), 10
    [UPDATE], Operations, 50002230
    [UPDATE], AverageLatency(us), 514.0513192711605
    [UPDATE], MinLatency(us), 151
    [UPDATE], MaxLatency(us), 273151
    [UPDATE], 95thPercentileLatency(us), 1245
    [UPDATE], 99thPercentileLatency(us), 2573
    [UPDATE], Return=OK, 50002230

## target 100M, 120K, 840t, 280c, 42m

    $ bin/ycsb run scylla -P workloads/workloada -target 120000 -threads 840 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=300000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.hosts=10.0.2.51,10.0.3.133,10.0.3.67 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 2507060
    [OVERALL], Throughput(ops/sec), 119662.07430217067
    [TOTAL_GCS_G1_Young_Generation], Count, 3899
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 14690
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.5859452904996291
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 3899
    [TOTAL_GC_TIME], Time(ms), 14690
    [TOTAL_GC_TIME_%], Time(%), 0.5859452904996291
    [READ], Operations, 149998494
    [READ], AverageLatency(us), 1219.179118018345
    [READ], MinLatency(us), 168
    [READ], MaxLatency(us), 5009407
    [READ], 95thPercentileLatency(us), 3673
    [READ], 99thPercentileLatency(us), 8447
    [READ], Return=OK, 149998494
    [READ], Return=ERROR, 485
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2654.7571428571428
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2230271
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 5
    [UPDATE], Operations, 150001021
    [UPDATE], AverageLatency(us), 808.0834112855805
    [UPDATE], MinLatency(us), 150
    [UPDATE], MaxLatency(us), 4067327
    [UPDATE], 95thPercentileLatency(us), 2235
    [UPDATE], 99thPercentileLatency(us), 5963
    [UPDATE], Return=OK, 150001021
    [READ-FAILED], Operations, 485
    [READ-FAILED], AverageLatency(us), 5008027.183505154
    [READ-FAILED], MinLatency(us), 4993024
    [READ-FAILED], MaxLatency(us), 5029887
    [READ-FAILED], 95thPercentileLatency(us), 5013503
    [READ-FAILED], 99thPercentileLatency(us), 5017599

## target 1B, 120K, 840t, 280c, 42m

    $ bin/ycsb run scylla -P workloads/workloada -target 120000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=300000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=false -p scylla.hosts=10.0.2.246,10.0.2.17,10.0.2.197 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 2506635
    [OVERALL], Throughput(ops/sec), 119682.36300857524
    [TOTAL_GCS_G1_Young_Generation], Count, 3702
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 15061
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.6008453564240506
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 3702
    [TOTAL_GC_TIME], Time(ms), 15061
    [TOTAL_GC_TIME_%], Time(%), 0.6008453564240506
    [READ], Operations, 150004642
    [READ], AverageLatency(us), 948.1402700724421
    [READ], MinLatency(us), 168
    [READ], MaxLatency(us), 209023
    [READ], 95thPercentileLatency(us), 2947
    [READ], 99thPercentileLatency(us), 4615
    [READ], Return=OK, 150004642
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2647.6154761904763
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2224127
    [CLEANUP], 95thPercentileLatency(us), 3
    [CLEANUP], 99thPercentileLatency(us), 8
    [UPDATE], Operations, 149995358
    [UPDATE], AverageLatency(us), 548.3967859192015
    [UPDATE], MinLatency(us), 126
    [UPDATE], MaxLatency(us), 211583
    [UPDATE], 95thPercentileLatency(us), 1224
    [UPDATE], 99thPercentileLatency(us), 2177
    [UPDATE], Return=OK, 149995358

## target 1B, 10K, 840t, 280c, LWT, 16m

    $ bin/ycsb run scylla -P workloads/workloada -target 10000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=10000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=true -p scylla.hosts=10.0.2.246,10.0.2.17,10.0.2.197

3.10.1-scylla-0 (no LWT optimization)

    [OVERALL], RunTime(ms), 1022705
    [OVERALL], Throughput(ops/sec), 9777.990720686807
    [TOTAL_GCS_G1_Young_Generation], Count, 199
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 483
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.047227695180917274
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 199
    [TOTAL_GC_TIME], Time(ms), 483
    [TOTAL_GC_TIME_%], Time(%), 0.047227695180917274
    [READ], Operations, 4863853
    [READ], AverageLatency(us), 43234.82885050186
    [READ], MinLatency(us), 1268
    [READ], MaxLatency(us), 1915903
    [READ], 95thPercentileLatency(us), 133119
    [READ], 99thPercentileLatency(us), 970239
    [READ], Return=OK, 4863853
    [READ], Return=ERROR, 135846
    [UPDATE-FAILED], Operations, 137774
    [UPDATE-FAILED], AverageLatency(us), 1140439.2397114115
    [UPDATE-FAILED], MinLatency(us), 2038
    [UPDATE-FAILED], MaxLatency(us), 7217151
    [UPDATE-FAILED], 95thPercentileLatency(us), 2004991
    [UPDATE-FAILED], 99thPercentileLatency(us), 3678207
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2649.525
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2226175
    [CLEANUP], 95thPercentileLatency(us), 3
    [CLEANUP], 99thPercentileLatency(us), 6
    [UPDATE], Operations, 4862527
    [UPDATE], AverageLatency(us), 43272.82207543526
    [UPDATE], MinLatency(us), 1318
    [UPDATE], MaxLatency(us), 1905663
    [UPDATE], 95thPercentileLatency(us), 132479
    [UPDATE], 99thPercentileLatency(us), 970751
    [UPDATE], Return=OK, 4862527
    [UPDATE], Return=ERROR, 137774
    [READ-FAILED], Operations, 135846
    [READ-FAILED], AverageLatency(us), 1147098.0586693757
    [READ-FAILED], MinLatency(us), 1938
    [READ-FAILED], MaxLatency(us), 7167999
    [READ-FAILED], 95thPercentileLatency(us), 2038783
    [READ-FAILED], 99thPercentileLatency(us), 3729407

## target 1B, 5K, 840t, 280c, LWT, 33m

    $ bin/ycsb run scylla -P workloads/workloada -target 5000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=1000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=true -p scylla.hosts=10.0.2.246,10.0.2.17,10.0.2.197

3.10.1-scylla-0 (no LWT optimization)

    [OVERALL], RunTime(ms), 205112
    [OVERALL], Throughput(ops/sec), 4875.385155427279
    [TOTAL_GCS_G1_Young_Generation], Count, 21
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 112
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.054604313740785525
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 21
    [TOTAL_GC_TIME], Time(ms), 112
    [TOTAL_GC_TIME_%], Time(%), 0.054604313740785525
    [READ], Operations, 498348
    [READ], AverageLatency(us), 4676.081210318894
    [READ], MinLatency(us), 1221
    [READ], MaxLatency(us), 474879
    [READ], 95thPercentileLatency(us), 6087
    [READ], 99thPercentileLatency(us), 81791
    [READ], Return=OK, 498348
    [READ], Return=ERROR, 903
    [UPDATE-FAILED], Operations, 912
    [UPDATE-FAILED], AverageLatency(us), 32673.577850877195
    [UPDATE-FAILED], MinLatency(us), 1906
    [UPDATE-FAILED], MaxLatency(us), 352255
    [UPDATE-FAILED], 95thPercentileLatency(us), 162559
    [UPDATE-FAILED], 99thPercentileLatency(us), 251007
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2648.3690476190477
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2224127
    [CLEANUP], 95thPercentileLatency(us), 4
    [CLEANUP], 99thPercentileLatency(us), 16
    [UPDATE], Operations, 499837
    [UPDATE], AverageLatency(us), 4728.5843344930445
    [UPDATE], MinLatency(us), 1266
    [UPDATE], MaxLatency(us), 445183
    [UPDATE], 95thPercentileLatency(us), 6127
    [UPDATE], 99thPercentileLatency(us), 81599
    [UPDATE], Return=OK, 499837
    [UPDATE], Return=ERROR, 912
    [READ-FAILED], Operations, 903
    [READ-FAILED], AverageLatency(us), 28861.203765227023
    [READ-FAILED], MinLatency(us), 1939
    [READ-FAILED], MaxLatency(us), 363519
    [READ-FAILED], 95thPercentileLatency(us), 141311
    [READ-FAILED], 99thPercentileLatency(us), 222591

## target 1B, 150K, 840t, 280c, 22m

    $ bin/ycsb run scylla -P workloads/workloada -target 150000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=200000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.hosts=10.0.0.116,10.0.1.102,10.0.0.12 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 1340102
    [OVERALL], Throughput(ops/sec), 149242.37110309512
    [TOTAL_GCS_G1_Young_Generation], Count, 2429
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 8676
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.6474134058452267
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 2429
    [TOTAL_GC_TIME], Time(ms), 8676
    [TOTAL_GC_TIME_%], Time(%), 0.6474134058452267
    [READ], Operations, 99989953
    [READ], AverageLatency(us), 1839.2275601629697
    [READ], MinLatency(us), 159
    [READ], MaxLatency(us), 4997119
    [READ], 95thPercentileLatency(us), 4627
    [READ], 99thPercentileLatency(us), 12687
    [READ], Return=OK, 99989953
    [READ], Return=ERROR, 34
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2647.7738095238096
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2224127
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 9
    [UPDATE], Operations, 100010013
    [UPDATE], AverageLatency(us), 941.8987527878834
    [UPDATE], MinLatency(us), 136
    [UPDATE], MaxLatency(us), 243711
    [UPDATE], 95thPercentileLatency(us), 2635
    [UPDATE], 99thPercentileLatency(us), 6755
    [UPDATE], Return=OK, 100010013
    [READ-FAILED], Operations, 34
    [READ-FAILED], AverageLatency(us), 5008926.117647059
    [READ-FAILED], MinLatency(us), 4997120
    [READ-FAILED], MaxLatency(us), 5017599
    [READ-FAILED], 95thPercentileLatency(us), 5017599
    [READ-FAILED], 99thPercentileLatency(us), 5017599

## target 1B, 150K, 840t, 280c, data integrity, 22m

    $ bin/ycsb run scylla -P workloads/workloada -target 150000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=200000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.hosts=10.0.0.116,10.0.1.102,10.0.0.12 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE -p dataintegrity=true

    [OVERALL], RunTime(ms), 1338203
    [OVERALL], Throughput(ops/sec), 149454.1560585352
    [TOTAL_GCS_G1_Young_Generation], Count, 2297
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 9090
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.6792691392860426
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 2297
    [TOTAL_GC_TIME], Time(ms), 9090
    [TOTAL_GC_TIME_%], Time(%), 0.6792691392860426
    [READ], Operations, 100003650
    [READ], AverageLatency(us), 2874.663277240381
    [READ], MinLatency(us), 163
    [READ], MaxLatency(us), 4931583
    [READ], 95thPercentileLatency(us), 6571
    [READ], 99thPercentileLatency(us), 27807
    [READ], Return=OK, 100003650
    [READ], Return=ERROR, 42
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2645.195238095238
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2222079
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 9
    [UPDATE], Operations, 99996308
    [UPDATE], AverageLatency(us), 1241.1170748724044
    [UPDATE], MinLatency(us), 144
    [UPDATE], MaxLatency(us), 3188735
    [UPDATE], 95thPercentileLatency(us), 4163
    [UPDATE], 99thPercentileLatency(us), 11175
    [UPDATE], Return=OK, 99996308
    [VERIFY], Operations, 100003692
    [VERIFY], AverageLatency(us), 12.45315773941626
    [VERIFY], MinLatency(us), 0
    [VERIFY], MaxLatency(us), 327679
    [VERIFY], 95thPercentileLatency(us), 26
    [VERIFY], 99thPercentileLatency(us), 38
    [VERIFY], Return=OK, 46427192
    [VERIFY], Return=UNEXPECTED_STATE, 53576458
    [VERIFY], Return=ERROR, 42
    [READ-FAILED], Operations, 42
    [READ-FAILED], AverageLatency(us), 5010480.761904762
    [READ-FAILED], MinLatency(us), 5001216
    [READ-FAILED], MaxLatency(us), 5021695
    [READ-FAILED], 95thPercentileLatency(us), 5017599
    [READ-FAILED], 99thPercentileLatency(us), 5021695
