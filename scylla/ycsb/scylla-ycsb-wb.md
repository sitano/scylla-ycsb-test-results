# Test 7 [Scylla, OY, B, Z]

Workload B is less contended than Workload A, but still bottlenecks on contention as concurrency grows. It performs 95% single-row lookups and 5% single-column updates.

## results

| Class | TPS | Unreplicated OPS / vCPU                  | Cap | 90%% | 99%% |
|       |     | write_OPS \* RF / vCPU + read_OPS / vCPU |     | ms   | ms   |
+-------+-----+------------------------------------------+-----+------+------+

## no target, 100M, 84t, 14c, 13m

    $ bin/ycsb run scylla -P workloads/workloadb -threads 84 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=50000000 -p scylla.coreconnections=14 -p scylla.maxconnections=14 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p hosts=10.0.0.171,10.0.0.142,10.0.1.8 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 808354
    [OVERALL], Throughput(ops/sec), 123708.17735793971
    [TOTAL_GCS_G1_Young_Generation], Count, 1432
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 6905
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.8542049646565738
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 1432
    [TOTAL_GC_TIME], Time(ms), 6905
    [TOTAL_GC_TIME_%], Time(%), 0.8542049646565738
    [READ], Operations, 94999167
    [READ], AverageLatency(us), 678.9906915394321
    [READ], MinLatency(us), 173
    [READ], MaxLatency(us), 78015
    [READ], 95thPercentileLatency(us), 1290
    [READ], 99thPercentileLatency(us), 1983
    [READ], Return=OK, 94999167
    [CLEANUP], Operations, 84
    [CLEANUP], AverageLatency(us), 26348.011904761905
    [CLEANUP], MinLatency(us), 1
    [CLEANUP], MaxLatency(us), 2213887
    [CLEANUP], 95thPercentileLatency(us), 15
    [CLEANUP], 99thPercentileLatency(us), 26
    [UPDATE], Operations, 5000833
    [UPDATE], AverageLatency(us), 580.6028067723918
    [UPDATE], MinLatency(us), 168
    [UPDATE], MaxLatency(us), 58591
    [UPDATE], 95thPercentileLatency(us), 1017
    [UPDATE], 99thPercentileLatency(us), 1516
    [UPDATE], Return=OK, 5000833

## target 100M, 120K, 840t, 280c, 41m

    $ bin/ycsb run scylla -P workloads/workloadb -target 120000 -threads 840 -p recordcount=100000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=300000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p hosts=10.0.2.51,10.0.3.133,10.0.3.67 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 2504901
    [OVERALL], Throughput(ops/sec), 119765.2122778505
    [TOTAL_GCS_G1_Young_Generation], Count, 4021
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 14735
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.5882468009713757
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 4021
    [TOTAL_GC_TIME], Time(ms), 14735
    [TOTAL_GC_TIME_%], Time(%), 0.5882468009713757
    [READ], Operations, 285003859
    [READ], AverageLatency(us), 497.30278506158754
    [READ], MinLatency(us), 153
    [READ], MaxLatency(us), 189439
    [READ], 95thPercentileLatency(us), 1094
    [READ], 99thPercentileLatency(us), 2263
    [READ], Return=OK, 285003859
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2652.5035714285714
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2228223
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 7
    [UPDATE], Operations, 14996141
    [UPDATE], AverageLatency(us), 390.81666690117146
    [UPDATE], MinLatency(us), 159
    [UPDATE], MaxLatency(us), 131967
    [UPDATE], 95thPercentileLatency(us), 780
    [UPDATE], 99thPercentileLatency(us), 1602
    [UPDATE], Return=OK, 14996141

## target 1B, 120K, 840t, 280c, 34m

    $ bin/ycsb run scylla -P workloads/workloadb  -target 120000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=250000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=false -p hosts=10.0.2.246,10.0.2.17,10.0.2.197 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 2088200
    [OVERALL], Throughput(ops/sec), 119720.33330140791
    [TOTAL_GCS_G1_Young_Generation], Count, 3524
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 11608
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.5558854515850973
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 3524
    [TOTAL_GC_TIME], Time(ms), 11608
    [TOTAL_GC_TIME_%], Time(%), 0.5558854515850973
    [READ], Operations, 237500520
    [READ], AverageLatency(us), 671.6459427962516
    [READ], MinLatency(us), 164
    [READ], MaxLatency(us), 2637823
    [READ], 95thPercentileLatency(us), 1410
    [READ], 99thPercentileLatency(us), 3305
    [READ], Return=OK, 237500520
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2654.9964285714286
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2230271
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 11
    [UPDATE], Operations, 12499480
    [UPDATE], AverageLatency(us), 445.36332303423825
    [UPDATE], MinLatency(us), 157
    [UPDATE], MaxLatency(us), 2635775
    [UPDATE], 95thPercentileLatency(us), 923
    [UPDATE], 99thPercentileLatency(us), 1793
    [UPDATE], Return=OK, 12499480

