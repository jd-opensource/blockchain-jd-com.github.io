`SDK`示例代码参照[GO SDK Samples](https://github.com/blockchain-jd-com/framework-go/blob/master/sdk/test/tx_test.go)

## 连接网关

通过`GatewayServiceFactory`中静态方法连接网关：
```go
type GatewayServiceFactory struct {
    blockchainService BlockchainService
}

// 提供默认签名用户的连接创建，出错直接panic
func MustConnect(gatewayHost string, gatewayPort int, secure bool, userKey ledger_model.BlockchainKeypair) *GatewayServiceFactory

// 提供默认签名用户的连接创建，出错返回错误信息
func Connect(gatewayHost string, gatewayPort int, secure bool, userKey ledger_model.BlockchainKeypair) (*GatewayServiceFactory, error)

// 不提供默认签名用户的连接创建，出错直接panic
func MustConnectWithoutUserKey(gatewayHost string, gatewayPort int, secure bool) *GatewayServiceFactory

// 不提供默认签名用户的连接创建，出错返回错误信息
func ConnectWithoutUserKey(gatewayHost string, gatewayPort int, secure bool) (*GatewayServiceFactory, error)

```

不传`userKey`时需要在提交交易前调用签名操作，添加终端账户签名信息。

通过：
```go
serviceFactory := sdk.MustConnect(GATEWAY_HOST, GATEWAY_PORT, SECURE, NODE_KEY)
service := serviceFactory.GetBlockchainService()
```
创建区块连服务，常规情况`service`**单例**即可。

## 调用过程

### 操作

1. 新建交易：
```java
txTemp := service.NewTransaction(ledgerHashs[0]);
```

2. 操作：
```go
user := sdk.NewBlockchainKeyGenerator().Generate(classic.ED25519_ALGORITHM)
address := framework.GenerateAddress(user.PubKey)
// 注册用户
txTemp.Users().Register(user.GetIdentity())
```

> 一笔交易中可以包含多个操作，所有操作类型参照[交易](spec/transaction.md)文档。

3. 准备交易
```java
prepTx := txTemp.Prepare()
```
4. 终端用户签名
```java
prepTx.Sign(NODE_KEY.AsymmetricKeypair)
```
> 若[连接网关](#连接网关)中传入了用户身份信息，且用户身份具备交易中包含的所有操作[权限](spec/user.md)，此处签名操作可省略。

5. 提交交易

```java
resp, err := prepTx.Commit()
```

6. 交易结果解析

对于有返回值的操作，可通过如下方式解析（此处以字符串类型返回值为例）：
```go
require.Nil(t, err)
require.True(t, resp.Success)
res := resp.OperationResults
require.Equal(t, 1, len(res))
fmt.Println(bytes.ToString(res[0].Result.Bytes))
```

### 查询

`service`中包含了所有链上数据的查询方法，直接使用即可：

```go
ledger, err := service.GetLedger(ledgerHashs[0])
```

与[网关 API](api/gw.md)所提供查询一致，参照[Query APIs](https://github.com/blockchain-jd-com/framework-go/blob/master/sdk/resty_query_service.go)

## 操作类型

按功能模块划分，主要有：[用户操作](spec/user.md#SDK)，[数据账户操作](spec/data-account.md#SDK)，[事件操作](spec/event.md#SDK)，[合约操作](spec/contract.md#SDK)，[数据权限操作](spec/data-permission.md#SDK)。
