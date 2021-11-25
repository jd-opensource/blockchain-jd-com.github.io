`SDK`可执行示例代码参照[JD Chain Samples](https://github.com/blockchain-jd-com/jdchain-samples)

## 连接网关

通过`GatewayServiceFactory`中静态方法连接网关：
```java

public static GatewayServiceFactory connect(NetworkAddress gatewayAddress)

public static GatewayServiceFactory connect(NetworkAddress gatewayAddress, BlockchainKeypair userKey)

public static GatewayServiceFactory connect(String gatewayHost, int gatewayPort, boolean secure)

public static GatewayServiceFactory connect(String gatewayHost, int gatewayPort, boolean secure, BlockchainKeypair userKey)

public static GatewayServiceFactory connect(String gatewayHost, int gatewayPort, boolean secure, SSLSecurity security)

public static GatewayServiceFactory connect(String gatewayHost, int gatewayPort, boolean secure, BlockchainKeypair userKey, SSLSecurity security)
```

其中`BlockchainKeypair userKey`参数用于自动签署终端用户签名，可不传，不传`userKey`时需要在提交交易前调用签名操作，添加终端账户签名信息；`SSLSecurity`为`TLS`通信相关配置。

通过：
```java
BlockchainService blockchainService = GatewayServiceFactory.connect(gatewayHost, gatewayPort, false).getBlockchainService();
```
创建区块连服务，常规情况`BlockchainService`**单例**即可。

## 调用过程

### 操作

1. 新建交易：
```java
TransactionTemplate txTemp = blockchainService.newTransaction(ledger);
```
2. 操作：
```java
BlockchainKeypair user = BlockchainKeyGenerator.getInstance().generate();
// 注册用户
txTemp.users().register(user.getIdentity());
```
> 一笔交易中可以包含多个操作，所有操作类型参照[交易](spec/transaction.md)文档。
3. 准备交易
```java
PreparedTransaction ptx = txTemp.prepare();
```
4. 终端用户签名
```java
ptx.sign(userKey);
```
> 若[连接网关](#连接网关)中传入了用户身份信息，且用户身份具备交易中包含的所有操作[权限](spec/user.md)，此处签名操作可省略。

5. 提交交易

```java
TransactionResponse response = ptx.commit();
```

6. 交易结果解析

对于有返回值的操作，可通过如下方式解析：
```java
// 解析合约方法调用返回值
for (int i = 0; i < response.getOperationResults().length; i++) {
    BytesValue content = response.getOperationResults()[i].getResult();
    switch (content.getType()) {
        case TEXT:
            System.out.println(content.getBytes().toUTF8String());
            break;
        case INT64:
            System.out.println(BytesUtils.toLong(content.getBytes().toBytes()));
            break;
        case BOOLEAN:
            System.out.println(BytesUtils.toBoolean(content.getBytes().toBytes()[0]));
            break;
        default: // byte[], Bytes
            System.out.println(content.getBytes().toBase58());
            break;
    }
}
```

### 查询

`BlockchainService`中包含了所有链上数据的查询方法，直接使用即可：

```java
LedgerInfo ledgerInfo = blockchainService.getLedger(ledger);
```

与[网关 API](api/gw.md)所提供查询一致，参照[Query Samples](https://github.com/blockchain-jd-com/jdchain-samples/blob/master/sdk-samples/src/test/java/com/jdchain/samples/sdk/QuerySample.java)

## 操作类型

按功能模块划分，主要有：[用户操作](spec/user.md#SDK)，[数据账户操作](spec/data-account.md#SDK)，[事件操作](spec/event.md#SDK)，[合约操作](spec/contract.md#SDK)，[数据权限操作](spec/data-permission.md#SDK)。