## target 1B, 200K, 840t, 280c, 10m

    $ bin/ycsb run scylla -P workloads/workloadb  -target 200000 -threads 840 -p recordcount=1000000000 -p fieldcount=10 -p fieldlength=128 -p operationcount=120000000 -p scylla.coreconnections=280 -p scylla.maxconnections=280 -p scylla.username=cassandra -p scylla.password=cassandra -p scylla.tokenaware=true -p scylla.lwt=false -p hosts=10.0.2.246,10.0.2.17,10.0.2.197 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE

    [OVERALL], RunTime(ms), 606697
    [OVERALL], Throughput(ops/sec), 197792.30818678846
    [TOTAL_GCS_G1_Young_Generation], Count, 1178
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 5872
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.9678636947273516
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 1178
    [TOTAL_GC_TIME], Time(ms), 5872
    [TOTAL_GC_TIME_%], Time(%), 0.9678636947273516
    [READ], Operations, 113998622
    [READ], AverageLatency(us), 3412.9072098432907
    [READ], MinLatency(us), 153
    [READ], MaxLatency(us), 525823
    [READ], 95thPercentileLatency(us), 13239
    [READ], 99thPercentileLatency(us), 52191
    [READ], Return=OK, 113998622
    [CLEANUP], Operations, 840
    [CLEANUP], AverageLatency(us), 2649.6785714285716
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2226175
    [CLEANUP], 95thPercentileLatency(us), 3
    [CLEANUP], 99thPercentileLatency(us), 7
    [UPDATE], Operations, 6001378
    [UPDATE], AverageLatency(us), 1557.6483642590085
    [UPDATE], MinLatency(us), 162
    [UPDATE], MaxLatency(us), 475135
    [UPDATE], 95thPercentileLatency(us), 7067
    [UPDATE], 99thPercentileLatency(us), 11159
    [UPDATE], Return=OK, 6001378

## target 1B, 150K, 1680t, 240c, 33m

    -db site.ycsb.db.scylla.ScyllaCQLClient -s -t -P workloads/workloadb -target 150000 -p operationcount=300000000 -threads 1680 -p recordcount=1000000000 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE -p fieldcount=10 -p fieldlength=128 -p insertcount=1000000000 -p scylla.coreconnections=240 -p scylla.maxconnections=240 -p scylla.username=cassandra -p scylla.password=cassandra -s -p scylla.hosts=10.0.2.178,10.0.0.145,10.0.3.125 -p scylla.tokenaware=true -t

    [OVERALL], RunTime(ms), 2005279
    [OVERALL], Throughput(ops/sec), 149605.11729290537
    [TOTAL_GCS_G1_Young_Generation], Count, 3394
    [TOTAL_GC_TIME_G1_Young_Generation], Time(ms), 17566
    [TOTAL_GC_TIME_%_G1_Young_Generation], Time(%), 0.875987830122392
    [TOTAL_GCS_G1_Old_Generation], Count, 0
    [TOTAL_GC_TIME_G1_Old_Generation], Time(ms), 0
    [TOTAL_GC_TIME_%_G1_Old_Generation], Time(%), 0.0
    [TOTAL_GCs], Count, 3394
    [TOTAL_GC_TIME], Time(ms), 17566
    [TOTAL_GC_TIME_%], Time(%), 0.875987830122392
    [READ], Operations, 283080481
    [READ], AverageLatency(us), 867.0984532275116
    [READ], MinLatency(us), 162
    [READ], MaxLatency(us), 799743
    [READ], 95thPercentileLatency(us), 1888
    [READ], 99thPercentileLatency(us), 7435
    [READ], Return=OK, 283080481
    [READ], Return=NOT_FOUND, 1915286
    [CLEANUP], Operations, 1680
    [CLEANUP], AverageLatency(us), 1444.7345238095238
    [CLEANUP], MinLatency(us), 0
    [CLEANUP], MaxLatency(us), 2426879
    [CLEANUP], 95thPercentileLatency(us), 2
    [CLEANUP], 99thPercentileLatency(us), 6
    [UPDATE], Operations, 15004233
    [UPDATE], AverageLatency(us), 593.0385365249927
    [UPDATE], MinLatency(us), 164
    [UPDATE], MaxLatency(us), 618495
    [UPDATE], 95thPercentileLatency(us), 1138
    [UPDATE], 99thPercentileLatency(us), 5711
    [UPDATE], Return=OK, 15004233
    [READ-FAILED], Operations, 1915286
    [READ-FAILED], AverageLatency(us), 650.8000930409348
    [READ-FAILED], MinLatency(us), 162
    [READ-FAILED], MaxLatency(us), 473087
    [READ-FAILED], 95thPercentileLatency(us), 1244
    [READ-FAILED], 99thPercentileLatency(us), 6727

