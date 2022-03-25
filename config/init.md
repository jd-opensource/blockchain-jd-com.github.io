账本初始化配置文件

- `ledger.init` 账本配置
- `local.conf` 当前节点本地配置
- bftsmart
  -  `bftsmart.config` `BFT-SMaRt`共识配置
- raft
  -  `raft.config` `Raft`共识配置
- mq
  -  `mq.config` `MQ`配置

## ledger.init

```properties
#账本的种子；一段16进制字符，最长可以包含64个字符；可以用字符“-”分隔，以便更容易读取；
ledger.seed=932dfe23-fe23232f-283f32fa-dd32aa76-8322ca2f-56236cda-7136b322-cb323ffe

#账本的描述名称；此属性不参与共识，仅仅在当前参与方的本地节点用于描述用途；
ledger.name=

#身份认证模式：KEYPAIR/CA，默认KEYPAIR即公私钥对模式
identity-mode=KEYPAIR

#账本根证书路径，identity-mode 为 CA 时，此选项不能为空，支持多个，半角逗号相隔
root-ca-path=

#声明的账本创建时间；格式为 “yyyy-MM-dd HH:mm:ss.SSSZ”，表示”年-月-日 时:分:秒:毫秒时区“；例如：“2019-08-01 14:26:58.069+0800”，其中，+0800 表示时区是东8区
created-time=2019-08-01 14:26:58.069+0800

#账本数据底层结构，分为：MERKLE_TREE, KV两种，默认MERKLE_TREE
ledger.data.structure=MERKLE_TREE

#合约运行超时时间，单位：毫秒，默认1分钟
contract.timeout=60000

#-----------------------------------------------
# 初始的角色名称列表；可选项；
# 角色名称不区分大小写，最长不超过20个字符；多个角色名称之间用半角的逗点“,”分隔；
# 系统会预置一个默认角色“DEFAULT”，所有未指定角色的用户都以赋予该角色的权限；若初始化时未配置默认角色的权限，则为默认角色分配所有权限；
#
# 注：如果声明了角色，但未声明角色对应的权限清单，这会忽略该角色的初始化；
#
#security.roles=DEFAULT, ADMIN, MANAGER, GUEST

# 赋予角色的账本权限清单；可选项；
# 可选的权限如下；
# AUTHORIZE_ROLES, SET_CONSENSUS, SET_CRYPTO, REGISTER_PARTICIPANT,
# REGISTER_USER, REGISTER_DATA_ACCOUNT, REGISTER_CONTRACT, UPGRADE_CONTRACT, 
# SET_USER_ATTRIBUTES, WRITE_DATA_ACCOUNT, 
# APPROVE_TX, CONSENSUS_TX
# 多项权限之间用逗点“,”分隔；
# 
#security.role.DEFAULT.ledger-privileges=REGISTER_USER, REGISTER_DATA_ACCOUNT

# 赋予角色的交易权限清单；可选项；
# 可选的权限如下；
# DIRECT_OPERATION, CONTRACT_OPERATION
# 多项权限之间用逗点“,”分隔；
#
#security.role.DEFAULT.tx-privileges=DIRECT_OPERATION, CONTRACT_OPERATION

# 其它角色的配置示例；
# 系统管理员角色：只能操作全局性的参数配置和用户注册，只能执行直接操作指令；
#security.role.ADMIN.ledger-privileges=CONFIGURE_ROLES, AUTHORIZE_USER_ROLES, SET_CONSENSUS, SET_CRYPTO, REGISTER_PARTICIPANT, REGISTER_USER
#security.role.ADMIN.tx-privileges=DIRECT_OPERATION

# 业务主管角色：只能够执行账本数据相关的操作，包括注册用户、注册数据账户、注册合约、升级合约、写入数据等；能够执行直接操作指令和调用合约；
#security.role.MANAGER.ledger-privileges=CONFIGURE_ROLES, AUTHORIZE_USER_ROLES, REGISTER_USER, REGISTER_DATA_ACCOUNT, REGISTER_CONTRACT, UPGRADE_CONTRACT, SET_USER_ATTRIBUTES, WRITE_DATA_ACCOUNT, 
#security.role.MANAGER.tx-privileges=DIRECT_OPERATION, CONTRACT_OPERATION

# 访客角色：不具备任何的账本权限，只有数据读取的操作；也只能够通过调用合约来读取数据；
#security.role.GUEST.ledger-privileges=
#security.role.GUEST.tx-privileges=CONTRACT_OPERATION

#-----------------------------------------------
#共识服务提供者；必须；
#consensus.service-provider
#共识服务的参数配置；推荐使用绝对路径；必须；
#consensus.conf

# BFT共识配置
consensus.service-provider=com.jd.blockchain.consensus.bftsmart.BftsmartConsensusProvider
consensus.conf=bftsmart/bftsmart.config

# RAFT共识配置
#consensus.service-provider=com.jd.blockchain.consensus.raft.RaftConsensusProvider
#consensus.conf=raft/raft.config

# MQ共识配置
#consensus.service-provider=com.jd.blockchain.consensus.mq.MsgQueueConsensusProvider
#consensus.conf=mq/mq.config

#------------------------------------------------

#密码服务提供者列表，以英文逗点“,”分隔；必须；
crypto.service-providers=com.jd.blockchain.crypto.service.classic.ClassicCryptoService, \
com.jd.blockchain.crypto.service.sm.SMCryptoService

#从存储中加载账本数据时，是否校验哈希；可选；
crypto.verify-hash=true

#哈希算法；
crypto.hash-algorithm=SHA256


#参与方的个数，后续以 cons_parti.id 分别标识每一个参与方的配置；
cons_parti.count=4

#---------------------
#第0个参与方的名称；
cons_parti.0.name=
#第0个参与方的公钥文件路径；
cons_parti.0.pubkey-path=
#第0个参与方的公钥内容（由keygen工具生成）；此参数优先于 pubkey-path 参数；
cons_parti.0.pubkey=
#第0个参与方的证书路径，identity-mode 为 CA 时，此选项不能为空
cons_parti.0.ca-path=

#第0个参与方的角色清单；可选项；
#cons_parti.0.roles=ADMIN, MANAGER
#第0个参与方的角色权限策略，可选值有：UNION（并集），INTERSECT（交集）；可选项；
#cons_parti.0.roles-policy=UNION

#第0个参与方的账本初始服务的主机；
cons_parti.0.initializer.host=127.0.0.1
#第0个参与方的账本初始服务的端口；
cons_parti.0.initializer.port=8800
#第0个参与方的账本初始服务是否开启安全连接；
cons_parti.0.initializer.secure=false

#---------------------
#第1个参与方的名称；
cons_parti.1.name=
#第1个参与方的公钥文件路径；
cons_parti.1.pubkey-path=
#第1个参与方的公钥内容（由keygen工具生成）；此参数优先于 pubkey-path 参数；
cons_parti.1.pubkey=
#第1个参与方的证书路径，identity-mode 为 CA 时，此选项不能为空
cons_parti.1.ca-path=

#第1个参与方的角色清单；可选项；
#cons_parti.1.roles=MANAGER
#第1个参与方的角色权限策略，可选值有：UNION（并集），INTERSECT（交集）；可选项；
#cons_parti.1.roles-policy=UNION

#第1个参与方的账本初始服务的主机；
cons_parti.1.initializer.host=127.0.0.1
#第1个参与方的账本初始服务的端口；
cons_parti.1.initializer.port=8810
#第1个参与方的账本初始服务是否开启安全连接；
cons_parti.1.initializer.secure=false

#---------------------
#第2个参与方的名称；
cons_parti.2.name=
#第2个参与方的公钥文件路径；
cons_parti.2.pubkey-path=
#第2个参与方的公钥内容（由keygen工具生成）；此参数优先于 pubkey-path 参数；
cons_parti.2.pubkey=
#第2个参与方的证书路径，identity-mode 为 CA 时，此选项不能为空
cons_parti.2.ca-path=

#第2个参与方的角色清单；可选项；
#cons_parti.2.roles=MANAGER
#第2个参与方的角色权限策略，可选值有：UNION（并集），INTERSECT（交集）；可选项；
#cons_parti.2.roles-policy=UNION

#第2个参与方的账本初始服务的主机；
cons_parti.2.initializer.host=127.0.0.1
#第2个参与方的账本初始服务的端口；
cons_parti.2.initializer.port=8820
#第2个参与方的账本初始服务是否开启安全连接；
cons_parti.2.initializer.secure=false

#---------------------
#第3个参与方的名称；
cons_parti.3.name=
#第3个参与方的公钥文件路径；
cons_parti.3.pubkey-path=
#第3个参与方的公钥内容（由keygen工具生成）；此参数优先于 pubkey-path 参数；
cons_parti.3.pubkey=
#第1个参与方的证书路径，identity-mode 为 CA 时，此选项不能为空
cons_parti.3.ca-path=

#第3个参与方的角色清单；可选项；
#cons_parti.3.roles=GUEST
#第3个参与方的角色权限策略，可选值有：UNION（并集），INTERSECT（交集）；可选项；
#cons_parti.3.roles-policy=INTERSECT

#第3个参与方的账本初始服务的主机；
cons_parti.3.initializer.host=127.0.0.1
#第3个参与方的账本初始服务的端口；
cons_parti.3.initializer.port=8830
#第3个参与方的账本初始服务是否开启安全连接；
cons_parti.3.initializer.secure=false
```

