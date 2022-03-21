`JD Chain`网络搭建

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
./jdchain-cli.sh testnet config --algorithm ED25519 --ledger-name testnet --password 123456 --peer-zip ../../jdchain-peer-1.6.3.RELEASE.zip --gw-zip ../../jdchain-gateway-1.6.3.RELEASE.zip --consensus BFTSMART --peer-size 4 --init-hosts 127.0.0.1 --peer-hosts 127.0.0.1 --peer-consensus-ports 10080,10082,10084,10086 --peer-manage-ports 7080,7081,7082,7083 --init-ports 8800,8810,8820,8830 --gw-port 8080 --output /home/imuge/jd/nodes/
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

> 启动过程中，相关日志请查阅`peer*/bin/peer.out`，`peer*/logs/peer.log`，`gw/bin/gw.out`，`gw/logs/gw.log`