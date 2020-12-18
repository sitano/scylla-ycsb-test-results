# Test 7 [Scylla, OY, load, 1B]

Scylla YCSB 1, 1B, zipfian key distro, workload A
i3.4xlarge, 16 vCPU, 8 cores, 2 threads per core, 128 GB RAM, 3.5TB NVMe md0 of 2 disks
c5.9xlarge loader, 32 vCPU, 16 cores.

3 machines cluster, 3 replication factor = 3 * 16 = 48 vCPU
https://github.com/sitano/scylla-cluster-tests/blob/ivan_nostress/test-cases/longevity/longevity-ycsb-a-1B.yaml

1 loader / 1B:

    $ bin/ycsb load cassandra-cql -s -t -P workloads/workloada -threads 84 -p recordcount=1000000000 -p readproportion=0 -p updateproportion=0 -p fieldcount=10 -p fieldlength=128 -p insertstart=0 -p insertcount=1000000000 -p cassandra.coreconnections=14 -p cassandra.maxconnections=14 -p cassandra.username=cassandra -p cassandra.password=cassandra -p hosts=10.0.0.171,10.0.0.142,10.0.1.8 -p cassandra.readconsistencylevel=QUORUM -p cassandra.writeconsistencylevel=QUORUM

Scylla took 3.5 hours to load 1B keys with Original YCSB benchmark Cassandra binding.

DATABASE SIZE ~4.8 TiB - 1.6 TiB a node, TABLE 1

Scylla loaded 4.8TiB of data (before major compaction, 3.9 TiB after)
that represents a 1B keys dataset in about 3 hours and showed
the same performance characteristics as with the smaller dataset.
