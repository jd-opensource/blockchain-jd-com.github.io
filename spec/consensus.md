JD Chain抽象了共识协议接口，共识算法可插拔。

已实现`MQ`、`Raft`、`BFT-SMaRt`三种共识算法。

### MQ

已实现基于`RabbitMQ`方案

初始化使用`mq.config`，配置`RabbitMQ`服务及交易、区块等消息队列。

`ledger.init`中设置：
```bash
consensus.service-provider=com.jd.blockchain.consensus.raft.MQConsensusProvider
consensus.conf=.../mq.config
```

参照[初始化基于RabbitMQ的测试网络](../cli/testnet.md#MQ)

### Raft

`JD Chain` 基于[SOFAJRaft](https://github.com/sofastack/sofa-jraft)集成`Raft`共识，感谢优秀的`SOFAJRaft`。

初始化使用`raft.config`，配置共识节点拓扑、`SOFAJRaft`运行参数等。

`ledger.init`中设置：
```bash
consensus.service-provider=com.jd.blockchain.consensus.raft.RaftConsensusProvider
consensus.conf=.../raft.config
```

参照[初始化基于RabbitMQ的测试网络](../cli/testnet.md#Raft)

### BFT-SMaRt

`JD Chain` 基于[BFT-SMaRT](https://github.com/bft-smart/library)深度改造优化，形成了具有高度代码差异的fork版本[JD Chain BFT-SMaRt](https://github.com/blockchain-jd-com/bftsmart)，感谢`BFT-SMaRT`团队。

初始化使用`bftsmart.config`，配置共识节点拓扑、`BFT-SMaRT`运行参数等。

`ledger.init`中设置：
```bash
consensus.service-provider=com.jd.blockchain.consensus.raft.BftsmartConsensusProvider
consensus.conf=.../bftsmart.config
```

参照[初始化基于BFT-SMaRt的测试网络](../cli/testnet.md#BFT-SMaRt)