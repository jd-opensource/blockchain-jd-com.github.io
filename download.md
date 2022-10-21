安装包下载，仅提供最近版本下载：

| 文件                                                                                                                    | 校验（SHA-256）  | 更新时间       | 文件大小    |
|-----------------------------------------------------------------------------------------------------------------------| ----  |------------|---------|
| [jdchain-peer-1.6.5.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-peer-1.6.5.RELEASE.zip)       | f44f732c343aaf8003fb1711504917f3b38d8bd897b2fe846ff6d5ffa2e1ddf1 | 2022/10/21 | 115.56M |
| [jdchain-gateway-1.6.5.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-gateway-1.6.5.RELEASE.zip) | de069bbed39f0b7bbd7617b5618c2860a8adb55ac3e19a511b05a5fce4497103 | 2022/10/21 | 114.78M |
| [jdchain-peer-1.6.4.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-peer-1.6.4.RELEASE.zip)       | 299c5d12cf1101dcdb0e056069ab94413582f904464df425c0c2cb70ac75c018 | 2022/05/11 | 114.45M |
| [jdchain-gateway-1.6.4.RELEASE](https://jdchain.s3.cn-north-1.jdcloud-oss.com/jdchain-gateway-1.6.4.RELEASE.zip) | 100a1fd6c63cb14a74bd7bef2b08652e6b04f3a17e6cd08dd3b10b2ff4dde02e | 2022/05/11 | 114.22M |

更多历史版本请编译源码获取

**关于版本升级**

正常情况下版本向下兼容，升级只支持所有节点同时升级的方式。
替换`peer`和`gw`中`bin`、`lib`、`libs`、`system` jar包，移除`runtime/~credentials/`、`runtime/~<ledger-hash>/~credentials/`目录内数据即可。

> 升级后不同版本若存在合约调用失败，请使用最新版本依赖重新打包更新合约。
> 数据无价，升级前做好备份，操作请谨慎！