## `local.conf`

```properties
#当前参与方的 id，与ledger.init文件中cons_parti.id一致，默认从0开始
local.parti.id=0

#当前参与方的公钥，用于非证书模式
local.parti.pubkey=
#当前参与方的证书信息，用于证书模式
local.parti.ca-path=

#当前参与方的私钥（密文编码）
local.parti.privkey=
#当前参与方的私钥文件，PEM格式,用于证书模式
local.parti.privkey-path=

#当前参与方的私钥解密密钥(原始口令的一次哈希，Base58格式)，如果不设置，则启动过程中需要从控制台输入;
local.parti.pwd=

#当前参与方的共识服务TLS配置
local.parti.ssl.protocol=
local.parti.ssl.enabled-protocols=
local.parti.ssl.ciphers=
local.parti.ssl.key-store=
local.parti.ssl.key-store-type=
local.parti.ssl.key-alias=
local.parti.ssl.key-store-password=
local.parti.ssl.trust-store=
local.parti.ssl.trust-store-password=
local.parti.ssl.trust-store-type=

#账本初始化完成后生成的"账本绑定配置文件"的输出目录
#推荐使用绝对路径，相对路径以当前文件(local.conf）所在目录为基准
ledger.binding.out=../

#账本数据库的连接字符
#rocksdb数据库连接格式：rocksdb://{path}，例如：rocksdb:///export/App08/peer/rocks.db/rocksdb0.db
#redis数据库连接格式：redis://{ip}:{prot}/{db}，例如：redis://127.0.0.1:6379/0
#kvdb数据库连接格式：kvdb://{ip}:{prot}/{db}，例如：kvdb://127.0.0.1:7078/test
ledger.db.uri=

#账本数据库的连接口令
ledger.db.pwd=
```

