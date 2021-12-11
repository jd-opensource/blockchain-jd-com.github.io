安装包下载，仅提供最新版本下载：

|  文件   | 校验（SHA-256）  | 更新时间 | 文件大小 |
|  ----  | ----  | ----  | ----  |
| [jdchain-peer-1.6.1.RELEASE](https://storage.jd.com/jd.block.chain/jdchain-peer-1.6.1.RELEASE.zip)  | db03e0ee83d1e34a847790ad21def7a07a644f6803e9b2a318e35cb3db7663b0 | 2021/11/26  | 43.3M  |
| [jdchain-gateway-1.6.1.RELEASE](https://storage.jd.com/jd.block.chain/jdchain-gateway-1.6.1.RELEASE.zip)  | 00462272f3be7bee2306d07460b536be15d177885d96a3cbb358a0a0d4e991c2 | 2021/11/26  | 63.1M  |

历史版本请编译源码获取

**关于版本升级**，理论上1.5.0到1.6.1都可以通过直接移除`peer`中`libs`、`manager`、`system`，`gateway`中`lib`旧依赖，换成新版本依赖，重启所有节点即可。
版本升级时注意下`log4j2`配置、启动脚本等的变化。
升级后不同版本若存在合约调用失败，请使用最新版本依赖重新打包更新合约。
数据无价，升级前做好备份，操作请谨慎！

**关于`log4j2`(漏洞)[https://github.com/apache/logging-log4j2/pull/608]**，针对`1.5.0`、`1.6.0`、`1.6.1`版本提供修复版本：链接地址：http://box.jd.com/sharedInfo/9DD46EF1E198A085EEC946AFBC5707A7  访问密码: lpgjth
