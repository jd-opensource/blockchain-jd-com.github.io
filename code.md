### JD Chain 源代码库

请使用`JDK` `1.8`

#### jdchain

- 仓库地址：https://github.com/blockchain-jd-com/jdchain
- 说明：`JD Chain`主项目，使用`git modules`管理所有子模块。

> 主项目下仅支持使用官方提供的指令进行源码拉取、编译打包。子模块的编译打包需要到具体模块代码目录下执行。

#### project

- 仓库地址：https://github.com/blockchain-jd-com/jdchain-project
- 说明：公共的父项目，定义公共的依赖

#### framework 

- 仓库地址：https://github.com/blockchain-jd-com/jdchain-framework
- 说明：框架源码库，定义公共数据类型、框架、模块组件接口、`SDK`、`SPI`、工具

#### core

- 仓库地址：https://github.com/blockchain-jd-com/jdchain-core
- 说明：模块组件实现的源码库

#### explorer

- 仓库地址：https://github.com/blockchain-jd-com/explorer
- 说明：相关产品的前端模块的源码库

#### bft-smart

- 仓库地址：https://github.com/blockchain-jd-com/bftsmart
- 说明：`BFT-SMaRt` 共识算法的源码库

#### binary-proto

- 仓库地址：https://github.com/blockchain-jd-com/binary-proto
- 说明：自研序列化/反序列化框架

#### utils

- 仓库地址：https://github.com/blockchain-jd-com/utils.git
- 说明：工具类库

#### httpservice

- 仓库地址：https://github.com/blockchain-jd-com/httpservice
- 说明：`HTTP` `RPC` 服务框架

#### kvdb

- 仓库地址：https://github.com/blockchain-jd-com/kvdb.git
- 说明：基于`RocksDB` `JNI`方式实现可独立部署，支持`KV`读写的数据库服务

#### samples

- 仓库地址: https://github.com/blockchain-jd-com/jdchain-samples
- 说明：`JD Chain`合约和`SDK`使用样例

#### indexer

- 仓库地址: https://github.com/blockchain-jd-com/jdchain-indexer
- 说明：高级检索，基于`dgraph`实现对数据账户中`value`穿透检索

#### go-sdk

- 仓库地址: https://github.com/blockchain-jd-com/framework-go
- 说明：`GO` `SDK`

#### test

- 仓库地址：https://github.com/blockchain-jd-com/jdchain-test
- 说明：集成测试用例的源码库