## bftsmart.config

```properties

# Copyright (c) 2007-2013 Alysson Bessani, Eduardo Alchieri, Paulo Sousa, and the authors indicated in the @author tags
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

############################################
###### #Consensus Participant0 ######
############################################

system.server.0.network.host=127.0.0.1
system.server.0.network.port=16000
system.server.0.network.secure=false

############################################
###### #Consensus Participant1 ######
############################################

system.server.1.network.host=127.0.0.1
system.server.1.network.port=16010
system.server.1.network.secure=false

############################################
###### #Consensus Participant2 ######
############################################

system.server.2.network.host=127.0.0.1
system.server.2.network.port=16020
system.server.2.network.secure=false

############################################
###### #Consensus Participant3 ######
############################################

system.server.3.network.host=127.0.0.1
system.server.3.network.port=16030
system.server.3.network.secure=false


############################################
####### Communication Configurations #######
############################################

#HMAC algorithm used to authenticate messages between processes (HmacMD5 is the default value)
#This parameter is not currently being used
#system.authentication.hmacAlgorithm = HmacSHA1

#Specify if the communication system should use a thread to send data (true or false)
#system.communication.useSenderThread = true  //unused property;

#Force all processes to use the same public/private keys pair and secret key. This is useful when deploying experiments
#and benchmarks, but must not be used in production systems.
system.communication.defaultkeys = true

############################################
### Replication Algorithm Configurations ###
############################################

#Number of servers in the group
system.servers.num = 4

#Maximum number of faulty replicas
system.servers.f = 1

#Timeout to asking for a client request
#system.totalordermulticast.timeout = 60000

#Allowable time tolerance range(millisecond)
system.totalordermulticast.timeTolerance = 3000000

#Maximum batch size (in number of messages)
system.totalordermulticast.maxbatchsize = 2000

#Number of nonces (for non-determinism actions) generated
system.totalordermulticast.nonces = 10

#if verification of leader-generated timestamps are increasing
#it can only be used on systems in which the network clocks
#are synchronized
system.totalordermulticast.verifyTimestamps = false

#Quantity of messages that can be stored in the receive queue of the communication system
system.communication.inQueueSize = 500000

# Quantity of messages that can be stored in the send queue of each replica
system.communication.outQueueSize = 500000

#The time interval for retrying to send message after connection failure.  In milliseconds;
system.communication.send.retryInterval=2000

#The number of retries to send message after connection failure.
system.communication.send.retryCount=100

#Set to 1 if SMaRt should use signatures, set to 0 if otherwise
system.communication.useSignatures = 0

#Set to 1 if SMaRt should use MAC's, set to 0 if otherwise
system.communication.useMACs = 1

#Set to 1 if SMaRt should use the standard output to display debug messages, set to 0 if otherwise
system.debug = 0

#Print information about the replica when it is shutdown
system.shutdownhook = true

############################################
###### State Transfer Configurations #######
############################################

#Activate the state transfer protocol ('true' to activate, 'false' to de-activate)
system.totalordermulticast.state_transfer = true

#Maximum ahead-of-time message not discarded
system.totalordermulticast.highMark = 50

#Maximum ahead-of-time message not discarded when the replica is still on EID 0 (after which the state transfer is triggered)
system.totalordermulticast.revival_highMark = 10

#Number of ahead-of-time messages necessary to trigger the state transfer after a request timeout occurs
system.totalordermulticast.timeout_highMark = 200

############################################
###### Log and Checkpoint Configurations ###
############################################

system.totalordermulticast.log = true
system.totalordermulticast.log_parallel = false
system.totalordermulticast.log_to_disk = false
system.totalordermulticast.sync_log = false

#Period at which BFT-SMaRt requests the state to the application (for the state transfer state protocol)
system.totalordermulticast.checkpoint_period = 1000
system.totalordermulticast.global_checkpoint_period = 120000

system.totalordermulticast.checkpoint_to_disk = false
system.totalordermulticast.sync_ckp = false


############################################
###### Reconfiguration Configurations ######
############################################

#Replicas ID for the initial view, separated by a comma.
# The number of replicas in this parameter should be equal to that specified in 'system.servers.num'
system.initial.view = 0,1,2,3

#The ID of the trust third party (TTP)
system.ttp.id = 2001

#This sets if the system will function in Byzantine or crash-only mode. Set to "true" to support Byzantine faults
system.bft = true

#Custom View Storage;
#view.storage.handler=bftsmart.reconfiguration.views.DefaultViewStorage

```

