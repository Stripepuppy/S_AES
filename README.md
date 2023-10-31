# S_AES
SimpleAES


# 程序测试结果

## 第1关：基本测试

根据S-AES算法编写和调试程序，提供GUI解密支持用户交互。输入可以是16bit的数据和16bit的密钥，输出是16bit的密文。
![image](https://github.com/Stripepuppy/S_AES/assets/133982775/a8d715a0-8aaf-411e-8689-326982d3de98)


## 第2关：交叉测试

考虑到是"**算法标准"**，所有人在编写程序的时候需要使用相同算法流程和转换单元(替换盒、列混淆矩阵等)，以保证算法和程序在异构的系统或平台上都可以正常运行。

设有A和B两组位同学(选择相同的密钥K)；则A、B组同学编写的程序对明文P进行加密得到相同的密文C；或者B组同学接收到A组程序加密的密文C，使用B组程序进行解密可得到与A相同的P。
![image](https://github.com/Stripepuppy/S_AES/assets/133982775/ccb84de4-fd76-46d8-8325-e7a246e289d1)



## 第3关：扩展功能

考虑到向实用性扩展，加密算法的数据输入可以是ASII编码字符串(分组为2 Bytes)，对应地输出也可以是ACII字符串(很可能是乱码)。
![image](https://github.com/Stripepuppy/S_AES/assets/133982775/4dd89030-dae8-4c74-8294-aa1bd4beded4)


## 第4关：多重加密

### 4.1 双重加密

将S-AES算法通过双重加密进行扩展，分组长度为16 bits，密钥长度为32 bits。

### 4.2 中间相遇攻击

假设找到了使用相同密钥的明、密文对(一个或多个)，可尝试使用**中间相遇攻击**的方法找到正确的密钥Key(K1+K2)。

### 4.3 三重加密

将S-AES算法通过三重加密进行扩展，选择第一种模式完成：

(1)按照32 bits密钥Key(K1+K2)的模式进行三重加密解密，由于可能密码过多，在此只展示前三个可能密钥，全部可能密钥存放在all_possible_keys.txt。
![image](https://github.com/Stripepuppy/S_AES/assets/133982775/f2c70f82-b497-4793-bd69-272510a75712)


## 第5关：工作模式

基于S-AES算法，使用**密码分组链(CBC)模式**对较长的明文消息进行加密。注意初始向量(16 bits) 的生成，并需要加解密双方共享。

在CBC模式下进行加密，并尝试对密文分组进行替换或修改，然后进行解密，请对比篡改密文前后的解密结果。
![image](https://github.com/Stripepuppy/S_AES/assets/133982775/465ff339-5746-4987-ad15-e7bf7e253d8d)


# S-DES加解密工具开发手册

本开发手册旨在帮助您了解和使用S-DES（Simplified Data Encryption Standard）加解密算法的Flask应用程序，以及使用暴力破解方式找到可能的密钥。下面是关于如何使用这个应用程序的详细说明。

## 目录

1. 概述

2. 安装依赖

3. 启动应用程序

4. 前端页面设计

   - 4.1 二进制输入页面 (binary.html)
   - 4.2 字符串输入页面 (string.html)
   - 4.3 多重加密页面 (advanced.html)
   - 4.4 CBC工作模式页面 (CBC.html)

5. S-AES加解密算法

   - 5.1 算法简介
   - 5.2 参数设置
   - 5.3 密钥加
   - 5.4 半字节代替
   - 5.5 行位移
   - 5.6 列混淆
   - 5.7 S-AES加密算法
   - 5.8 S-AES解密算法

6. 多重加密

   - 6.1 双重加密
   - 6.2 中间相遇攻击
   - 6.3 三重加密

7. 密码分组链(CBC)工作模式

8. 用户指南

   

## 1. 概述

这个应用程序实现了密钥长度为16bits的情况下S-AES的加解密功能，并提供两种不同的输入模式：字符串输入和二进制输入；双重加密（含中间相遇攻击查找密钥功能）、三重加密；基于S-AES算法，使用**密码分组链(CBC)模式**对较长的明文消息进行加密，并尝试对密文分组进行替换或修改，然后进行解密，对比篡改密文前后的解密结果。

## 2. 安装依赖

在开始使用该应用程序之前，您需要安装以下依赖项：

- Python 3.x
- Flask

您可以使用以下命令来安装Flask：

```bash
pip install Flask
```

## 3. 启动应用程序

要运行该应用程序，使用以下命令在终端中进入应用程序所在的目录，并执行以下命令：

```bash
python app.py
```

应用程序将在本地启动一个Flask服务器，并在默认端口（通常为5000）上运行。您可以通过访问 `http://localhost:5000/` 在浏览器中打开应用程序的主页。

## 4. 前端页面设计

### 4.1 二进制输入页面 (binary.html)

### 4.2 字符串输入页面 (string.html)

### 4.3 多重加密页面 (advanced.html)

### 4.4 CBC工作模式页面 (CBC.html)

## 5. 加解密算法

### 5.1. 算法简介

S-AES，即简化高级加密标准（Simplified Advanced Encryption Standard），是为了教育目的而提出的高级加密标准(AES)的简化版本。尽管结构与AES相似，但S-AES的操作更简单，只有两轮加密过程。

加密算法使用4个不同的函数或变换：密钥加、半字节代替、行位移、列混淆。

### 5.2 密钥加

密钥加函数将16位状态矩阵与16位轮密钥逐位异或。

```
# 密钥加 输入16bit与密钥
def addKey(arr, key):
    arr = np.array(arr)
    key = np.array(key)
    res = np.bitwise_xor(arr, key)
    return res
```



### 5.3 半字节代替

半字节代替函数是简单的查表操作。AES定义一个4*4的半字节值矩阵，称为S盒，其中包含所有4位值的排列。状态中的每个半字节都按以下方式映射到一个新的半字节：半字节最左侧的2位用作行值，最右侧的2位用作列值。这些行和列的值用作S盒中选择唯一的4位输出值的索引。

```python
# 半字节替代 S盒
def halfByteSubstitude(arr):
    indexDic = {(0, 0): 0, (0, 1): 1, (1, 0): 2, (1, 1): 3}
    SBox = [
        [np.array([1, 0, 0, 1]), np.array([0, 1, 0, 0]), np.array([1, 0, 1, 0]), np.array([1, 0, 1, 1])],
        [np.array([1, 1, 0, 1]), np.array([0, 0, 0, 1]), np.array([1, 0, 0, 0]), np.array([0, 1, 0, 1])],
        [np.array([0, 1, 1, 0]), np.array([0, 0, 1, 0]), np.array([0, 0, 0, 0]), np.array([0, 0, 1, 1])],
        [np.array([1, 1, 0, 0]), np.array([1, 1, 1, 0]), np.array([1, 1, 1, 1]), np.array([0, 1, 1, 1])]
    ]
    mat = stateMatConstruct(arr)
    for i in range(4):
        mat[i] = SBox[indexDic[tuple(mat[i][0:2])]][indexDic[tuple(mat[i][2:4])]]
    return stateMatDestorey(mat)


def inverseHalfByteSubstitude(arr):
    indexDic = {(0, 0): 0, (0, 1): 1, (1, 0): 2, (1, 1): 3}
    SBox = [
        [np.array([1, 0, 1, 0]), np.array([0, 1, 0, 1]), np.array([1, 0, 0, 1]), np.array([1, 0, 1, 1])],
        [np.array([0, 0, 0, 1]), np.array([0, 1, 1, 1]), np.array([1, 0, 0, 0]), np.array([1, 1, 1, 1])],
        [np.array([0, 1, 1, 0]), np.array([0, 0, 0, 0]), np.array([0, 0, 1, 0]), np.array([0, 0, 1, 1])],
        [np.array([1, 1, 0, 0]), np.array([0, 1, 0, 0]), np.array([1, 1, 0, 1]), np.array([1, 1, 1, 0])]
    ]
    mat = stateMatConstruct(arr)
    for i in range(4):
        mat[i] = SBox[indexDic[tuple(mat[i][0:2])]][indexDic[tuple(mat[i][2:4])]]
    return stateMatDestorey(mat)
```



### 5.4 行位移

行位移函数在状态的第二行执行一个半字节循环移位。第一行不变。

由于逆行位移函数将第二行移回原来的位置，因此逆行位移函数和行位移函数相同。

```python
# 行位移
def rowShift(arr):
    S00, S01, S10, S11 = stateMatConstruct(arr)
    mat = [S00, S01, S11, S10]
    return stateMatDestorey(mat)
```



### 5.5 列混淆

列混淆函数在各列上执行。列中的每半个字节都映射为一个新值，其中新值是该列中两个半字节的函数。

```python
# 列混淆
def columnMix(arr):
    mixMat = [
        [np.array([0, 0, 0, 1]), np.array([0, 1, 0, 0])],
        [np.array([0, 1, 0, 0]), np.array([0, 0, 0, 1])],
    ]
    mat = stateMatConstruct(arr)
    mat = [
        [mat[0], mat[1]],
        [mat[2], mat[3]]
    ]
    result = mulGFMat(mixMat, mat)
    K = [item for sublist in result for item in sublist]
    return stateMatDestorey(K)


# 逆列混淆
def inverseColumnMix(arr):
    mixMat = [
        [np.array([1, 0, 0, 1]), np.array([0, 0, 1, 0])],
        [np.array([0, 0, 1, 0]), np.array([1, 0, 0, 1])],
    ]
    mat = stateMatConstruct(arr)
    mat = [
        [mat[0], mat[1]],
        [mat[2], mat[3]]
    ]
    result = mulGFMat(mixMat, mat)
    K = [item for sublist in result for item in sublist]
    return stateMatDestorey(K)
```



### 5.6 S-AES加密算法

加密算法被组织为三轮，第0轮是简单的密钥加轮；第1轮是包含4个函数的完整轮；第2轮仅包含3个函数。每轮都使用16位密钥的密钥加函数。初始的16位密钥被扩展到48位，以便每轮都可以使用一个不同的密钥。

```
# 加密算法
def encode(key, content):
    k1, k2, k3 = generate_key(key)
    content = np.array(content)
    content = addKey(
        rowShift(halfByteSubstitude(addKey(columnMix(rowShift(halfByteSubstitude(addKey(content, k1)))), k2))), k3)
    return content
```



### 5.7 S-AES解密算法

## 6. 多重加密

### 6.1 双重加密

```
# 双重加密
def doubleEncode(key, content):
    firstKey = key[0:16]
    lastKey = key[16:32]
    return encode(lastKey, encode(firstKey, content))
```



### 6.2 中间相遇攻击

```
# 相遇攻击破解双重加密
def brute_force_sdes(ciphertext, plaintext):
    possible_keys = []
    midTextA = {}
    midTextB = {}

    # 对每一个可能的keyA, 加密明文并存储中间结果
    for key in range(2 ** 16):
        binary_key = np.array([int(b) for b in format(key, '016b')])
        encoded_text = encode(binary_key, plaintext)
        midTextA[tuple(encoded_text)] = binary_key

    # 对每一个可能的keyB, 解密密文并存储中间结果
    for key in range(2 ** 16):
        binary_key = np.array([int(b) for b in format(key, '016b')])
        decoded_text = decode(binary_key, ciphertext)
        midTextB[tuple(decoded_text)] = binary_key

    for encoded_text_str in midTextA.keys():
        if encoded_text_str in midTextB.keys():
            possible_keys.append(np.concatenate([midTextA.get(encoded_text_str), midTextB.get(encoded_text_str)]))

    return possible_keys
```



### 6.3 三重加密

```
# 三重加密
def tripleEncode(key, content):
    firstKey = key[0:16]
    lastKey = key[16:32]
    return encode(firstKey, decode(lastKey, encode(firstKey, content)))
```



## 7. 密码分组链(CBC)工作模式

密码分组链（Cipher Block Chaining，简称 CBC）是一种常用的块密码工作模式。在这种工作模式中，每个明文块在加密之前会与前一个密文块进行XOR操作（对于第一个块，则与一个初始向量IV进行XOR操作）。这使得相同的明文块，如果出现在不同的位置或者与不同的IV配对，会生成不同的密文块。

```
# CBC加密字符串
def encodeCBCString(key, string):
    IV = np.array([1, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1])
    arr = string_change(string)
    en_arr = []
    for i in range(len(arr)):
        temp = np.bitwise_xor(arr[i], IV)
        IV = encode(key, temp)
        en_arr.append(IV)
    en_arr = [c.astype(str) for c in en_arr]
    en_arr = [''.join(c.tolist()) for c in en_arr]
    en_arr = [int(c, 2) for c in en_arr]
    encoded_string = ''.join([chr(i) for i in en_arr])
    return encoded_string


# CBC解密字符串
def decodeCBCString(key, string):
    firstIV = np.array([1, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1])
    arr = string_change(string)
    arr.reverse()
    de_arr = []
    for i in range(len(arr)):
        if i != len(arr) - 1:
            IV = arr[i + 1]
        else:
            IV = firstIV
        temp = decode(key, arr[i])
        temp = np.bitwise_xor(temp, IV)
        de_arr.append(temp)
    de_arr.reverse()
    de_arr = [c.astype(str) for c in de_arr]
    de_arr = [''.join(c.tolist()) for c in de_arr]
    de_arr = [int(c, 2) for c in de_arr]
    decoded_string = ''.join([chr(i) for i in de_arr])
    return decoded_string
```



## 8. 用户指南

### 8.1 二进制输入模式

在主页上选择 "Binary" 选项卡，然后您可以执行以下操作：

- **加密二进制数据**：输入16位二进制表示的明文和16位二进制密钥，然后单击 "Encrypt" 按钮来加密数据。
- **解密二进制数据**：输入16位二进制表示的密文和16位二进制密钥，然后单击 "Decrypt" 按钮来解密数据。

### 8.2 字符串输入模式

在主页上选择 "String" 选项卡，然后您可以执行以下操作：

- **加密字符串**：输入明文文本和16位二进制密钥，然后单击 "Encrypt" 按钮来加密文本。
- **解密字符串**：输入密文文本和16位二进制密钥，然后单击 "Decrypt" 按钮来解密文本。

### 8.3 多重加密模式

在主页上选择 "Advanced" 选项卡，然后您可以执行以下操作：

- **双重加密二进制数据**：输入16位二进制表示的明文和32位二进制密钥，然后单击 "Encrypt" 按钮来加密文本。
- **三重加密二进制数据**：输入16位二进制表示的密文和32位二进制密钥，然后单击 "Encrypt" 按钮来加密文本。
- **中间相遇攻击查找密钥**：输入16位二进制表示的明文和密文，然后单击 "Attack！" 按钮来查找密钥。

### 8.4 CBC工作模式

在主页上选择 "CBC" 选项卡，然后您可以执行以下操作：

- **加密字符串**：输入明文文本和16位二进制密钥，然后单击 "Encrypt" 按钮来加密文本。
- **解密字符串**：输入密文文本和16位二进制密钥，然后单击 "Decrypt" 按钮来解密文本。





