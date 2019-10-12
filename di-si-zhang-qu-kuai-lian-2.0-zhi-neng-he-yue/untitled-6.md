# 4.7 EIP-1155 ：多重通证标准

## EIP 1155

| **作者** | [Witek Radomski](mailto:witek@enjin.com), [Andrew Cooke](mailto:andrew@enjin.com) |
| :--- | :--- |
| **讨论区** | [https://github.com/ethereum/EIPs/issues/1155](https://github.com/ethereum/EIPs/issues/1155) |
| **状态** | Final（终稿） |
| **类型** | 标准化跟进 |
| **分类** | ERC |
| **创建日期** | 2018-06-17 |

**A. 概述**

管理多种通证类型的合约的标准接口。 单个智能合约可以涵括同质通证、非同质通证或其它配置（例如半同质通证）的任意组合。

**B. 摘要**

该标准制订了一种在单个合约中表示任意数量的同质性和非同质性通证类型的智能合约接口。 现有标准（例如ERC-20）要求为每种通证类型部署单独的智能合约。ERC-721标准的通证ID是只能绑定一种不可置换通证的索引，而这些非同质性通证成组地使用设置统一的单个智能合约进行部署。不同的是，ERC-1155多重通证标准允许每个通证ID代表一种新的可配置的通证类型，它可以有自己的元数据，供应总量和其它属性。

每个函数的参数集中的\_id参数，在一个交易中表示特定的通证或通证类型。

**C. 动机**

像ERC-20和ERC-721这样的通证标准要求每种同质通证和非同质通证集，部署在一个独有的合约中。这样导致了在以太坊区块链上放置了大量的冗余字节码，天生会分离各自的合约到它自己的授权地址导致某些功能受限。随着诸如Enjin Coin之类的区块链游戏和平台的兴起，游戏开发人员可能会创建数千种通证，因此需要一种新型的通证标准来支持它们。但是，ERC-1155并不特定于游戏，许多其他应用程序也可以从这种灵活性中受益。

这种设计方法可能会产生新的功能需求，比如一次性传递多种通证，以节省交易成本。多通证交易（第三方托管，原子互换，原子互换是指不通过中介物而将一种通证换成为另外一种通证）可以在这个标准之上实现，而无需分别“批准”单个通证合约。在单个合约中描述和混合多种同质通证或非同质通证也很容易。

**\# 向后兼容**

本标准与 ERC-20及ERC-721 非同质通证标准兼容。

**\# 规范**

pragma solidity ^0.5.9;

**一些核心接口函数或者变量说明**

* **mint\(string \_name, uint256 \_totalSupply, string \_uri, uint8 \_decimals, string \_symbol\)**   增发同质通证；
* **approve\(address \_spender, uint256 \_id, uint256 \_currentValue, uint256 \_value\)**

  授权给\_spender账户一定额度的编号为 \_id的同质通证，\_currentValue为当前已授权额度；

* **allowance\(uint256 \_id, address \_owner, address \_spender\)**

  拥有者 \_owner给消费者\_spender在当前查询账户授权\(approve\)的额度；

* batchApprove\(address \_spender, uint256\[\] \_ids, uint256\[\] \_currentValues, uint256\[\] \_values\) 批量授权给\_spender\[\]一组账户一定额度\_values\[\]的编号为\_ids\[\]的同质通证，\_currentValue\[\]为当前已授权额度，这几个数组的长度要严格对齐；
* batchTransferFrom\(address \_from, address \_to, uint256\[\] \_ids, uint256\[\] \_values\)

  拥有者从 \_from地址给 \_to地址转账授权范围内的一定额度\_values\[\]的各类编号为\_ids\[\]的各类同质通证；

* **-batchTransfer\(address \_to, uint256\[\] \_ids, uint256\[\] \_values\)**

  批量给目标账号\_to转账各类编号为\_ids\[\]的各类数额分别是\_values\[\]的各类同质通证；

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
* **balanceOf\(uint256 \_id, address \_owner\)**

  返回拥有者\_owner的 \_id同质通证的余额；

* **myBalance\(\)**：
* **transfer\(address \_to, uint256\[\] tokenIndices\)**： 资产转账；
* **transferFrom\(address \_from, address \_to, uint256 \_id, uint256 \_value\)**

  拥有者从 \_from地址给 \_to地址转账授权范围内的一定额度的一类同质通证；

* **multicastTransfer\(address\[\] \_to, uint256\[\] \_ids, uint256\[\] \_values\)**

  当前账号批量给目标地址组合\_to\[\]分别转移额度为\_values\[\]的各类编号为\_ids\[\]的同质通证。

* **endContract\(\)**： 结束合约；
* **getContractAddress\(\)**： 获取合约地址；

## 应用

在同一合约中将同质通证和非同质通证组合在一起的一个示例策略是：在uint256 itemID参数的头部128位中传递通证ID，然后使用底部的128位空间来传递您希望传递给合约的任何额外数据。

非同质通证可以通过使用基于存取在合约或者数据集的索引来进行交互。因此要访问混合数据合约中通证集的特定通证以及特定的非同质通证（Non-Fungible Token），可以通过unit128来传递通证id。

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

