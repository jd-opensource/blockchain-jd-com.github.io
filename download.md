安装包下载，仅提供最近版本下载：

|  文件   | 校验（SHA-256）  | 更新时间 | 文件大小 |
|  ----  | ----  | ----  | ----  |
| [jdchain-peer-1.6.1.RELEASE](https://storage.jd.com/jd.block.chain/jdchain-peer-1.6.1.RELEASE-1211.zip)  | 7b6e2ca482b4d84533cc3839968f8590634646f44854bf780ec89f25626945eb | 2021/12/11  | 43.3M  |
| [jdchain-gateway-1.6.1.RELEASE](https://storage.jd.com/jd.block.chain/jdchain-gateway-1.6.1.RELEASE-1211.zip)  | 3df90d580ee7a51507549518e052831cc69379b8d1b62175cfd92d6185e8c2f0 | 2021/12/11  | 63.1M  |
| [jdchain-peer-1.6.0.RELEASE](https://storage.jd.com/jd.block.chain/jdchain-peer-1.6.0.RELEASE.zip)  | bcb4ff3bd99660d2f44741d764bbd1f04abf8869e676b6e9ffef359dff3f5cef | 2021/12/11  | 43.3M  |
| [jdchain-gateway-1.6.0.RELEASE](https://storage.jd.com/jd.block.chain/jdchain-gateway-1.6.0.RELEASE.zip)  | 0857c73b9abeebc8298c4815dc0a5b8d832079ce1cdc4fc807c6f0d427cd0b5a | 2021/12/11  | 63.1M  |
| [jdchain-peer-1.5.0.RELEASE](https://storage.jd.com/jd.block.chain/jdchain-peer-1.5.0.RELEASE.zip)  | 4604a3b322731944993732711396e208611f5bf726f9fed6a8cdb4ba7ff3217b | 2021/12/11  | 43.3M  |
| [jdchain-gateway-1.5.0.RELEASE](https://storage.jd.com/jd.block.chain/jdchain-gateway-1.5.0.RELEASE.zip)  | c8bbe9fa1e8f79392bb0c78411833b272e033644a68ba50e1049e9797d0ec3d6 | 2021/12/11  | 63.1M  |

历史版本请编译源码获取

**关于版本升级**，理论上1.5.0到1.6.1都可以通过直接移除`peer`中`libs`、`manager`、`system`，`gateway`中`lib`旧依赖，换成新版本依赖，重启所有节点即可。
版本升级时注意下`log4j2`配置、启动脚本等的变化。
升级后不同版本若存在合约调用失败，请使用最新版本依赖重新打包更新合约。
数据无价，升级前做好备份，操作请谨慎！

**关于log4j2漏洞**，[Restrict LDAP access via JNDI](https://github.com/apache/logging-log4j2/pull/608)，针对`1.5.0`、`1.6.0`、`1.6.1`版本提供修复安装包，对应release版本已更新。
