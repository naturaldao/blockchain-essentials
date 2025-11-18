---
description: V神还是你家大神！另外，偶尔你不能只考虑技术，还得想想未来的人类未来的社会如何管理。
---

# 4.11 账户抽象化：EIP-86、EIP-2938、EIP-3074、ERC-4337、RIP-7560、ERC-7702

## 账户抽象化（Account Abstraction）的**历史** <a href="#eip2938" id="eip2938"></a>

### EIP-86

账户抽象化这个概念是在 2017 年2月 Vitalik Buterin 的 [EIP-86](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-86.md) 里首次提出的，其目的是实现 “交易来源和签名的抽象”。不过其动机和想法可以追溯到 2016 年年初 vitalik 提交的一个[ issue](https://github.com/ethereum/EIPs/issues/86)， 文中建议 “与其将 ECDSA 签名算法和默认的 nonce 机制写死在协议内作为 ‘标准’ 的账户安全机制，不如初步建立一个（统一的）账户模型，在未来把所有的账户都变成合约，让合约可以支付 gas，让用户可以自由定义自己的安全模型。”

> #### Rationale
>
> The goal of these changes is to set the stage for abstraction of account security. Instead of having an in-protocol mechanism where ECDSA and the default nonce scheme are enshrined as the only "standard" way to secure an account, we take initial steps toward a model where in the long term all accounts are contracts, contracts can pay for gas, and users are free to define their own security model.

什么是天才？凭借直觉就能预判准未来，那他就是！

前面有关章节已经介绍以太坊有两种类型的账户：

* 外部账户（Externally Owned Account, EOA）
* 合约账户（Contract Account, CA）。

前者由用户私钥控制，而后者由存储在智能合约账户（有时也被称为智能钱包）内的合约代码控制。

外部账户的权限要大于合约账户，因为只有外部账户可以通过支付 gas 启动交易的执行过程（即与智能合约交互）。

随着实践经验与教训的积累，钱包用户开始对外部账户产生一些担忧。这些担忧包括：

1. 严重依赖人工：由于账户访问完全依靠私钥，且所有交易均需动用私钥进行签名，即转账和实施智能合约操作的唯一方法是人工操作（使用私钥），在全天无休的区块链行业，这就实在太不方便。甚至完全可以说手段太落后了。
2. 私钥管理问题：既然私钥决定一切，对于如何安全存储私钥以及谁有权访问它，就会给用户带来极困难和极具压力的挑战——如果有50亿地球人都得日常使用私钥，因私钥掌控Money，那么每天必定有大量私钥的遗失和泄露事件发生！如果不泄露就绑架就凶杀……
3. 依赖椭圆曲线签名（ECDSA加密算法）：已经有专业团队发出外部账户采用的ECDSA加密算法会在不久的将来被破解的预警！抗量子的数字签名是对当前 ECDSA 的必要改进，并且可以说迫在眉睫！该提案可以让用户升级至更安全的加密算法。但能否顺利升级，能否赶在被破解前完成升级换代，目前看起来胜算都不大！……那咋办呢？
4. 一对一的操作：一次不能执行多个操作。否则会产生不必要的成本和糟糕的用户体验。外部账户一点都不好玩！

值得思考的是，Vitalik 这个提案的核心思想，是在遥远的未来，已经没了外部账户而所有的账户均为合约账户，让合约账户可以支付gas，所有人都不用应对与私钥相关的安全问题。

### EIP-2938: Account Abstraction

改造链上交易类型的 [EIP-86](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-86.md) 给大家带来了巨大的启迪，类似它的方案此后又出现了EIP-101、EIP-208、EIP-859、EIP-2718。

2020年，Vitalik 等人提出了 [EIP-2938](https://eips.ethereum.org/EIPS/eip-2938) 草案，该草案概述了一个更简单的实现。这个实现对协议/共识的改动最小，并且通过设置节点的内存池规则来满足所需的安全保证——开发小组的动机是将它“封装”到以太坊共识层（Consensus Layer），而非像通证标准ERC-20那样，仅仅只是提供一个在智能合约层面应用的技术标准。

[EIP-2938](https://eips.ethereum.org/EIPS/eip-2938) 提出的账户抽象化（Account Abstraction, AA）是一个可以让合约账户成为和外部账户一样的 “顶层” 账户的提案。具体来说，这个方案要求对以太坊协议进行修改，并允许从合约（每个智能合约都有一个合约账户），而不仅仅是外部账户来发起以太坊交易。合约本身将具备验证和矿工需验证的gas费用支付逻辑。那么也就是说，实现了账户抽象化之后，合约账户也可以发起交互请求并支付交易费。

而实际的应用场景更复杂，考虑到遥远的未来可以说细思极恐：EIP-2938通过账户抽象化来为以太坊提供一个基础功能层，以确定何时支付，谁来支付以及怎样支付 gas——简单地说，这是一个以改造链上主体对象为核心的解决方案。如果你还没感觉，那就仔细看完（特别是ERC-4337）。

EIP-2938可以让更多通用型钱包或者其它应用的智能合约执行复杂的逻辑。譬如没有EIP-2938，你想把所有代币都放到一个新钱包里，然后你一不小心先把所有 ETH 都发送到这个新钱包里了。马上，你会尴尬地发现你的旧钱包因为没有 ETH 而无法发送任何交易，剩下的代币无法转移到新钱包。账户抽象化能让你使用其它合约地址（如智能合约钱包）里的 ETH 支付 gas 费用（这种情况叫代付交易，Sponsored Transactions），甚至还可以让你使用旧钱包里的其它代币支付 gas 费用。当然，这只是一个很容易理解的示例，实际的应用还可以复杂很多倍。

### EIP-3074: AUTH and AUTHCALL opcodes...

[EIP-3074](https://eips.ethereum.org/EIPS/eip-3074) 主要的技术创新是向 [以太坊虚拟机（EVM）](https://ethereum.org/zh/developers/docs/evm/) 添加两个新的操作码：AUTH 和 AUTHCALL，它们旨在被称为“调用者”的智能合约使用。这些调用者获得了对授权他们的外部账户 EOA 的控制权，并可以代表他们进行调用。

这也就是说，用户可使用私钥对包含其意图的消息进行签名。然后，该消息被包含在调用程序的链上交易中。调用者使用签名消息和 AUTH 操作码来控制用户的外部帐户，并且使用 AUTHCALL 代表用户去执行操作。

要注意的是，包含用户签名消息的交易不一定必须从该用户的帐户发送，这使用户完全不必依赖 ETH 来发送交易。事实上，还可以通过其他方式支付费用，例如使用道易程创建的伟大的**价格单位制**里面的那个叫做uToken的 ERC-20 代币。

这种方法有几个问题，主要是认为赋予调用者如此大的权力会导致类似于 DAO 黑客攻击的灾难性事件。

之后的EIP-3607、EIP-5003基本沿袭了 EIP-3074 的内核思想。

### ERC-4337: Account Abstraction Using Alt Mempool...

1. [ERC-4337](https://eips.ethereum.org/EIPS/eip-4337) 看上去与EIP-2938类似，但ERC-4337是无需更改任何共识层即可在协议上实现账户抽象的技术标准。它显然是更好的解决方案。EIP-4337最早于2021年提出，已经于2023年3月被部署到以太坊主网，可实现在单个合约账户中进行交易和创建合约。
2. [ERC-4337](https://eips.ethereum.org/EIPS/eip-4337) 的核心技术创新是用一个被称为 UserOperation 的更高层伪交易对象，取代了 transaction ，而它本身则是一个描述一个 transaction 结构体。你可以理解为 ERC-4337 为以太坊在 transaction 基础上，新创了另一种交易形式。或者说比特币只有一种办法交易，而以太坊现在有了另一种更复杂也更高效的形式完成交易。以太坊对比特币的超越是全面的！并且，把这个场景放到智能合约的应用里去考虑就叫细思极恐。是的，我词穷了，你能理解就好！
3. validateUserOp：它接受一个 UserOperation 作为输入。这个函数应该验证UserOperation上的签名和nonce，如果验证成功则支付费用并增加nonce，如果验证失败则抛出异常。
4. op执行功能：将calldata解释为钱包采取行动的指令。这个函数如何解释calldata以及它的结果是完全开放的；但我们预计最常见的行为是将calldata解析为钱包的一个或多个调用。
5. 捆绑者(Bundler)将这些对象打包成一笔交易，纳入到一个区块当中。捆绑者支付捆绑交易的燃料费，但收取单独执行UserOperation的费用。捆绑者与验证者的工作方式类似，即根据费用优先等级逻辑选择要纳入的对象。任意捆绑者均可参与到流程当中。
6. 为了简化钱包的逻辑，确保安全所需的大部分复杂智能合约技巧不是在钱包本身中完成的，而是在称为入口点的全局合约中完成的。validateUserOp 和执行函数预计将使用 require(msg.sender == ENTRY\_POINT) 进行门控，因此只有受信任的入口点才能使钱包执行任何操作或支付费用。入口点仅在 validateUserOp 之后对钱包进行任意调用，并且携带该调用数据的 UserOperation 已经成功，因此这足以保护钱包免受攻击。如果钱包不存在，入口点还负责使用提供的 initCode 创建钱包。
7. 启用创新用例：包括聚合签名、每日交易限额设置、账户紧急冻结、白名单设置以及保护隐私的应用程序等。

### **ERC-4337的优缺点**

#### **ERC-4337可实现的一些亮点：**

1. 钱包设置——无需写下助记词。只需轻点几下，即可快捷轻松地进行设置。
2. 账户恢复——用户无需再担心丢失助记词，现已可以实现多重身份验证和账户恢复。
3. 易于使用的钱包功能——用户可以享用非常丰富的定制服务，包括自动支付、预先批准交易和捆绑交易。至于还有什么功能，完全取决于你写的智能合约。
4. 更高的安全性——降低人为出错的几率，钱包将能够更加安全。
5. 更灵活的gas支付方式——由ERC-4337提供支持的钱包可用任意ERC-20代币、EIP-3712代币和未来其它币种支付gas。

#### **ERC-4337的缺点：**

最突出的一点就是因为其复杂性增加，需要更高的Gas成本：基本的ERC-4337操作约需要42000 gas，而常规交易需要 21000 gas，原因如下：

1、需要支付大量的单个存储读/写成本，在 EOA 的情况下，这些成本会捆绑到一笔 21000 gas 的付款中：\
（1）编辑包含 pubkey+nonce (\~5000) 的存储 slot；\
（2）用户操作调用数据成本（约 4500，通过压缩可减少到约 2500）；\
（3）ECRECOVER (\~3000)；\
（4）首次访问钱包本身 (\~2600)\
（5）首次访问收款人账户 (\~2600)\
（6）将 ETH 转入收款人账户 (\~9000)\
（7）编辑存储以支付费用（\~5000）\
（8）访问包含代理 (\~2100) 的存储 slot，然后访问代理本身 (\~2600)；

2、除了上述存储读/写成本之外，合约还需要执行 “业务逻辑”（解包 UserOperation、对其进行哈希、洗牌变量等）

3、需要消耗 gas 来支付日志费用（EOA 不发布日志）；

4、一次性合约创建成本（约 32000 gas，加上代理中每个 code byte 200 gas，再加上设置代理地址的 20000 gas）\
简而言之，账户抽象地址的每一步都需要计算，需要消耗更多的资源，也增加了额外的费用。

### ERC-5189

还没完？

[ERC-5189](https://eips.ethereum.org/EIPS/eip-5189) 显然是受 ERC-4337 的启发而创建的，但它卡顿了。如果你对技术感兴趣可以点击链接去看看。

### **RIP-7560:** Native Account Abstraction

Title为Native Account Abstraction，意味着和 [EIP-2938](https://eips.ethereum.org/EIPS/eip-2938) 一样，它是一个“封装”到以太坊共识层的技术提案。

[RIP-7560 ](https://github.com/ethereum/RIPs/blob/e3bead34f1bcf1aa37fd51923ad99a77b801775c/RIPS/rip-7560.md#unused-gas-penalty-charge)简介里的第一句话为：

Combining the [EIP-2938](https://github.com/ethereum/RIPs/blob/e3bead34f1bcf1aa37fd51923ad99a77b801775c/RIPS/eip-2938) and [ERC-4337](https://github.com/ethereum/RIPs/blob/e3bead34f1bcf1aa37fd51923ad99a77b801775c/RIPS/eip-4337) into a comprehensive Native Account Abstraction proposal.

该提案的意图，是引入共识层协议变更的原生账户抽象（Native Account Abstraction），并将 EIP-2938 和 ERC-4337 合并为一个全面的账户抽象提案。

它是以[EIP-4337](https://eips.ethereum.org/EIPS/eip-4337), [EIP-6780](https://eips.ethereum.org/EIPS/eip-6780)为基础。

### EIP-7701: Native Account Abstraction with EOF

[EIP-7701 ](https://eips.ethereum.org/EIPS/eip-7701)有个小标题：A variant of RIP-7560 transactions relying on EOF Smart Contract Accounts

它是以[EIP-3540](https://eips.ethereum.org/EIPS/eip-3540)为基础。

### EIP-7702: Set EOA account code for one transaction

讨论前先说明下，

1. 目前EIP-7702还只是一个草案。最终方案请以该页面的内容为准：

{% embed url="https://eips.ethereum.org/EIPS/eip-7702" %}

2. 技术标准常常不是直接在应用中照本宣科，或者依葫芦画瓢！一个没有区块链基础知识的人是绝不可能用2024、2025年的AI开发好智能合约的，那样项目有怎样的安全漏洞，只有天知道！

它有个令人惊讶的小标题：Add a new tx type that sets the code for an EOA during one transaction execution。

它的简介也令人惊讶：

Add a new transaction type that adds a `contract_code` field and a signature, and converts the signing account (not necessarily the same as the `tx.origin`) into a smart contract wallet for the duration of that transaction. Intended to offer similar functionality to [EIP-3074](https://eips.ethereum.org/EIPS/eip-3074).

看似简单，但一石惊起千层浪！

[EIP-7702](https://eips.ethereum.org/EIPS/eip-7702) 提出了一种同时接受 contract\_code 和签名字段的新交易类型，在开始执行交易时，它将签名者账户的合约代码设置为 contract\_code。在交易结束时，它会将代码重新设置为空。

EIP-7702和 EIP-3074 一样，实现了 EOA 对智能合约的临时委托功能。然而 EIP-7702 并没有引入新的操作码（这需要硬分叉），而是定义了要调用的函数：

AUTH -> 调用“verify”（验证）&#x20;

AUTHCALL -> 调用“execute”（执行）

具体来说，它：

```
* 检查你的账户合约代码是否为空；
* 如果为空，则设置为提供的合约代码；
* 根据提供的智能合约处理交易的方式执行交易；
* 将账户合约代码设置恢复为空。
```

由此可见，EIP-7702 能够达成与 EIP-3074 同样的核心功能，如批量交易和交易赞助，增强了交易类型的复杂性和控制策略的灵活性。这也就是说，EIP-7702 允许外部拥有账户（EOAs）在交易中临时扮演智能合约钱包的角色，使得 EOAs 可以执行以前只有智能合约才能进行的复杂操作，极大地增强了 EOAs 的功能性和灵活性。通过引入 contract\_code 字段，EOAs 可以在交易中动态地引入智能合约代码，实现在单一交易中完成多种操作，如批量处理和复杂的交易指令，从而简化流程并降低交易成本，同时减少了操作复杂性。

其次，EIP-7702 还引入了一种新的权限管理机制，即权限降级。这使得账户持有者可以为其子密钥分配具体权限，从而增强了账户的安全性。通过细粒度的权限控制，用户可以限制子密钥的操作范围，防止未授权的交易和滥用行为，旨在保护用户资金安全，这对于防止未授权的交易和滥用具有重要意义。

EIP-7702 相比 EIP-3074 的优势是，它与EIP-4337 构建的所有账户抽象工作高度兼容，“用户需要签名的合约代码实际上可以是现有的 EIP-4337 钱包代码”。

一旦此项改动生效，用户现有的 EOA 就可以执行任何智能合约代码。通过额外的 EIP，EOA 还可以永久升级以运行特定的代码。

## **创新**

账户抽象化为包括新型钱包在内的区块链dApp的创新设计打开了新的思路。也许有助于下列技术方案的实施：

* 多签和账户的社交恢复
* 验证逻辑灵活性——更高效和更简洁的签名算法（如：Schnorr, BLS）
*   执行层量子安全与后量子安全签名算法（如：Lamport, Winternitz）

    如果EIP-2938这样的提案得到普遍采用，则无需为量子安全在执行层做进一步的工作。用户可将钱包自行升级到量子安全的版本。甚至连封装交易（wrapper transaction）也是安全的。矿工可以为每个捆绑交易使用新创建的EOA，由于是新创建的，所以每个交易都受到哈希保护，更保证了安全性。并且，在矿工将交易添加到区块中之前不会发布该交易。\
    对于其它方案，则始终要考虑到外部账户的加密算法，能够被量子攻击所破解！
*   钱包可升级性及其执行逻辑灵活性

    钱包验证逻辑可以是有状态的，因此钱包可以更改其公钥或（譬如如果使用DELEGATECALL发布）完全升级其代码。

    钱包可以为执行步骤添加自定义逻辑，例如进行原子多操作（这是EIP 3074的一个关键目标）。
*   EIP-7702提议对以太坊协议进行一系列更改，以实现账户抽象。其关键思想是引入一种称为“用户操作”的新交易类型。与普通交易不同，用户操作不是通过签名进行身份验证的。相反，它们是由与账户关联的智能合约钱包进行身份验证的。

    当用户操作提交到网络时，首先会发送到关联的智能合约钱包。然后，钱包根据自己的验证逻辑验证操作。如果验证成功，则钱包向用户操作的目标发送普通交易。

    这一过程允许更灵活地验证交易。例如，智能合约钱包可以要求在执行交易之前进行多重签名，或者可以使用链下数据（如生物识别验证的证明）来验证交易。

    EIP-7702还引入了“支付主合约”的概念。这些是可以为用户操作赞助gas费用的合约。这意味着只要有一个愿意支付其gas费用的支付主，用户就可以与以太坊进行交互，而无需持有任何ETH。
* 智能合约的应用升级\
  这句话需要你自己去想象……

## **技术改进的根本动机**

账户抽象化背后的动机很简单，但会带来根本性的改变：当前，以太坊交易具备功能可编程性（通过调用智能合约实现），但是交易的验证方式却是固定的。只有持有有效的 ECDSA 签名、有效的 nonce 值以及足够的账户余额，一笔交易才算有效。账户抽象化引入了一种新的交易类型 —— 抽象账户交易（AA Transaction）。这种交易总是由一个特殊地址产生，协议不会检查其签名，nonce 和余额。通过引入这种交易，账户抽象化实现了从固定验证方式到可编程验证方式的转变。抽象账户交易的有效性由其 target 字段指定的智能合约验证，通过验证之后，合约可以自行为该交易支付手续费。

我们业已看到，随着技术探讨的深入，一个小小的突破口，带来了很大的一个未来世界！

这就叫创新！并且它对于区块链未来的发展极为重要！

## 小结

1. EIP-86、EIP-101、EIP-859、EIP-2718、EIP-2938、EIP-3074、EIP-3607、EIP-4337、EIP-5003、EIP-5189，RIP-7560……其中EIP-86、EIP-2938、EIP-4337、RIP-7560都是 Vitalik 领衔制订，前后耗费七八年时间。创新不易！
2. 以太坊封装 ERC-4337？\
   对于开发者来说，这是值得重视和学习的！\
   请仔细阅读[以太坊是否应该封装更多功能？](https://mp.weixin.qq.com/s/Sshg-KP3sm7JLq7RgCvGqw)（注意“**封装 ERC-4337”一节** ）\
   ——原文：[Should Ethereum be okay with enshrining more things in the protocol?](https://vitalik.eth.limo/general/2023/09/30/enshrinement.html)\
   此后 RIP-7560 的出现，证实了封装势在必行。
3. 提案即技术协议，它的不断变化，可能会给开发带来不良影响！这篇文章里面有提到一些细节：[《RIP-7560 ：从共识层实现标准化的原生账户抽象》](https://www.aicoin.com/article/379381.html)。
4. 账户抽象化的出发点，是想要显著改善用户与dApp的交互体验。它客观上有个巨大的应用优点：减少外部账户也就是人类的缺陷和失误带来的不良影响——考虑到未来合约满天飞带来的超高效率的交互，我们甚至可以说它很可能让人类对dApp的影响降低到极小化。并且，未来肯定还会通过一次次创新持续降级外部账户（或者使之产生蜕变），而优化dApp的效率和安全性。
5. 对使用 ERC-4337 达到社交恢复用户账户这样的应用，要特别小心！因为我们说到社交恢复，绝大多数俗人理解的就是几个人能够联合起来恢复某个用户的账户，或者赋予某个用户的一个新账户接收款项的权利——万一这几个人中有数人同时挂掉呢？万一他们联合起来作恶呢？\
   如果你不但这样想还这样做，那意外就迟早会发生，而且大概率还是灾难级别越大发生的可能性越高——很遗憾我们有时候确实没法阻止它就这样发生。\
   而如果里面不是人，而是一个AI一个地址，这些AI能够飞到你的身边验明正身，由它们来掌握恢复人类账户的权限，你觉得如何？

### 参考文献

## What is EIP-7702? A Beginner's Guide

* [![Juan Leal](https://blog.thirdweb.com/content/images/size/w100/2024/03/Screenshot-2024-01-01-at-8.01.04-PM.png)](https://blog.thirdweb.com/author/juan/)

[**Juan Leal**](https://blog.thirdweb.com/author/juan/)

May 14, 2024 • 7 min read

<figure><img src="https://images.bannerbear.com/direct/5gd4bWZ6D8W1RxEQBD/requests/000/055/762/355/9BvRDJ724zWWmnmgzlAKNOd03/66654b4c8906784dc8aa448334022108eda33fd5.png" alt="What is EIP-7702? A Beginner&#x27;s Guide"><figcaption></figcaption></figure>

Ethereum's evolution never stops. With each [Ethereum Improvement Proposal (EIP)](https://blog.thirdweb.com/native-account-abstraction-rip-7560/), the network takes a step towards greater scalability, security, and usability. One such proposal, EIP-7702, is set to revolutionize the way users interact with the Ethereum blockchain by introducing [account abstraction](https://ethereum.org/en/developers/docs/accounts/?ref=blog.thirdweb.com).

Account abstraction is a concept that has been discussed in the Ethereum community for a while, but EIP-7702 brings it closer to reality. The proposal aims to provide a more flexible and user-friendly experience by allowing [smart contracts](https://ethereum.org/en/developers/docs/smart-contracts/?ref=blog.thirdweb.com) to act as Ethereum accounts. This shift could potentially unlock a myriad of new use cases and greatly enhance the overall user experience on Ethereum.

In this blog post, we'll take a deep dive into EIP-7702, explaining what account abstraction is, how it works, and what benefits it brings to developers and users. We'll also explore how thirdweb's tools and services can help developers navigate this new terrain and take full advantage of the opportunities presented by EIP-7702.

### Decoding Account Abstraction <a href="#decoding-account-abstraction" id="decoding-account-abstraction"></a>

Before we delve into the specifics of EIP-7702, let's first understand what account abstraction is. In the current Ethereum ecosystem, there are two types of accounts: externally owned accounts (EOAs) and contract accounts. EOAs are controlled by private keys and can initiate transactions, while contract accounts are controlled by their contract code and can only perform transactions in response to receiving a transaction.

Account abstraction aims to blur the distinction between these two types of accounts. With account abstraction, all accounts on Ethereum can be treated as contract accounts. This means that EOAs can be replaced by [smart contract wallets](https://ethereum.org/en/developers/docs/accounts/?ref=blog.thirdweb.com#contract-accounts), which can have arbitrary verification logic for validating transactions.

This shift brings several benefits. For one, it allows for more flexibility in how transactions are authorized. Instead of relying solely on private keys, smart contract wallets can define their own authentication methods, such as multi-signature schemes, social recovery, or biometric authentication.

Furthermore, account abstraction enables new features like gas sponsorship, where a third party can pay for the gas fees of a transaction, and account recovery mechanisms, which can help users regain access to their funds if they lose their private keys.

### Exploring EIP-7702 <a href="#exploring-eip-7702" id="exploring-eip-7702"></a>

Now that we have a basic understanding of account abstraction, let's take a closer look at EIP-7702 itself. The full technical specification of the proposal can be found in the [Official EIP-7702 Proposal](https://eips.ethereum.org/EIPS/eip-7702?ref=blog.thirdweb.com).

EIP-7702 proposes a set of changes to the Ethereum protocol that would enable account abstraction. The key idea is to introduce a new type of transaction called a "user operation." Unlike regular transactions, user operations are not authenticated with a signature. Instead, they are authenticated by a smart contract wallet associated with the account.

When a user operation is submitted to the network, it is first sent to the associated smart contract wallet. The wallet then validates the operation based on its own verification logic. If the validation succeeds, the wallet sends a regular transaction to the target of the user operation.

This process allows for much more flexibility in how transactions are validated. For example, a smart contract wallet could require multiple signatures before a transaction is executed, or it could use off-chain data (like a proof of biometric authentication) to validate a transaction.

EIP-7702 also introduces the concept of "paymaster contracts." These are contracts that can sponsor the gas fees for user operations. This means that users can interact with Ethereum without needing to hold any ETH, as long as there is a paymaster willing to cover their gas fees.

### EIP-7702's Impact on Developers and Users <a href="#eip-7702s-impact-on-developers-and-users" id="eip-7702s-impact-on-developers-and-users"></a>

The changes proposed by EIP-7702 have significant implications for both developers and users of Ethereum.

For developers, EIP-7702 brings a new level of flexibility in designing smart contract wallets. They can implement custom verification logic, enabling features like multi-factor authentication, spending limits, and emergency recovery mechanisms. This opens up a whole new design space for smart contract wallets.

Moreover, the ability to sponsor gas fees through paymaster contracts allows developers to create more user-friendly onboarding experiences. New users can interact with Ethereum applications without needing to acquire ETH first, reducing a significant barrier to entry.

For users, EIP-7702 promises a more seamless and secure Ethereum experience. Smart contract wallets can offer better safety features, like protection against phishing attacks and the ability to recover funds if private keys are lost. Gas sponsorship also makes it easier for users to get started with Ethereum, as they don't need to worry about purchasing ETH for gas fees.

However, these benefits come with some challenges. Implementing account abstraction adds complexity to Ethereum clients and wallets. There are also questions around how to optimize gas costs for user operations and how to ensure that paymaster contracts are not abused.

### Technical and Adoption Challenges <a href="#technical-and-adoption-challenges" id="technical-and-adoption-challenges"></a>

While EIP-7702 presents a plethora of benefits, it's not without its challenges. One of the main hurdles is the added complexity it brings to Ethereum clients and wallets. Implementing account abstraction requires significant changes to the existing infrastructure, which could lead to potential compatibility issues and increased development overhead.

Another challenge is optimizing gas costs for user operations. Since smart contract wallets will be performing additional computations to validate transactions, this could lead to higher gas costs compared to traditional EOA transactions. Developers will need to find ways to minimize these costs, possibly through gas optimization techniques or by utilizing gas sponsorship mechanisms.

There's also the question of how to prevent abuse of paymaster contracts. While gas sponsorship is a powerful feature, it could potentially be exploited by malicious actors. Safeguards will need to be put in place to ensure that paymaster contracts are used responsibly and don't enable spam or denial-of-service attacks.

Adoption of EIP-7702 will also require buy-in from the broader Ethereum community. Wallets, dapps, and other ecosystem participants will need to update their software to support the new transaction types and interactions introduced by account abstraction. This process will take time and require coordination across the community.

### Leveraging thirdweb for Enhanced Account Abstraction <a href="#leveraging-thirdweb-for-enhanced-account-abstraction" id="leveraging-thirdweb-for-enhanced-account-abstraction"></a>

Despite these challenges, the benefits of account abstraction are compelling, and tools like [thirdweb](https://thirdweb.com/?ref=blog.thirdweb.com) are making it easier for developers to take advantage of these features.

thirdweb provides a suite of developer tools and infrastructure that simplify the process of building and deploying smart contracts on Ethereum. With thirdweb, developers can easily create [smart contract wallets](https://blog.thirdweb.com/smart-contract-wallet-erc4337/) that leverage the capabilities of EIP-7702.

For instance, thirdweb's [Solidity SDKs](https://portal.thirdweb.com/solidity?ref=blog.thirdweb.com) include pre-built, audited smart contract templates for common use cases like multi-signature wallets, escrow contracts, and token vesting. These templates can be easily extended to incorporate custom verification logic and advanced features enabled by account abstraction.

thirdweb also offers [gasless transactions](https://blog.thirdweb.com/gasless-transactions-on-the-blockchain/), a feature that aligns perfectly with the gas sponsorship capabilities of EIP-7702. With gasless transactions, users can interact with smart contracts without needing to hold ETH for gas fees. This feature, combined with the flexibility of smart contract wallets, could greatly improve the onboarding experience for new Ethereum users.

Furthermore, thirdweb's infrastructure is designed to handle the increased computational requirements of account abstraction. Their [serverless architecture](https://blog.thirdweb.com/serverless-functions-overview/) can scale automatically to meet the demands of user operations and smart contract wallet interactions.

### Ethereum's Future alongside Account Abstraction <a href="#ethereums-future-alongside-account-abstraction" id="ethereums-future-alongside-account-abstraction"></a>

As EIP-7702 and the concept of account abstraction gain traction, it's exciting to consider how they might shape Ethereum's future.

One of the most promising implications is a vastly improved user experience. With smart contract wallets, users will have access to advanced security features, customizable transaction validation, and the ability to interact with Ethereum without needing to manage ETH for gas directly. This could make Ethereum much more approachable and user-friendly, lowering the barriers to entry for mainstream adoption.

Account abstraction also unlocks new possibilities for developers. The ability to define custom transaction validation logic opens up a whole new design space for smart contract interactions. We could see the emergence of new types of dapps and use cases that were previously not possible or practical with traditional EOA accounts.

For example, account abstraction could enable more sophisticated [identity and reputation systems](https://blog.thirdweb.com/what-is-a-self-sovereign-identity/). Smart contract wallets could incorporate identity verification, credit scoring, or other off-chain data into their transaction validation process. This could pave the way for decentralized credit markets, [self-sovereign identity](https://blog.thirdweb.com/what-is-a-self-sovereign-identity/), and more robust [decentralized governance](https://blog.thirdweb.com/governance-sdk/) models.

Gas sponsorship also introduces new economic models and incentive structures. We could see the rise of "gas relayers" - entities that specialize in sponsoring transactions in exchange for other forms of value (e.g., token rewards, service fees, etc.). This could create new revenue streams and business models within the Ethereum ecosystem.

Of course, the full impact of account abstraction will depend on the rate and scale of adoption. But with proposals like EIP-7702 and the growing ecosystem of tools like thirdweb, the future looks bright. As more developers start experimenting with these new capabilities, we're likely to see a wave of innovation that could redefine what's possible on Ethereum.

### Conclusion <a href="#conclusion" id="conclusion"></a>

As the Ethereum ecosystem continues to evolve, EIP-7702 and the concept of account abstraction represent a significant step forward in terms of usability, security, and flexibility. By blurring the lines between EOAs and contract accounts, account abstraction opens up a world of possibilities for developers and users alike.

For developers, EIP-7702 provides a new canvas to create innovative smart contract wallets with custom verification logic, advanced security features, and improved user onboarding. Tools like [thirdweb](https://thirdweb.com/?ref=blog.thirdweb.com) are already making it easier for developers to integrate these features into their projects, providing pre-built templates, gasless transactions, and scalable infrastructure.

For users, account abstraction promises a more intuitive and secure Ethereum experience. Smart contract wallets can offer better protection against common vulnerabilities, more flexible authentication methods, and the ability to interact with Ethereum without the need to directly manage ETH for gas fees. This could significantly lower the barriers to entry for mainstream adoption.

As EIP-7702 moves closer to implementation, it's clear that account abstraction will play a central role in shaping Ethereum's future. While there are still challenges to overcome, such as increased complexity and potential gas optimization issues, the benefits are too compelling to ignore. With the right tools and community support, account abstraction could unlock a new era of innovation and growth for Ethereum.

#### Related Reading <a href="#related-reading" id="related-reading"></a>

[What is EIP-7702? A Beginner's Guide](https://blog.thirdweb.com/eip-7702/)

[以太坊账户抽象研报：拆解 10 个相关 EIP 提案与冲击千万级日活用户的瓶颈问题](https://www.8btc.com/article/6788664)

[ERC 4337：无需更改以太坊协议的账户抽象](https://www.odaily.news/post/5174238)

[zkSync Era Account Abstraction](https://github.com/Dapp-Learning-DAO/Dapp-Learning-zkSync/tree/main/Lesson02)

[EIP-7702 对你意味着什么？第一部分 -- 7702 的采纳周期](https://learnblockchain.cn/article/13293)

[EIP-7702 对 MetaMask 和其他钱包提供商意味着什么](https://learnblockchain.cn/article/12040)

[EIP-7702宣传网站](https://eip7702.io/)\
