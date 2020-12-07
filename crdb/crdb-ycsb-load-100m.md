Cluster configuration from aws.vars.
Loading with git@github.com:sitano/cockroach.git.

    $ cockroach workload init ycsb 'postgresql://root@127.0.0.1:5000?sslmode=disable' --workload A --insert-count 100000000 --field-count 10 --field-length 128 --drop

In 7 hours this results in a database size of 1.1 TiB that later gets compacted to 450GiB.
