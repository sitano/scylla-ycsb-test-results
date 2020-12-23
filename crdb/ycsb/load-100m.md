# Test 3 [CRDB, CY, load 100M]

CRDB YCSB 1, 100M, to a single node, zipfian key distro, workload A
i3.4xlarge, 16 vCPU, 8 cores, 2 threads per core, 128 GB RAM, 3.5TB NVMe md0 of 2 disks
c5.4xlarge loader.
3 machines cluster, 3 replication factor = 3 * 16 = 48 vCPU

    $ cockroach workload init ycsb 'postgresql://root@127.0.0.1:5000?sslmode=disable' --workload A --insert-count 100000000 --field-count 10 --field-length 128 --drop

In 7 hours this results in a database size of 1.1 TiB that later gets compacted to 450GiB.

    > show create table ycsb.usertable;

    CREATE TABLE usertable (
      ycsb_key VARCHAR(255) NOT NULL,
      field0 STRING NOT NULL,
      field1 STRING NOT NULL,
      field2 STRING NOT NULL,
      field3 STRING NOT NULL,
      field4 STRING NOT NULL,
      field5 STRING NOT NULL,
      field6 STRING NOT NULL,
      field7 STRING NOT NULL,
      field8 STRING NOT NULL,
      field9 STRING NOT NULL,
      CONSTRAINT "primary" PRIMARY KEY (ycsb_key ASC),
      FAMILY fam_0_ycsb_key (ycsb_key),
      FAMILY fam_1_field0 (field0),
      FAMILY fam_2_field1 (field1),
      FAMILY fam_3_field2 (field2),
      FAMILY fam_4_field3 (field3),
      FAMILY fam_5_field4 (field4),
      FAMILY fam_6_field5 (field5),
      FAMILY fam_7_field6 (field6),
      FAMILY fam_8_field7 (field7),
      FAMILY fam_9_field8 (field8),
      FAMILY fam_10_field9 (field9)
    )

    > show zone configuration for table ycsb.usertable;

    CONFIGURE ZONE USING
    range_min_bytes = 134217728,
    range_max_bytes = 536870912,
    gc.ttlseconds = 90000,
    num_replicas = 3,
    constraints = [''],
    lease_preferences = [['']]
    Test 3 [CRDB, CY, load 100M]