## target 1B, 180K, 1680t, 240c, 27m

    -db site.ycsb.db.scylla.ScyllaCQLClient -s -t -P workloads/workloadb -target 180000 -p operationcount=300000000 -threads 1680 -p recordcount=1000000000 -p scylla.readconsistencylevel=ONE -p scylla.writeconsistencylevel=ONE -p fieldcount=10 -p fieldlength=128 -p insertcount=1000000000 -p scylla.coreconnections=240 -p scylla.maxconnections=240 -p scylla.username=cassandra -p scylla.password=cassandra -s -p scylla.hosts=10.0.2.178,10.0.0.145,10.0.3.125 -p scylla.tokenaware=true -t

    ...
    2020-12-11 15:00:20:658 750 sec: 134554289 operations; 179947.1 current ops/sec; est completion in 15 minutes [READ: Count=1698844, Max=60063, Min=162, Avg=1319.45, 90=2053, 99=16047, 99.9=33887, 99.99=48831] [UPDATE: Count=90047, Max=25951, Min=174, Avg=886.81, 90=1360, 99=8823, 99.9=15135, 99.99=22335] [READ-FAILED: Count=10594, Max=44895, Min=177, Avg=1036.17, 90=1551, 99=12823, 99.9=28367, 99.99=39839] 
    2020-12-11 15:00:30:658 760 sec: 136354817 operations; 180052.8 current ops/sec; est completion in 15 minutes [READ: Count=1700073, Max=42495, Min=161, Avg=1111.34, 90=1863, 99=11239, 99.9=22895, 99.99=31775] [UPDATE: Count=89885, Max=24143, Min=177, Avg=782.07, 90=1198, 99=6911, 99.9=14527, 99.99=22431] [READ-FAILED: Count=10559, Max=33951, Min=157, Avg=859.73, 90=1284, 99=9535, 99.9=20879, 99.99=30255] 
    2020-12-11 15:00:40:658 770 sec: 138154752 operations; 179993.5 current ops/sec; est completion in 15 minutes [READ: Count=1699456, Max=55007, Min=165, Avg=1185.09, 90=1946, 99=12823, 99.9=25823, 99.99=35167] [UPDATE: Count=89867, Max=24447, Min=175, Avg=829.76, 90=1297, 99=7715, 99.9=14879, 99.99=22447] [READ-FAILED: Count=10651, Max=36287, Min=164, Avg=972.29, 90=1531, 99=11431, 99.9=23039, 99.99=35423] 
    2020-12-11 15:00:50:658 780 sec: 139954798 operations; 180004.6 current ops/sec; est completion in 14 minutes [READ: Count=1699757, Max=45215, Min=159, Avg=1100.06, 90=1852, 99=11135, 99.9=22159, 99.99=30095] [UPDATE: Count=89747, Max=25247, Min=171, Avg=781.23, 90=1181, 99=6715, 99.9=15847, 99.99=22031] [READ-FAILED: Count=10520, Max=33663, Min=179, Avg=819.63, 90=1237, 99=8263, 99.9=19391, 99.99=29375] 
    2020-12-11 15:01:00:658 790 sec: 141754822 operations; 180002.4 current ops/sec; est completion in 14 minutes [READ: Count=1699519, Max=43775, Min=163, Avg=1161.72, 90=1934, 99=12407, 99.9=23775, 99.99=32015] [UPDATE: Count=90122, Max=27039, Min=175, Avg=818.58, 90=1259, 99=7299, 99.9=15287, 99.99=21679] [READ-FAILED: Count=10366, Max=30767, Min=167, Avg=908.73, 90=1387, 99=10407, 99.9=22111, 99.99=29663] 
    2020-12-11 15:01:10:658 800 sec: 143474556 operations; 171973.4 current ops/sec; est completion in 14 minutes [READ: Count=1623871, Max=448255, Min=173, Avg=7650.74, 90=4707, 99=171007, 99.9=368127, 99.99=420863] [UPDATE: Count=85906, Max=42143, Min=178, Avg=1505.45, 90=3631, 99=15207, 99.9=28687, 99.99=38431] [READ-FAILED: Count=10002, Max=394239, Min=182, Avg=2977.09, 90=1405, 99=119231, 99.9=245631, 99.99=392959] 
    2020-12-11 15:01:20:658 810 sec: 145354772 operations; 188021.6 current ops/sec; est completion in 14 minutes [READ: Count=1775323, Max=3330047, Min=173, Avg=5060.75, 90=3919, 99=144639, 99.9=207743, 99.99=246911] [UPDATE: Count=93928, Max=34719, Min=170, Avg=1302.29, 90=2985, 99=12695, 99.9=19551, 99.99=24623] [READ-FAILED: Count=10938, Max=206719, Min=154, Avg=2812.52, 90=2095, 99=71359, 99.9=145151, 99.99=204031] 
    2020-12-11 15:01:30:658 820 sec: 147154742 operations; 179997 current ops/sec; est completion in 14 minutes [READ: Count=1699702, Max=45759, Min=154, Avg=1153.26, 90=1953, 99=11927, 99.9=23231, 99.99=31791] [UPDATE: Count=89823, Max=24767, Min=164, Avg=816.58, 90=1357, 99=7099, 99.9=16023, 99.99=21007] [READ-FAILED: Count=10441, Max=31647, Min=179, Avg=917.61, 90=1469, 99=10183, 99.9=19791, 99.99=26255] 
    2020-12-11 15:01:40:658 830 sec: 148954802 operations; 180006 current ops/sec; est completion in 14 minutes [READ: Count=1699979, Max=42911, Min=164, Avg=1121.96, 90=1913, 99=11191, 99.9=22751, 99.99=31423] [UPDATE: Count=89534, Max=22191, Min=176, Avg=780.09, 90=1279, 99=6639, 99.9=14383, 99.99=19935] [READ-FAILED: Count=10518, Max=27727, Min=177, Avg=885.15, 90=1432, 99=9439, 99.9=21519, 99.99=27711] 
    2020-12-11 15:01:50:658 840 sec: 150754747 operations; 179994.5 current ops/sec; est completion in 13 minutes [READ: Count=1699662, Max=49343, Min=161, Avg=1222.78, 90=2042, 99=13503, 99.9=27087, 99.99=39007] [UPDATE: Count=89960, Max=24879, Min=172, Avg=839.11, 90=1393, 99=7471, 99.9=14919, 99.99=20751] [READ-FAILED: Count=10347, Max=40255, Min=175, Avg=959.58, 90=1510, 99=11847, 99.9=23231, 99.99=35999] 
    2020-12-11 15:02:00:658 850 sec: 152552833 operations; 179808.6 current ops/sec; est completion in 13 minutes [READ: Count=1697368, Max=49119, Min=165, Avg=1225.4, 90=2059, 99=13303, 99.9=28127, 99.99=39039] [UPDATE: Count=90203, Max=23231, Min=170, Avg=842.31, 90=1402, 99=7763, 99.9=14695, 99.99=20847] [READ-FAILED: Count=10492, Max=40991, Min=144, Avg=925.36, 90=1452, 99=10591, 99.9=23471, 99.99=40575] 
    2020-12-11 15:02:10:658 860 sec: 154317652 operations; 176481.9 current ops/sec; est completion in 13 minutes [READ: Count=1666974, Max=437503, Min=161, Avg=5986.9, 90=3579, 99=147583, 99.9=335871, 99.99=397567] [UPDATE: Count=87750, Max=88703, Min=174, Avg=1386.84, 90=2661, 99=13551, 99.9=39423, 99.99=78975] [READ-FAILED: Count=10136, Max=375039, Min=181, Avg=2879.87, 90=1577, 99=91391, 99.9=276991, 99.99=342527] 
      ...
