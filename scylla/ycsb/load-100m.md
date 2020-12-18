# Test 7 [Scylla, OY, load, 100M]

Scylla YCSB 1, 100M, zipfian key distro, workload A
i3.4xlarge, 16 vCPU, 8 cores, 2 threads per core, 128 GB RAM, 3.5TB NVMe md0 of 2 disks
c5.9xlarge loaders.
3 machines cluster, 3 replication factor = 3 * 16 = 48 vCPU
https://github.com/sitano/scylla-cluster-tests/blob/ivan_nostress/test-cases/longevity/longevity-ycsb-a-100M.yaml

Scylla took 20 minutes to load 100M keys with Original YCSB benchmark Cassandra binding.

    $ bin/ycsb load cassandra-cql -P workloads/workloada -threads 84 -p recordcount=100000000 -p readproportion=0 -p updateproportion=0 -p fieldcount=10 -p fieldlength=128 -p insertstart=0 -p insertcount=100000000 -p cassandra.coreconnections=14 -p cassandra.maxconnections=14 -p cassandra.username=cassandra -p cassandra.password=cassandra -s  -p hosts=10.0.0.171,10.0.0.142,10.0.1.8 -p cassandra.readconsistencylevel=QUORUM -p cassandra.writeconsistencylevel=QUORUM

DATABASE SIZE 300 GiB - 100 GiB a node
TABLE 1

20 minutes.
