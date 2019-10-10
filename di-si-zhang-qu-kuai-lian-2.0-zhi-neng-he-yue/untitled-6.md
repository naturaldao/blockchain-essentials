# 4.7 EIP 1155 （1）：加密物品标准

## EIP 1155

| **作者** | [Witek Radomski](mailto:witek@enjin.com), [Andrew Cooke](mailto:andrew@enjin.com) |
| :--- | :--- |
| **讨论区** | [https://github.com/ethereum/EIPs/issues/1155](https://github.com/ethereum/EIPs/issues/1155) |
| **状态** | Final（终稿） |
| **类型** | 标准化跟进 |
| **分类** | ERC |
| **创建日期** | 2018-06-17 |

**A. 概述**

在单个部署的合约中定义多个物品或通证的标准接口。

**B. 摘要**

这个标准概述了一种可以在单个合约中表示任意数量的可置换和不可置换通证的智能合约接口。ERC-20等现有的标准需要为每种通证部署单独的智能合约。ERC-721标准的通证ID是只能绑定一个不可置换通证的索引，由这些独特的索引组成的通证集群部署在包含全体信息集合的一个合约上。不同的是，ERC-1155加密标准允许每个物品ID代表一个新的可配置的通证类型，它可能有自己的通证总量和其它类似的属性。

\_id参数包含在每个函数的参数中，并在一个交易中表示特定的物品或物品类型。

**C. 动机**

像ERC-20和ERC-721这样的通证标准要求为每个可置换和不可置换通证集部署在一个单独的合约中。这样导致了在Ethereum区块链上放置了大量的冗余字节码，天生会分离各自的合约到它自己的授权地址导致某些功能受限。随着加密游戏和像Enjin Coin这样的平台的兴起，游戏开发人员可能会创建数万个物品，需要一种新的通证标准来支持。

这种设计方法可能会产生新的功能需求，比如一次性传递或审批多种类型的通证，以节省交易成本。多通证交易（第三方托管，原子互换，原子互换是指不通过中介物而将一种token换成为另外一种token）可以在这个标准之上实现，它消除了单独“批准”单个通证的需要。这样也很方便在一份合约中混合或声明多个可置换和不可置换通证。

**\# 组播传输**

multicastTransferFrom 函数支持批量传输到多个地址和从多个地址传输。也可以通过获取源方和目标方批准并执行此函数的方式创建简单的原子交换。

**\# 向后兼容**

本标准与 ERC-721 不可替换通证兼容。两个接口都能在不发生冲突的情况下继承：

**\# 规范**

pragma solidity ^0.4.24;

**接口函数或者变量说明**

* **totalTickets**:
* **inventory**:
* **ticketIndex**:
* **expiryTimeStamp**:
* **owner**:
* **admin**:
* **transferFee**:
* **numOfTransfers**:
* **name**:
* **symbol**:
* **decimals**:
* **Token\(uint256\[\] numberOfTokens, string evName,uint expiry, string eventSymbol, address adminAddr\)**: 构建函数，样例如example: \[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15\], "MJ comeback", 1603152000, "MJC", "0x007bEe82BDd9e866b2bd114780a47f2261C684E3"\]
* **getDecimals\(\)**: 返回代币的小数点后位数值；
* **trade\(uint256 expiry,uint256\[\] tokenIndices,uint8 v,bytes32 r,bytes32 s\)**: 交易函数，例如：0, \[3, 4\], 27, "0x2C011885E2D8FF02F813A4CB83EC51E1BFD5A7848B3B3400AE746FB08ADCFBFB", "0x21E80BAD65535DA1D692B4CEE3E740CD3282CCDC0174D4CF1E2F70483A6F4EB2"
* **encodeMessage\(bytes12 prefix, uint value,uint expiry, uint256\[\] tokenIndices\)**:
* **name\(\)**: 返回代币名称；
* **symbol\(\)**: 返回代币表示；
* **getAmountTransferred\(\)**: 返回已传输的数量；
* **isContractExpired\(\)**： 合约是否过期；
* **balanceOf\(address \_owner\)**： 返回账户余额；
* **myBalance\(\)**：
* **transfer\(address \_to, uint256\[\] tokenIndices\)**： 资产转账；
* **transferFrom\(address \_from, address \_to, uint256\[\] tokenIndices\)**：
* **endContract\(\)**： 结束合约；
* **getContractAddress\(\)**： 获取合约地址；

## 应用

在同一合同中将可替换的和不可替代的物品组合在一起的一个示例策略是：在uint256 itemID参数的头部128位中传递物品ID，然后使用底部的128位空间来传递您希望传递给合同的任何额外数据。

不可替代的物品可以通过使用基于存取在合约或者数据集的索引来进行交互。因此要访问混合数据合约中物品集的特定物品以及特定的不可置换通证（Non-Fungible Token），可以通过unit128来传递物品id。

在通过unit128提取和转移非同质通证时候需要访问合约代码里面各自的非同质通证的2处数据。

## 启发

1. 除了为多个通证提供标准接口外，ERC-1155还带来其它重要好处：
   * 通过将多个复杂的操作打包到一个交易中，它将使交易更便宜；
   * 它能组合多个通证并启用原子交换（勿需第三方）；
   * 它将大大提高以太坊网络的效率；
   * ERC-1155允许开发人员将一组通证的代码存储在一个智能合约中，并在需要时被其他智能合约所调用。 简而言之，它可被充分地重复利用，而不是像现在其它标准下的不断复制代码部署新合约。 可想而知，这将大大减少以太坊的存储空间和算力。 
2. 因为智能合约能够保证收益无法被侵犯，知识产权、版权等概念，可能在区块链领域将无存在的必要。

## 最后

请多关注：[https://nonfungible.com/](https://nonfungible.com/)