## raft.config

```properties
system.server.0.network.host=127.0.0.1
system.server.0.network.port=16000
system.server.0.network.secure=false

system.server.1.network.host=127.0.0.1
system.server.1.network.port=16010
system.server.1.network.secure=false

system.server.2.network.host=127.0.0.1
system.server.2.network.port=16020
system.server.2.network.secure=false

system.server.3.network.host=127.0.0.1
system.server.3.network.port=16030
system.server.3.network.secure=false

system.server.block.max.num=100
system.server.block.max.bytes=4194304

system.server.election.timeout=5000
system.server.snapshot.interval=1800

system.client.configuration.refresh.interval=60000

system.server.rpc.connect.timeout=10000
system.server.rpc.default.timeout=10000
system.server.rpc.snapshot.timeout=300000
system.server.rpc.request.timeout=20000

system.raft.maxByteCountPerRpc=131072
system.raft.maxEntriesSize=1024
system.raft.maxBodySize=524288
system.raft.maxAppendBufferSize=262144
system.raft.maxElectionDelayMs=1000
system.raft.electionHeartbeatFactor=5
system.raft.applyBatch=32
system.raft.sync=true
system.raft.syncMeta=false
system.raft.disruptorBufferSize=16384
system.raft.replicatorPipeline=true
system.raft.maxReplicatorInflightMsgs=256
```

## mq.config

```properties
# MQ连接地址，格式：{MQ类型}://{IP}:{PORT}
system.msg.queue.server=amqp://127.0.0.1:5672

# 当前账本交易发送队列主题（不同账本需不同主题）
system.msg.queue.topic.tx=tx

# 当前账本结块消息应答队列主题
system.msg.queue.topic.tx-result=tx-result

# 当前账本普通消息主题
system.msg.queue.topic.msg=msg

# 当前账本普通消息主题
system.msg.queue.topic.msg-result=msg-result

# 当前账本区块信息主题
system.msg.queue.topic.block=block

# 当前账本结块最大交易数
system.msg.queue.block.txsize=1000

# 当前账本结块最大时长（单位：毫秒），小于等于0时将由system.msg.queue.block.txsize决定出块频率
system.msg.queue.block.maxdelay=10

# 当前账本节点总数
system.servers.num=1

# 当前账本对应节点的公钥信息列表
system.server.0.pubkey=

```

