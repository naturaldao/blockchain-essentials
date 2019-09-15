# 4.2 解读以太坊

“My goal in creating Ethereum was to create a platform that was open, decentralized, transparent, easy to use and free for anyone to participate and build things. I think that this kind of platform can be good for humanity.”（ “我创建以太坊的目标是创建一个开放、去中心化、透明、易用和任何人都可以自由参与和构建东西的平台。我认为这种平台对人类有益。”） ——Vitalik Buterin

以下摘自《[Mastering Ethereum](https://github.com/ethereumbook/ethereumbook/blob/develop/book.asciidoc)》：

From a computer science perspective, Ethereum is a deterministic but practically unbounded state machine, consisting of a globally accessible singleton state and a virtual machine that applies changes to that state.

From a more practical perspective, Ethereum is an open source, globally decentralized computing infrastructure that executes programs called smart contracts. It uses a blockchain to synchronize and store the system’s state changes, along with a cryptocurrency called ether to meter and constrain execution resource costs.

The Ethereum platform enables developers to build powerful decentralized applications with built-in economic functions. While providing high availability, auditability, transparency, and neutrality, it also reduces or eliminates censorship and reduces certain counterparty risks.

从计算机科学的角度来看，以太坊是一个确定性但实际上无限制的状态机，由全局可访问的单一状态和能够改变其状态的虚拟机组成。

从更实际的角度来看，以太坊是一个开源的，全球分散的计算基础设施，执行称为智能合约的程序。 它使用区块链来同步和存储系统的状态变化，以及称为ether（货币符号为ETH）的加密货币来计量和约束执行资源所需的成本。

以太坊平台使开发人员能够构建具有内置经济功能的强大的去中心化应用程序。在提供高可用性，可审计性，透明度和中立性的同时，它还减少或消除了审查并降低了某些交易对手的风险。

和比特币类似，以太坊是一个去中心化公链，由很多台电脑组网协作而成，接入这个网络的每个节点（电脑）本地都会保存一份完整区块链（可理解为一个本地数据库）。

每个节点电脑都需要安装以太坊客户端，而这个以太坊客户端自带了EVM（虚拟机），它是一个以太坊智能合约的执行环境，类似JVM。通过交易触发智能合约后，智能合约的代码就会在EVM中执行了。这种方式相当于把程序部署到了非常非常多的电脑上（只要这个电脑安装了以太坊客户端并接入了以太坊网络），随时都可以通过交易来触发这些智能合约的执行，也从而完成了分布式程序的部署和调用。所以以太坊是一台去中心化的“超级计算机”或者叫“世界计算机”。

因此，以太坊（Ethereum）是一个开源的有智能合约功能的、可以承载众多去中心化应用（decentralized applications,dapp）的公共区块链平台。通过其专用通证（加密货币）以太币（Ether，ETH）提供去中心化的虚拟机（Ethereum Virtual Machine）来处理点对点合约。

以太坊的概念首次在2013至2014年间由程序员Vitalik Buterin受比特币启发后提出，在2014年通过ICO众筹得以开始发展。

以太坊最重要的技术贡献就是构建了一整套智能合约的基础逻辑、协议和标准，并且还在不断拓展中。以太坊的智能合约可用数种用图灵完备的编程语言编写。

以太坊可用来创建无数的区块链应用，即智能合约、去中心化的程序（dapp）和自治组织，目前较知名的应用有：

* 游戏：CryptoKitties让玩家繁殖及交易虚拟猫。
* 虚拟宝物交易平台：FreeMyVunk。
* 去中心化创业投资：The DAO目标是去中心化投资、The Rudimental让独立艺术家在区块链上进行众筹。
* 社会经济平台：Backfeed。
* 去中心化预测市场：Augur。
* 物联网：Ethcore研发的以太坊客户端Parity、Chronicled（一间区块链公司）发表了以太坊区块链的实物资产验证平台；芯片公司、物理IP创建者和生产者可以用植入的蓝牙或近场通信进行验证。Slock.it（Rent, sell or share anything）的付费充电、或是Airbnb智能租赁。
* 版权授权：Ujo Music平台让创作人用智能合约发布音乐，消费者可以直接付费给创作人。伊莫珍·希普用此平台发布了一首单曲。
* 智能电网：TransActive Grid让用户可以和邻居买卖能源。
* 去中心化期权市场：Etheropt。
* 锚定汇率的代币：DigixDAO提供与黄金挂钩的代币，在2016年4月正式营运。Decentralized Capital提供和各种货币挂钩的代币。
* 移动支付：Everex让外劳汇款回家乡。

这个名单几乎可以无限增加！

