JD Chain抽象了存在性和版本数据两类数据操作，存储实现可插拔，支持关系型、非关系型数据库。已适配`RocksDB`、`Redis`以及自研`KVDB`实现。

初始化账本时，由`local.conf`中`ledger.db.uri`配置项设定，不同节点可以使用不同的存储实现。初始化完成后存储配置会记录在`ledger-binding.conf`中。

## RocksDB

`JNI`方式集成`RocksDB`，与节点进程跑在一起。

`ledger.db.uri`配置为`rocksdb://{db-uri}`，如`rocksdb:///home/imuge/jd/nodes/peer0/testnet-db`

`RocksDB`实现支持布隆过滤器、LRU缓存配置，在`{db-uri}`后以参数形式设置：
- bloom={expectedInsertions},{fpp}
- lru={initialCapacity|,{maximumSize}

如：`rocksdb:///home/imuge/jd/nodes/peer0/testnet-db?bloom=100000000,0.01&lru=3000,5000`

可以有效提高存储吞吐


## Redis

`redis://{ip}:{prot}/{db}`

如：`redis://127.0.0.1:6379/0`

## KVDB

`kvdb://{ip}:{prot}/{db}`

如：`kvdb://127.0.0.1:7078/test`