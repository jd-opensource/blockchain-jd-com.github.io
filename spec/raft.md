## Raft共识

Raft 是一种为了管理复制日志的一致性算法。它提供了和 Paxos 算法相同的功能和性能，但是它的算法结构和 Paxos 不同，使得 Raft 算法更加容易理解并且更容易构建实际的系统。


### 复制状态机

![image](https://user-images.githubusercontent.com/3976206/148003926-2c1f4d15-0184-4814-b59a-97c4eba5a461.png)

复制状态机有以下特征:

a. 每一个服务器存储一个包含一系列指令的日志，并且按照日志的顺序进行执行。
b. 每一个日志都按照相同的顺序包含相同的指令，所以每一个服务器都执行相同的指令序列。
c. 由于每个状态机都是确定的，每一次执行操作都产生相同的状态和同样的序列。

### Raft协议

![image](https://user-images.githubusercontent.com/3976206/148004300-e83be819-5829-45ec-8a5f-449f307c0f0f.png)

Raft协议一致性算法分解为以下几个模块:

* Leader Election
* Log Replication
* Membership Change
* Log Compaction

具体内容参加[Raft共识协议](https://raft.github.io/raft.pdf)

### JdChain Raft共识

jdchain中Raft协议使用开源框架[sofajraft](https://github.com/sofastack/sofa-jraft)实现，具体实现如下:
![image](https://user-images.githubusercontent.com/3976206/148005249-d83bd3dc-ba59-41bc-8a74-fbe53883639b.png)


1. 客户端提交交易到网关， 网关根据Raft集群信息将请求转发至Raft Leader节点。 
2. Leader节点对接收到的交易进行打快，并存储到本地日志中。
3. Leader节点将日志复制到其他节点中，当超半数节点成功收到日志后，将日志中的区块信息应用到本地状态机即本地账本中。
4. Leader节点将交易结果返回至客户端
5. Follower节点在接收到Leader节点应用日志之后， 将日志中的区块信息应用到本地状态机中


### JdChain启用Raft共识

#### 初始化时启用Raft共识

JdChain中启用Raft共识，需修改`ledger.init`文件, 将共识提供者选项`consensus.service-provider` 修改为 `com.jd.blockchain.consensus.raft.RaftConsensusProvider`, 
共识服务参数配置`consensus.conf`修改为对应的raft配置文件路径


```
#共识服务提供者；必须；
consensus.service-provider=com.jd.blockchain.consensus.raft.RaftConsensusProvider

#共识服务的参数配置；推荐使用绝对路径；必须；
consensus.conf=raft.config

```

`raft.config` 配置文件说明:

```
# Raft节点地址
system.server.X.network.host=127.0.0.1
# Raft节点共识端口
system.server.X.network.port=16000
# Raft节点共识服务是否启用TLS
system.server.X.network.secure=false
# Raft节点日志存储路径
system.server.X.raft.path=

#Leader节点提议区块时最大交易数
system.server.block.max.num=100

#Leader节点提议区块时区块最大字节数
system.server.block.max.bytes=4194304

#一个 Follower 当超过这个设定时间没有收到 Leader 的消息后，变成 Candidate 节点的时间。
#Leader 会在 timeout 时间内向 Follower 发消息（心跳或者复制日志），如果没有收到，
#Follower 就需要进入 Candidate，发起选举或者等待新的 Leader 出现，默认10秒。
system.server.election.timeout=10000

#自动 Snapshot 间隔时间，默认30分钟
system.server.snapshot.interval=1800

#客户端对Raft集群配置更新时间，默认60秒
system.client.configuration.refresh.interval=60000

#RPC相关配置
system.server.rpc.connect.timeout=10000
system.server.rpc.default.timeout=10000
system.server.rpc.snapshot.timeout=300000
system.server.rpc.request.timeout=20000

#SofaJraft相关参数配置
#节点之间每次文件 RPC (snapshot拷贝）请求的最大大小，默认为 128 K 
system.raft.maxByteCountPerRpc=131072

#从 leader 往 follower 发送的最大日志个数，默认 1024 
system.raft.maxEntriesSize=1024

#从 leader 往 follower 发送日志的最大 body 大小，默认 512K
system.raft.maxBodySize=524288

#日志存储缓冲区最大大小，默认256K
system.raft.maxAppendBufferSize=262144

#选举定时器间隔会在指定时间之外随机的最大范围，默认1秒
system.raft.maxElectionDelayMs=1000

#指定选举超时时间和心跳间隔时间之间的比值, 默认1/5
system.raft.electionHeartbeatFactor=5

# 向 leader 提交的任务累积一个批次刷入日志存储的最大批次大小，默认 32 个任务
system.raft.applyBatch=32

#写入日志、元信息的时候必要的时候调用 fsync，通常都应该为 true
system.raft.sync=true

# 写入 snapshot/raft 元信息是否调用 fsync，默认为 false
system.raft.syncMeta=false

#内部 disruptor buffer 大小，如果是写入吞吐量较高的应用，需要适当调高该值，默认 16384
system.raft.disruptorBufferSize=16384

#是否启用复制的 pipeline 请求优化，默认打开
system.raft.replicatorPipeline=true

#在启用 pipeline 请求情况下，最大 in-flight 请求数，默认256
system.raft.maxReplicatorInflightMsgs=256

```


#### 运行时切换Raft共识

JdChain提供了在区块链运行中切换共识机制的功能，可在`bft`与`raft`共识机制中进行切换。

具体命令为:

```
./jdchain-cli.sh tx switch-consensus --type <consuensu_type> --file <consuensu_config_file>
```

consuensu_type取值有`bft`,`raft`,`mq`

示例:

将运行bft共识的区块链集群共识机制修改为raft共识:

```

./jdchain-cli.sh tx switch-consensus --type raft --file /home/config/test/raft.config

```







