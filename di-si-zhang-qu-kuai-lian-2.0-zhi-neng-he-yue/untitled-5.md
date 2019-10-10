# 4.6 ERC-721不可置换通证标准及其爆发性应用

## **同质性物品和非同质性物品**

如果一个物品被认为是同质性的，那么这个物品类别下的每个个体之间没有差别。也就是每两个同质性物品的价值在所有人眼中完全相同，它们互相之间可以交换。

比如说人民币1元硬币，每枚1元硬币都可以与另一枚1元硬币互换。所以人民币1元硬币是可互换的，它就属于同质性物品。

故宫博物馆的每件藏品，则都属于非同质性物品，哪怕两件藏品都是仕女图，保存完好度，制作年份，风格特色，甚至曾经的收藏者的身份，等等，对它们的价值都有影响。

同质性的物品可能通过区块链技术变成非同质性物品。譬如游戏道具，或游戏卡牌。

**同质资产的定义**

adjective Law. 

\(especially of goods\) being of such nature or kind as to be freely exchangeable or replaceable, in whole or in part, for another of like nature or kind.

**非同质资产的定义**

因此，非同质资产（不可替代资产）具有与同质物品定义相反的特征。 这些特征是：

* Unique（独特）
* Irreplaceable（不可替代）
* Non-interchangeable（不可互换）

最常见的应用案例：

同质资产：法币、比特币、便利贴、Post-it、几页同品牌打印纸……  
非同质资产：艺术作品、人、游戏角色、房产、地产……

Technically speaking each Bitcoin is definitely unique. This is why it is possible to trace the origin of a Bitcoin, and whether or not it participated in transactions in the “darknet”.

Some people treat NFTs as fungible assets, totally ignoring the use behind them, considering only the financial value of the asset. Two parcels of LAND on the [Decentraland Marketplace](https://market.decentraland.org/) worth 10,000 MANA may seem totally fungible to an unscrupulous speculator.

It is therefore the use value that defines the fungible or non-fungible character of the asset. And not its technical characteristics. The main use of an asset and the perception that one can have of it define fundamentally if the asset is fungible or not.

https://medium.com/nonfungible/why-most-of-the-definitions-of-non-fungible-are-incorrect-3565fa3cfc66

## ERC-721不可置换通证标准

ERC-721是一个自由的开放标准，描述了如何在以太坊区块链上构建不可互换或独特的通证。尽管现在大多数通证采用了ERC20通证标准，也就是说大多数通证都是可置换的（每个代币都与其他代币完全相同），对于主要需要货币属性的通证来说，ERC20标准是很好的选择。但是对于具有唯一性的物品，ERC20接口就力不从心了，例如同是一幅名画，真实作品和复制品价值是不一样的。所以就有了专门针对收藏品等的ERC721标准：每个ERC-721通证都是唯一的。

这就是说：ERC20通证是可以相互置换的，而ERC721通证则是不能置换的，亦即ERC721的每个通证是唯一的，且不可分割，它的最小单位就是1。

ERC-721通证标准的第一个应用是[Cryptokitties](https://www.cryptokitties.co/)。中文名叫谜恋猫，又被币民称为加密猫。

Cryptokitties的每一只猫对应于一个ERC-721通证。每一只猫都是不一样的，不同的毛色，不同的眼睛嘴巴鼻子，等等。而不同的遗传特征组合决定了这只猫的价格高低。你用一只普通的猫，来换我的名贵的猫，我会给你吗？当然不可能！所以ERC-721的通证，就是典型的“不可替换”的通证。每一个通证都是一个独立的个体，有着自己独特的价值。这样每一个都具有独特性和稀缺性，所以ERC-721最大的应用属性被认为是收藏。ERC-721在收藏品市场的应用，就是每个ERC-721对应罕见的独一无二的收藏品。

ERC-721不可置换通证标准允许在智能合同中部署不可置换通证（NFT）的标准API。该标准提供了跟踪和转让不可置换通证的基本功能。我们很容易就联想到不可置换通证由个人拥有和处理以及向第三方经纪人/钱包/拍卖商（运营商）委托的例子，不可置换通证可以代表数字或实物资产的所有权（或债务）。我们也很容易就联想到ERC-721不可置换通证标准适应的几种资产：

* 有形资产——房产、独特的艺术品等等；
* 虚拟收藏品——独特的小猫图片、收藏卡片、卡牌游戏中的卡牌、Decentraland中的土地和房产等等；
* “负值”资产——贷款、债务和其它财务责任。

一般来说，所有的小猫都是独特的，没有两只小猫是一样的。不可置换通证（NFT）恰好是可区分的，你能够独立跟踪每一只猫的所有权。

所以，ERC-721是允许在以太坊的任何不可置换通证上应用钱包/经纪人/拍卖app的一种标准接口。它提供简单的ERC-721智能合约以及跟踪任意数量不可置换通证的合约。可以说ERC-721定义了智能合约必须执行的最小接口，以便管理，拥有和交易独特的通证。它没有强制标记元数据的标准或限制添加补充功能。此标准受ERC-20通证标准的启发，并建立在EIP-20创建以来两年的经验基础之上。EIP-20的局限是不足以追踪不可置换通证，因为每一个资产都是不同的（不可置换的），但一堆ERC20代币里的每一个都是相同的（可置换的）。

ERC721不可置换通证标准定稿于18年3月13日，它从测试版中移出并转化为社区正式版v1规范，很快就得到来自整个加密生态系统的大量项目的支持和认可。最大的导火索是以该通证标准创新出基因工程智能合约的游戏“谜恋猫”，在ERC721不可置换通证标准定稿之前，“谜恋猫”于2017年11月28日即登录以太坊区块链，并成功地引发市场热捧。

ERC721不可置换通证标准的代表性应用为区块链“游戏”CryptoCelebrities、CryptoKitties（谜恋猫）、EtherTulips、CryptoPunks、Ethercraft、Decentraland、Etheremon（以太怪物）和Etherbots（以太机器人）等等。这些“游戏”可以说很低幼，但受追捧程度都很高。因为ERC721不可置换通证标准为游戏开发者提供了三个新的构建模块：

首先，智能合约允许开发人员创建可公开验证的规则，用户在不受国界限制的情况下可以在全球各地互相连接，并且交易金是内置于协议中的。

其次，不可置换通证（NFT）能够开发出可证明的集稀缺性、可编程性和抗审查三种特性于一体的数字商品。

其三，它使得虚拟角色、道具、勋章、物品可以跨服（服务器）、跨界（国界）、跨戏（游戏）。

这三个新的构建模块将使开发者能够扩展现有的游戏内核，或者创建新的游戏内核。会不会鸟枪变大炮，我们拭目以待！

ERC721不可置换通证标准，又被翻译为非同质代币标准。

\*\*\*\*

