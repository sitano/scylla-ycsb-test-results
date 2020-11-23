# Test 7 [Scylla, OY, load, 1B]

Scylla YCSB 1, 1B rows, zipfian key distro, workload A
i3.4xlarge, 16 vCPU, 8 cores, 2 threads per core, 128 GB RAM, 3.5TB NVMe md0 of 2 disks
c5.9xlarge loaders.
3 machines cluster, 3 replication factor = 3 * 16 = 48 vCPU
https://github.com/sitano/scylla-cluster-tests/blob/ivan_nostress/test-cases/longevity/longevity-ycsb-a-1B.yaml

$ bin/ycsb run scylla -P workloads/workloada -target 10000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=10000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=true -p hosts=10.0.2.246,10.0.2.17,10.0.2.197

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

$ bin/ycsb run scylla -P workloads/workloada -target 5000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=1000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=true -p hosts=10.0.2.246,10.0.2.17,10.0.2.197

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

