`JD Chain`网络搭建

## 压缩包下载

访问[资源下载](download)选择并下载指定版本的`jdchain-peer*.zip`、`jdchain-gateway*.zip`安装包。

或者下载[源码](https://github.com/blockchain-jd-com/jdchain)，参照首页说明编译打包指定版本，打包输出目录为`deploy`各子模块`target`。

### `peer`节点安装包目录结构

`jdchain-peer-${version}.zip`，解压完后的安装包结构如下：

- `bin`  相关命令操作目录，使用前请配置*执行权限*
	- `keygen.sh` 身份生成工具
	- `jdchain-cli.sh` 命令行工具
	- `ledger-init.sh` 账本初始化工具
	- `peer-startup.sh` 节点启动脚本
	- `peer-shutdown.sh` 节点关闭脚本
	- `manager-startup.sh` 管理工具启动脚本
	- `manager-shutdown.sh` 管理工具关闭脚本
- `config` 对应命令的配置目录
	- `init`
		- `ledger.init`
		- `local.conf`
		- `bftsmart.config`
	- `keys` 解压时不存在，会在执行`keygen.sh`脚本时自动创建
		- `*.priv`
		- `*.pub`
		- `*.pwd`
	- `log4j2-peer.xml` `log4j2`配置，可根据实际情况修改替换` 
	- `ledger-binding.conf` 解压时不存在，会在成功执行`ledger-init.sh`脚本后生成
	- `application-peer.properties` `spring`配置
- `docs` 相关文档保存目录
- `libs` 项目运行依赖第三方及非`system`依赖包保存路径
- `system` 项目运行系统包保存路径
- `runtime` 解压时不存在，存放运行时数据
- `manager` 管理工具运行包
- `logs` 日志

### 网关安装包目录结构

``jdchain-gateway-${version}.zip``，解压完后的安装包结构如下：

- `bin`
	- `startup.sh` 启动脚本
	- `shutdown.sh` 停止脚本
- `config`
	- `gateway.conf` 网关配置
	- `log4j2-gw.xml log4j2`配置，可根据实际情况修改替换
	- `application-gw.properties` `spring`配置
- `lib` 运行时所需jar包路径
- `logs` 日志
- `runtime` 解压时不存在，存放运行时数据

## 命令行方式【推荐】

使用命令行工具[testnet](cli/testnet)快速生成、初始化`JD Chain`测试网络。

### 配置生成

解压`jdchain-peer*.zip`，进入解压后文件`bin`目录下，修改该目录下`shell`可执行权限。

创建四节点，单网关基于`BFT-SMaRt`共识、`KEYPAIR`身份认证模式的单机运行网络：
```bash
./jdchain-cli.sh testnet config --algorithm ED25519 --ledger-name testnet --password 123456 --peer-zip jdchain-peer-1.6.0.RELEASE.zip --gw-zip jdchain-gateway-1.6.0.RELEASE.zip --output /home/nodes
```
其中：
- `algorithm`，公私钥算法，支持`ED25519`/`ECDSA`/`RSA`/`SM2`
- `ledger-name`，账本名称
- `password`，秘钥保存密码
- `peer-zip`，`jdchain-peer*.zip`压缩包路径
- `gw-zip`，`jdchain-gateway*.zip`压缩包路径
- `output`，输出目录

执行上述指令，输出如下：
```
INIT CONFIGS FOR LEDGER INITIALIZATION SUCCESS: 

initializer addresses: 127.0.0.1:8800 127.0.0.1:8810 127.0.0.1:8820 127.0.0.1:8830 
peer addresses: 127.0.0.1:7080 127.0.0.1:7081 127.0.0.1:7082 127.0.0.1:7083
consensus addresses: 127.0.0.1:10080 127.0.0.1:10081 127.0.0.1:10082 127.0.0.1:10083 127.0.0.1:10084 127.0.0.1:10085 127.0.0.1:10086 127.0.0.1:10087 
gateway port: 8080

more details, see /home/nodes
```
成功创建账本初始化配置文件，会在`/home/nodes`目录下生成四个`peer`，一个网关`gw`。
后续操作会使用`127.0.0.1:8800 127.0.0.1:8810 127.0.0.1:8820 127.0.0.1:8830`这四个地址执行初始化账本，使用`127.0.0.1:7080 127.0.0.1:7081 127.0.0.1:7082 127.0.0.1:7082`作为`peer`节点的管理服务，使用`127.0.0.1:10080 127.0.0.1:10081 127.0.0.1:10082 127.0.0.1:10083 127.0.0.1:10084 127.0.0.1:10085 127.0.0.1:10086 127.0.0.1:10087`作为`peer`节点的共识服务，使用`8080`端口作为网关服务。

后续操作**请确保上诉地址端口**未被占用

### 初始化

> 初始化前**请确保初始化使用的地址端口**未被占用

分别进入四个`peer*/bin`目录下，修改脚本可执行权限，依次执行
```bash
ledger-init.sh
```
按照提示操作（等待所有初始化脚本启动之后，有一个按任意键继续的提示，所有节点都需要点击一下确认键），节点间在初始化过程中会尝试相互连接，指令执行有先后，所有可忽略初始化过程中的连接错误日志。成功执行后会在`peer*/config`目录下生成`ledger-binding.conf`文件，表示账本初始化完成。

若初始化不能成功，请查阅实时控制台输出，以及`peer*/logs/init.log`文件查找原因。

### 启动

> 启动前**请确保节点的管理服务、共识服务、网关服务使用的地址端口**未被占用

初始化成功后，依次进入`peer*/bin`目录下，执行
```bash
peer-startup.sh
```
启动`peer`节点

进入`gw/bin`目录下，执行
```bash
startup.sh
```
启动网关，启动成功后，访问`localhost:8080`查看区块链浏览器。

> 启动过程中，相关日志请查阅`peer*/bin/peer.out`，`peer*/logs/peer.log`，`gw/bin/gw.out`，`gw/logs/gw.log`


## 基于基础脚本

此方式较为繁琐，但是其他便捷组网方法的基础，有助于了解`JD Chain`各组件的运行方式。

### 注册用户

`JD Chain`以一个 密钥对 标识一个用户，任何区块链操作都必须有用户的签名信息，因此首先需要创建一个用户。
`JD Chain`提供了创建用户的工具，可直接通过命令行生成一个新用户。切换到`bin`路径，执行命令：
```bash
./jdchain-cli keys add -n key-name
```
其中`key-name`是生成的密钥对文件前缀，需自定义。
执行命令过程中需要输入密钥对中 私钥 加密的密码，并选择是否将该密码编码保存（推荐选择保存，该密码编码会保存*.pwd文件中）。

>  注意：请妥善保存该密码，切勿丢失！

执行命令后，该 密钥对 文件会生成到 `config/keys` 路径：
```bash
key-name.priv
key-name.pwd
key-name.pub
```
其中，`key-name.priv`是签名私钥的密文形式内容（以口令哈希作为对称密钥，对签名私钥进行对称加密后的密文数据），`key-name.pub`为签名公钥内容，`key-name`是用于加密签名私钥的口令哈希值。

>  注意：`key-name.pwd`记录的编码在后续`Gateway`配置时会用到！

请将所有`Peer`节点都按照上述流程注册一个独立的用户，在实际生产环境中该过程可能由不同的参与方完成。

### 账本初始化

账本初始化是所有参与`Peer`节点进行共识，并生成初始化账本至数据库的过程。该过程需要所有节点共同参与，同时启动。

1. 初始化配置

`config/init` 目录下有三个配置文件需要修改：

- `local.conf`描述账本初始化的本地（即当前节点）配置；
- `ledger.init`描述账本初始化过程中涉及到的其他参与`Peer`节点配置信息;
- `bftsmart.config`为`BFTSmart`进行共识的相关配置。

`local.conf`：

```properties
#当前参与方的 id
local.parti.id=0

#当前参与方的公钥
local.parti.pubkey=endPsK36koyFr1D245Sa9j83vt6pZUdFBJoJRB3xAsWM6cwhRbna

#当前参与方的私钥（密文编码）
local.parti.privkey=177gjsj5PHeCpbAtJE7qnbmhuZMHAEKuMsd45zHkv8F8AWBvTBbff8yRKdCyT3kwrmAjSnY

#当前参与方的私钥解密密钥(原始口令的一次哈希，Base58格式)，如果不设置，则启动过程中需要从控制台输入
local.parti.pwd=DYu3G8aGTMBW1WrTw76zxQJQU4DHLw9MLyy7peG4LKkY

#账本初始化完成后生成的"账本绑定配置文件"的输出目录
ledger.binding.out=../config

#账本数据库的连接字符，下为rocksdb样例
#rocksdb数据库连接格式：rocksdb://{path}，例如：rocksdb:///export/App08/peer/rocks.db/rocksdb0.db
#redis数据库连接格式：redis://{ip}:{prot}/{db}，例如：redis://127.0.0.1:6379/0
#kvdb数据库连接格式：kvdb://{ip}:{prot}/{db}，例如：kvdb://127.0.0.1:7078/test
ledger.db.uri=rocksdb:///export/app/peer/rocks.db/rocksdb0.db

#账本数据库的连接口令
ledger.db.pwd=
```
其中`local.parti.id`从 `0` 开始编号，且不同`Peer`节点不能相同；`local.parti.privkey`为当前节点私钥，即` config/keys/*.priv` 文件中的内容；其他参数根据实际环境进行配置。

`ledger.init`：

```properties
#账本的种子；一段16进制字符，最长可以包含64个字符；可以用字符“-”分隔，以便更容易读取；
ledger.seed=932dfe23-fe23232f-283f32fa-dd32aa76-8322ca2f-56236cda-7136b322-cb323ffe

#账本的描述名称；此属性不参与共识，仅仅在当前参与方的本地节点用于描述用途；
ledger.name=

#声明的账本创建时间；格式为 “yyyy-MM-dd HH:mm:ss.SSSZ”，表示”年-月-日 时:分:秒:毫秒时区“；例如：“2019-08-01 14:26:58.069+0800”，其中，+0800 表示时区是东8区
created-time=2019-08-01 14:26:58.069+0800

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
consensus.service-provider=com.jd.blockchain.consensus.bftsmart.BftsmartConsensusProvider

#共识服务的参数配置；推荐使用绝对路径；必须；
consensus.conf=bftsmart.config

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

账本初始化过程中，需要其他`Peer`节点的公钥信息进行验证及保存，因此每个节点都需要获取其他`Peer`节点的公钥信息至本地。配置中`cons_parti.N.xxxx`中的`N`需按照实际每个参与方定义的序号配置（该配置为`local.conf`中的`local.parti.id`）。其他参数根据实际环境进行配置。

`bftsmart.config`：

```bash
system.server.0.network.host=127.0.0.1
system.server.0.network.port=16000

system.server.1.network.host=127.0.0.1
system.server.1.network.port=16010

system.servers.num = 4

system.servers.f = 1

system.initial.view = 0,1,2,3
```

>  注意：上述只是将`bftsmart.config`文件中主要需要修改的参数显示，以方便进行相关说明，其他参数的修改可参考具体文件中的具体说明

参数具体说明如下：

- `system.server.$n.network.host`：第`n`个节点的域名；
- `system.server.$n.network.port`：第`n`个节点的共识端口，每个节点会占用**连续**两个端口用于共识通信，请确保配置端口为被占用！
- `system.servers.num`：共识节点总数；
- `system.servers.f`：共识节点最大支持的作恶节点数；
- `system.initial.view`：为每个共识节点分配的`viewID`，每个`viewID`之间使用英文逗号分隔，`viewID`不允许重复，且`viewID`总数需要和`system.servers.num`一致。

2. 初始化部署
上诉步骤配置好四个节点，每个`Peer`节点分别执行：

```bash
./ledger-init.sh
```
控制台会输出类似如下信息：
```bash
------ Web controller of Ledger Initializer[127.0.0.1:30010] was started. ------

Init settings and sign permision...
[PERMISSION_READY] Ledger init permission has already prepared! Any key to continue...
```
根据提示按任意键继续。
如果其它节点没有准备好，控制台会展示`Connection refused`异常信息,不断进行重试操作。目前默认设置为重试`600`次操作，每次间隔时间`3`秒。
执行成功后会在 `config`目录下生成`ledger-binding.conf`文件，该文件即账本初始化生成的文件，`Peer`节点启动时需要依赖该文件。

>  注意：因为`JD Chain`支持多账本形式，若`config/ledger-binding.conf`文件在初始化之前就存在的话，初始化操作后不会覆盖其中的内容，会以追加的方式写入。若第一次创建账本，建议先将该文件删除再进行初始化！

>  注意：`Peer`节点会定时检测`ledger-binding.conf`，有新账本加入时会自动进行更新，不需要重启`Peer`节点！目前默认时间每隔`5`分钟自动更新。

账本初始化成功后，每个`Peer`节点对应的账本相关的数据都会写入到对应的`Rocksdb`中。

### Peer节点启动

`Peer`节点启动依赖于 `config` 目录下`ledger-binding.conf`的配置，启动前请确保区块链网络成功初始化，该文件正确生成。

由于`Peer`节点启动后会自动与其他参与节点进行通信，因此需要同时启动`4`个`Peer`节点，只需要执行`peer-startup.sh`即可，参考命令：

```shell
cd bin
./peer-startup.sh
```

>  `peer-startup.sh`命令中可修改启动端口，默认为：-p 7080。如果在同一台机器上运行多个节点，请确保启动端口不冲突！

>  `peer-startup.sh`命令执行后，会在其所在文件夹中生成对应的`peer.out`启动日志文件，启动成功后日志输出到`logs`目录中

>  `Peer`节点会与账本中涉及到的参与方进行通信，当通信不成功（例如有节点尚未启动）时，会自动进行重试，因此多个`Peer`节点启动可不必完全同时进行

### 网关节点启动

`JD Chain`的网关服务是应用的接入层。
终端接入是`JD Chain`网关的基本功能，在确认终端身份的同时提供连接节点、转发消息和隔离共识节点与客户端等服务。网关确认客户端的合法身份，接收并验证交易；网关根据初始配置文件与对应的共识节点建立连接，并转发交易数据。

网关节点可单独部署，依赖`gateway.conf`配置文件：
```properties
#网关的HTTP服务地址；
http.host=0.0.0.0
#网关的HTTP服务端口；
http.port=8080
#网关的HTTP服务上下文路径，可选；
#http.context-path=

#共识节点的服务地址（与该网关节点连接的Peer节点的IP地址）；
peer.host=127.0.0.1
#共识节点的服务端口（与该网关节点连接的Peer节点的端口，即在Peer节点的peer-startup.sh中定义的端口）；
peer.port=7080
#共识节点的服务是否启用安全证书；
peer.secure=false

#共识节点的服务提供解析器
#BftSmart共识Provider：com.jd.blockchain.consensus.bftsmart.BftsmartConsensusProvider
#简单消息共识Provider：com.jd.blockchain.consensus.mq.MsgQueueConsensusProvider
peer.providers=com.jd.blockchain.consensus.bftsmart.BftsmartConsensusProvider

#数据检索服务对应URL，格式：http://{ip}:{port}，例如：http://127.0.0.1:10001
#若该值不配置或配置不正确，则浏览器模糊查询部分无法正常显示
# data.retrieval.url=http://127.0.0.1:10001
# schema.retrieval.url=http://127.0.0.1:8082

#默认公钥的内容（Base58编码数据）；
keys.default.pubkey=
#默认私钥的路径；在 pk-path 和 pk 之间必须设置其一；
keys.default.privkey-path=
#默认私钥的内容（加密的Base58编码数据）；在 pk-path 和 pk 之间必须设置其一；
keys.default.privkey=
#默认私钥的解码密码；
keys.default.privkey-password=

```

其中：

- `data.retrieval.url`与`schema.retrieval.url`分别与[Argus（高级检索）](https://github.com/blockchain-jd-com/jdchain-indexer)中的[区块链基础数据检索服务](https://github.com/blockchain-jd-com/jdchain-indexer#%E5%90%AF%E5%8A%A8%E5%8C%BA%E5%9D%97%E9%93%BE%E5%9F%BA%E7%A1%80%E6%95%B0%E6%8D%AE%E7%B4%A2%E5%BC%95%E6%A3%80%E7%B4%A2%E6%9C%8D%E5%8A%A1)和[`Schema`服务](https://github.com/blockchain-jd-com/jdchain-indexer#%E5%90%AF%E5%8A%A8value%E7%B4%A2%E5%BC%95%E6%9C%8D%E5%8A%A1)相对应，只有启动并正确配置了`Argus`，`JD Chain`浏览器才能正常使用模糊搜索功能。

- `keys.default`几个参数代表接入`JD Chain`网络的身份信息，只有处于非停用（`DEACTIVATED`）状态的参与方身份才可作为此处的配置。

执行网关`bin`目录如下命令即可启动网关：

```bash
./startup.sh
```

命令执行后，会在其所在文件夹中生成对应的`gw.out`启动日志文件，启动成功后日志会输出到`logs`目录下；
网关节点启动成功后，可通过访问区块链浏览器获取其账本相关信息：http://[ip]:[port] 。

## 更多

1. 基于`RabbitMQ`

以单节点为例

下载可直接初始化使用的[配置](https://jdchain.s3.cn-north-1.jdcloud-oss.com/1.6.2-rabbitmq.zip)，根据实际情况更改账本、公私钥、`RabbitMQ`、数据库配置等信息。

启动`RabbitMQ`：
```bash
$ docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmq rabbitmq:management
```

执行账本初始化：
```bash
$ ./peer0/bin/ledger-init.sh
```

初始化完成后，执行：
```bash
$ ./peer0/bin/peer-startup.sh

$ ./gw/bin/startup.sh
```

即可启动基于`RabbitMQ`的单节点`JD Chain`网络。默认网关浏览器地址：`http://localhost:8080/`。

2. 管理工具

> 不再推荐使用

`JD Chain`提供了基于界面操作的网络初始化启动工具，相关脚本为`manager-startup.sh`和`manager-shutdown.sh`。

点击 [操作视频](https://storage.jd.com/jd.block.chain/init-jdchain-by-manager-tool.mp4) 查看。

> 此方式出现最多的是端口冲突问题，操作过程中出现问题请查看`peer/bin/jump.out`查找原因