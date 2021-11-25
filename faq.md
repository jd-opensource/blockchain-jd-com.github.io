### 源码拉取构建

`SSH`方式拉取源码请在`github`上正确配置`SSH Keys`

### KV并发写入

`JD Chain`在写入`kv`时要求填写当前最高数据版本`version`信息，也就是在写入`kv`前需要查询一下最新的数据版本，无法满足并发情况。解决方案是在合约方法中通过上下文`getUncommittedLedger`访问账本运行时所有待提交数据获取最新数据版本，再写入数据的方式。代码参考：https://github.com/blockchain-jd-com/jdchain/blob/76f8eda3eae27d83a169c183c237f775ab9938b7/samples/contract-samples/src/main/java/com/jdchain/samples/contract/SampleContractImpl.java#L28

事件并发发布类似

### NoClassDefFoundError: sun/jvmstat/monitor/MonitoredHost

jdk版本等原因，出现这个问题是因为toools.jar找不到，需要将jdk下 lib/tools.jar 复制到每一个peer的libs目录下，并重新启动项目。参考[issue76](https://github.com/blockchain-jd-com/jdchain/issues/76)

### 重复交易

`JD Chain`交易哈希是对交易内容哈希，交易时间戳精度为`ms`，在并发情况下，容易出现交易哈希重复情况。如并发调用同一个合约方法，且参数不变，服务端会报交易重复错误。解决方案是存在类似使用场景时，合约方法增加随机冗余参数，可以每次生成不一样的交易哈希，规避交易重复问题。

### 1.5.0升级1.6.0

停机情况下替换最新1.6.0版本相关依赖包，链上合约需要更新依赖重新打包升级。