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

Comming soon...