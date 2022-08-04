`JD Chain`网络搭建

`JD Chain`网络由共识节点（`PEER`）和网关节点（`GATEWAY`）组成。
`PEER`节点主要负责共识、交易执行以及全量账本数据库的管理，`GATEWAY`节点负责数据接入、提供区块链数据接口、区块链浏览器等。
多个`PEER`节点组成一个共识网络，可以管理多个账本。
`GATEWAY`节点无状态，不存储区块链数据，`JD Chain SDK`使用`GATEWAY`接口服务与`JD Chain`网络进行数据交互。


## 安装包

访问[资源下载](download)选择并下载指定版本的`jdchain-peer*.zip`、`jdchain-gateway*.zip`安装包。

或者下载[源码](https://github.com/blockchain-jd-com/jdchain)，参照首页说明编译打包指定版本，打包输出目录为`deploy`各子模块`target`。

### `peer`节点安装包目录结构

`jdchain-peer-${version}.zip`，解压完后的安装包结构如下：

- `bin`  相关命令操作目录，使用前请配置*执行权限*
	- `jdchain-cli.sh` 命令行工具
	- `ledger-init.sh` 账本初始化工具
	- `peer-startup.sh` 节点启动脚本
	- `peer-shutdown.sh` 节点关闭脚本
- `config` 配置
	- `init` 初始化配置
		- `ledger.init` 账本配置
		- `local.conf` 本地配置
		- `bftsmart`
			- `bftsmart.config` `BFT-SMaRt`共识初始化配置
		- `raft`
			- `raft.config` `Raft`共识初始化配置
		- `mq`
			- `mq.config` `MQ`共识初始化配置
	- `log4j2-peer.xml` `log4j2`配置，可根据实际情况修改替换
	- `application-peer.properties` `spring`配置
	- `ledger-binding.conf`账本配置，账本初始化成功后会自动生成
- `libs` 项目运行依赖第三方及非`system`依赖包
- `system` 项目运行系统包
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
- `lib` 运行时所需jar包
- `logs` 日志
- `runtime` 解压时不存在，存放运行时数据

## 配置生成

解压`jdchain-peer*.zip`，进入解压后文件`bin`目录下，修改该目录下`.sh`可执行权限。

使用命令行工具[TestNet](cli/testnet)指令快速生成`JD Chain`测试网络初始化配置文件。

如创建四节点，单网关基于`BFT-SMaRt`共识、`KEYPAIR`身份认证模式的单机运行网络：
```bash
./jdchain-cli.sh testnet config --algorithm ED25519 --ledger-name testnet --password 123456 --peer-zip ../../jdchain-peer-1.6.4.RELEASE.zip --gw-zip ../../jdchain-gateway-1.6.4.RELEASE.zip --consensus BFTSMART --peer-size 4 --init-hosts 127.0.0.1 --peer-hosts 127.0.0.1 --peer-consensus-ports 10080,10082,10084,10086 --peer-manage-ports 7080,7081,7082,7083 --init-ports 8800,8810,8820,8830 --gw-port 8080 --output /home/imuge/jd/nodes/
```
> 各参数意义参照[TestNet](cli/testnet)详细描述

执行上述指令，输出如下：
```
INIT CONFIGS FOR LEDGER INITIALIZATION SUCCESS: 

initializer addresses: 127.0.0.1:8800 127.0.0.1:8810 127.0.0.1:8820 127.0.0.1:8830 
peer addresses: 127.0.0.1:7080 127.0.0.1:7081 127.0.0.1:7082 127.0.0.1:7083 
consensus addresses: 127.0.0.1:10080 127.0.0.1:10081 127.0.0.1:10082 127.0.0.1:10083 127.0.0.1:10084 127.0.0.1:10085 127.0.0.1:10086 127.0.0.1:10087 
gateway port: 8080

more details, see /home/imuge/jd/nodes/
```
成功创建账本初始化配置文件，会在`/home/nodes`目录下生成四个`peer`，一个网关`gw`。
后续操作会使用`127.0.0.1:8800 127.0.0.1:8810 127.0.0.1:8820 127.0.0.1:8830`这四个地址执行初始化账本，使用`127.0.0.1:7080 127.0.0.1:7081 127.0.0.1:7082 127.0.0.1:7082`作为`peer`节点的管理服务，使用`127.0.0.1:10080 127.0.0.1:10081 127.0.0.1:10082 127.0.0.1:10083 127.0.0.1:10084 127.0.0.1:10085 127.0.0.1:10086 127.0.0.1:10087`作为`peer`节点的共识服务，使用`8080`端口作为网关服务。

后续操作**请确保上诉地址端口**未被占用

> 关于生成多机器部署配置，请参照[TestNet](cli/testnet)详细描述

## 初始化

> 初始化前**请确保初始化使用的地址端口**未被占用

分别进入四个`peer*/bin`目录下，修改脚本可执行权限，依次执行
```bash
ledger-init.sh
```
按照提示操作（等待所有初始化脚本启动之后，有一个按任意键继续的提示，所有节点都需要点击一下确认键），节点间在初始化过程中会尝试相互连接，指令执行有先后，所有可忽略初始化过程中的连接错误日志。成功执行后会在`peer*/config`目录下生成`ledger-binding.conf`文件，表示账本初始化完成。

若初始化不能成功，请查阅实时控制台输出，以及`peer*/logs/init.log`文件查找原因。

## 启动

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

> JD Chain 1.6.4开始，网关默认添加鉴权，用户名：`jdchain`，密码：`jdchain`。可通过修改网关配置`gw/config/application-gw.properties`修改，修改后重启网关生效
> JD Chain 1.6.4还加入了简单的接口鉴权配置，修改`gw/config/application-gw.properties`中`spring.security.ignored`可以配置开放的接口，修改后重启网关生效。`SDK`使用查询接口的过程中若出现`401`就是由此处权限配置引起。

> 启动过程中，相关日志请查阅`peer*/bin/peer.out`，`peer*/logs/peer.log`，`gw/bin/gw.out`，`gw/logs/gw.log`

至此`JD Chain`部署运行完成，可以参照数据上链部分说明执行数据上链操作。

## 多账本

一个`PEER`节点支持管理多个账本，多个账本间数据数据完全隔离，`PEER`节点启动时会根据`ledger-binding.conf`配置加载账本。

在执行完上面的单账本初始化后，`ledger-binding.conf`中记录的是单个账本的配置信息，修改`peer*/config/init`目录下`bftsmart.config`、`ledger.init`、`local.conf`可再次执行新账本初始化。

### 初始化配置

> 所有`PEER`节点都需要修改
> 以下配置修改仅展示需要修改的字段

1. `bftsmart.config`

修改共识网络`IP`和端口信息，**每个节点的每个账本运行时会占用两个共识相关端口**，如下面配置的节点`0`运行时会占用`10090`、`10091`端口用于共识相关服务。所以单机运行多节点在配置不同节点共识端口时要注意配置的端口冲突。

```properties
############################################
###### #Consensus Participants ######
############################################
#Consensus Participant0
system.server.0.network.host=127.0.0.1
system.server.0.network.port=10090

#Consensus Participant1
system.server.1.network.host=127.0.0.1
system.server.1.network.port=10092

#Consensus Participant2
system.server.2.network.host=127.0.0.1
system.server.2.network.port=10094

#Consensus Participant3
system.server.3.network.host=127.0.0.1
system.server.3.network.port=10096
```

> `bftsmart.config`所有节点配置保持一致

2. `ledger.init`

```properties
#账本的种子；一段16进制字符，最长可以包含64个字符；可以用字符“-”分隔，以便更容易读取；
ledger.seed=ce210778-71a3-4fa8-ab21-975d2bacb755cf2d39e1-5813-41cd-889c-38dc1ce7381e

#账本的描述名称；此属性不参与共识，仅仅在当前参与方的本地节点用于描述用途；
ledger.name=testnet2

#声明的账本创建时间；格式为 “yyyy-MM-dd HH:mm:ss.SSSZ”，表示”年-月-日 时:分:秒:毫秒时区“；例如：“2019-08-01 14:26:58.069+0800”，其中，+0800 表示时区是东8区
created-time=2022-08-05 09:38:55.649+0800
```

> `ledger.init`所有节点配置保持一致

3. `local.conf`

```properties
ledger.db.uri=rocksdb:///home/imuge/jd/nodes/peer0/testnet-db2
```

> `local.conf`所有节点配置均不相同，配置的是不同节点参与方、区块链存储等信息
> 新账本配置的存储一定不能与本地现有账本存储配置相同

### 初始化

再次分别进入四个`peer*/bin`目录下，修改脚本可执行权限，依次执行
```bash
ledger-init.sh
```

成功后查看`ledger-binding.conf`文件，会增加新创建的账本配置信息。

默认情况下`PEER`和`GATEWAY`会自动感知到新账本，刷新区块链浏览器，右上角下拉列表可以切换不同账本。若没有自动感知到，需要重启所有节点和网关。

> 组网出现最多的是端口冲突问题
> 异常情况请查看`peer*/bin/peer.out`，`peer*/logs/peer.log`，`gw/bin/gw.out`，`gw/logs/gw.log`相关日志