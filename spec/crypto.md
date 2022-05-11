JD Chain统一了密码算法接口、公私钥编码规范，密码算法可插拔，本文档主要介绍已实现的传统、国密及高级密码算法及其应用。

## 传统密码算法

主要基于`bouncycastle`实现

### 杂凑

#### RIPEMD160

输出为`20`个字节长度。

#### SHA256

输出为`32`个字节长度。

#### 示例

```java
HashFunction function = Crypto.getHashFunction(ClassicAlgorithm.SHA256);
HashDigest digest = function.hash(data);
```

### 对称加密

#### AES

`AES128/CBC/PKCS7Padding`

#### 示例

```java
SymmetricEncryptionFunction function = Crypto.getSymmetricEncryptionFunction(ClassicAlgorithm.AES);
SymmetricKey symmetricKey =function.generateSymmetricKey()

// 加解密
byte[] ciphertextBytes = function.encrypt(data, symmetricKey);
byte[] plainBytes = function.decrypt(ciphertextBytes, symmetricKey);
assertArrayEquals(data, plainBytes);
```

### 非对称加密

#### ECDSA

`Curve secp256r1 with SHA256`

#### ED25519

#### RSA

`RSA2048/SHA256withRSA/PKCS1v2/PKCS8`

#### 示例

```java
SignatureFunction function = Crypto.getSignatureFunction(ClassicAlgorithm.ECDSA);
AsymmetricKeypair keyPair = function.generateKeypair();
PubKey pubKey = keyPair.getPubKey();
PrivKey privKey = keyPair.getPrivKey();

SignatureDigest signatureDigest = function.sign(privKey, data);
assertTrue(function.verify(signatureDigest, pubKey, data));
```

## 国密算法

### 杂凑

#### SM3

输出为`32`个字节长度。

#### 示例
```java
HashFunction function = Crypto.getHashFunction(SMAlgorithm.SM3);
HashDigest digest = function.hash(data);
```

### 对称加密

#### SM4

#### 示例

```java
SymmetricEncryptionFunction function = Crypto.getSymmetricEncryptionFunction(SMAlgorithm.SM4);
SymmetricKey symmetricKey =function.generateSymmetricKey()

// 加解密
byte[] ciphertextBytes = function.encrypt(data, symmetricKey);
byte[] plainBytes = function.decrypt(ciphertextBytes, symmetricKey);
assertArrayEquals(data, plainBytes);
```

### 非对称加密

#### SM2

#### 示例

```java
SignatureFunction function = Crypto.getSignatureFunction(SMAlgorithm.SM2);
AsymmetricKeypair keyPair = function.generateKeypair();
PubKey pubKey = keyPair.getPubKey();
PrivKey privKey = keyPair.getPrivKey();

SignatureDigest signatureDigest = function.sign(privKey, data);
assertTrue(function.verify(signatureDigest, pubKey, data));
```

## 高级密码算法

### Elgamal

签名验签：
```java
// 获取算法
CryptoAlgorithm algorithm = Crypto.getAlgorithm("ELGAMAL");
AsymmetricEncryptionFunction cryptoFunction = Crypto.getAsymmetricEncryptionFunction(algorithm);
// 生成公私钥对
AsymmetricKeypair keypair = cryptoFunction.generateKeypair();
byte[] data = "hello".getBytes(StandardCharsets.UTF_8);
byte[] encrypt = cryptoFunction.encrypt(keypair.getPubKey(), data);
byte[] decrypt = cryptoFunction.decrypt(keypair.getPrivKey(), encrypt);
assertArrayEquals(data, decrypt);
```

### Paillier

Paillier 提供加解密，同态加法、乘法，[工具类](https://github.com/blockchain-jd-com/jdchain-framework/blob/5d83bf7248604ad8afab34c24ddbefe1725a3fe1/crypto/crypto-adv/src/test/java/com/jd/blockchain/crypto/service/adv/PaillierCryptoFunctionTest.java)及合约[示例参照](https://github.com/blockchain-jd-com/jdchain-samples/blob/b576f2bb9e7ec7e43b1bc2cb43326561ffa8f8aa/contract-samples/src/main/java/com/jdchain/samples/contract/SampleContract.java#L181)

### Shamir

基于开源[Shamir](https://github.com/codahale/shamir)实现，提供秘密分享工具类，[示例参照](https://github.com/blockchain-jd-com/utils/blob/master/utils-crypto-adv/src/test/java/utils/crypto/adv/ShamirUtilsTest.java)

### BulletProof

移植[开源实现](https://github.com/bbuenz/BulletProofLib)提供[范围证明工具类](https://github.com/blockchain-jd-com/utils/blob/master/utils-crypto-adv/src/test/java/utils/crypto/adv/BulletProofUtilsTest.java)