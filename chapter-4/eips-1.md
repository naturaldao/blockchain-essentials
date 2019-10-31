# 4.10 以太坊EIPs

EIPs即以太坊改进提案（Ethereum Improvement Proposals）。根据官方定义，EIPs是以开放协作模式为以太坊平台构建各种标准（包括核心协议规范、客户端API和合约标准）的一个专业化的提案协作区。EIP应阐明相应功能的简明技术规范和基本原理。此外，EIP作者有责任就对应提案在社区内建立共识并记录异议。

简单来说，EIP就是以太坊的发展路线图。但这份路线图不是由某一个人或机构决定的，而是由整个以太坊社区达成共识来决定的。

以太坊官网可以查阅到所有正在建设的和已经完成的提案：

https://eips.ethereum.org/all

或者在Github上也可以找到所有当前已有和草拟的EIPs的可浏览版本：

**https://github.com/ethereum/EIPs/issues**

我们首先应该明白一个可贵的事实：实际上全球精英都已经并将继续参与以太坊的基础建设。譬如在这你就可以一睹为快：

**https://ethereum-magicians.org/**

智能合约开发基地：

https://github.com/ethereum/EIPs

ERC-number是以太坊智能合约标准的名称，有点类比IEEE标准，后面都有个标准的编码。前面我们已经重点介绍了四种智能合约标准。

LES：即Light Ethereum Subprotocol，以太坊轻客户端子协议。

API：即Application Programming Interface，应用程序编程接口。

RPC：即Remote Procedure Call，远程过程调用。

ABI：即Application Binary Interface，应用程序二进制接口。

ERC：即Ethereum Request for Comments，以太坊意见征求。

URI：即Uniform Resource Identifier,统一资源标识符。

### EIP状态术语

Draft（草案）——一个开放的求关注的idea。

Accepted（已接受）——以太坊计划立即采用的一个EIP，如希望被包含在下一个硬分叉里的EIP（核心/共识层的EIP）。

Final（终案）——一个已经被以太坊采用，已被包含在此前的硬分叉中的EIP（核心/共识层的EIP）。

Deferred（延后）——已经被以太坊接纳，但不会立即部署的一个EIP。会在以后的硬分叉里部署。

### EIP的类型

EIPs被分成多个类型，每个类型都有自己的EIP列表。

#### Standard Track（标准追踪类EIP）

标准追踪类EIP用于描述影响大多数或所有以太坊功能执行造成影响的的任何更改，例如对网络协议的改动，区块或交易有效性规则、已提交的应用标准或公约的改动，或影响使用以太坊的应用的互操作性的任何更改或补充。而且，标准追踪类可以进一步细分为以下几种类型

1. **Core（核心）**

核心提案是需要通过社区共识并实行一个分叉才能得以改进的提案（例如EIP5，EIP101），以及不一定是共识攸关（无需进行分叉）的变更，但是可能与“核心开发”的讨论有关（例如，EIP86的矿工或节点策略改变之2、3和4）。

1. **Networking（网络）**

网络提案包括对devp2p \(EIP8\)和Light Ethereum Subprotocol（以太坊轻客户端子协议）相关的改进，以及对“whisper ”和“swarm”的网络协议规范的改进。

1. **Interface（接口）**

接口提案包括与客户端API/RPC规范及标准相关的改进提议，也包括某些语言层面的标准，比如方法名（如EIP6）以及合约ABI。标签“接口”与接口回购协议一致，讨论应该在EIP提交给EIPs存储库之前主要在其存储库中进行。

1. **ERC（ERC提案）**

该提案为应用级标准及惯例，包括智能合约标准，比如代币标准（ERC20），名称登记（ERC26，ERC137），库/软件包格式（EIP82）和钱包格式（EIP75，EIP85）。

#### Informational（信息类EIP）

信息类EIP用于描述以太坊的设计问题，或者为以太坊社区提供一般性指导意见，但并不提出新功能。信息类EIP不一定代表以太坊社区的共识或建议，因此用户和实施者可以自由选择是否忽视信息类EIP或者遵循其中的建议。

#### Meta（元EIP）

元EIP用于描述以太坊相关的流程，或者对这些流程进行更改的建议。这类提案包含程序步骤、指导方针、决策过程的改动，以及以太坊开发工具或环境的变动。我们也可以把元EIP看作是流程类EIP。

元EIP与标准追踪类EIP类似，但仅用于以太坊协议之外的其它领域。该提案可能是一个实施方案，但与以太坊的代码库无关；这些提案通常也需要社区共识，但与信息类EIP不同，它们不是单纯的建议，用户通常不会忽视它们。

在Github上（[https://github.com/ethereum/EIPs](https://github.com/ethereum/EIPs)），我们可以追踪到各个EIP的进展。

### 如何参与以太坊标准的建设

1. 首先查阅EIP-1（[https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1.md](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1.md)）里面对于EIP的流程与要求的说明。
2. 点击右上角的“Fork”，分叉（相当于复制）其文件夹。
3. 将你的EIP添加到刚刚分叉出的文件夹里。这里有一个EIP模板（[https://github.com/ethereum/EIPs/blob/master/eip-X.md](https://github.com/ethereum/EIPs/blob/master/eip-X.md)）。
4. 最后提交一个Pull请求到Ethereum的EIPs文件夹（[https://github.com/ethereum/EIPs](https://github.com/ethereum/EIPs)）。

附注：

LES：即Light Ethereum Subprotocol，以太坊轻客户端子协议。

API：即Application Programming Interface，应用程序编程接口。

RPC：即Remote Procedure Call，远程过程调用。

ABI：即Application Binary Interface，应用程序二进制接口。

ERC：即Ethereum Request for comments，以太坊注释请求。

URI：即Uniform Resource Identifier,统一资源标识符。

##  智能合约和以太坊的价值

我们还没有实现智能合约的所有价值，可以预见的是：

1. 此前的其它任何技术都无法部署智能合约，智能合约是区块链的独门秘技。
2. 自动判断执行，降低人工判断错误或是人为操作改动的风险；如果没有代码错误，智能合约完全可以做到零差错！
3. 降低成本，智能合约在部署和执行时具有显著的低成本优势；譬如在资产批量兑换时，执行阶段可以做到极低成本（只需要支付以太坊gas费用）。
4. 传统合约采用是是事后执行，根据状态决定奖惩的模式；智能合约则属于事前预定执行模式，加上智能合约可以抗人为干扰控制合约涉及的全部资金的特色，使资金的流向完全遵照约定的条款来执行，极大地提升了合约的价值，并将大大减少因传统合约无法严格按条款得到执行，以及违约追责而造成的社会资源和人力成本的巨大浪费！它将大大提升人类社会延续和发展的效率。
5. 智能合约绑定了双方的数字资产，相对传统合约违约成本更高，从而带来了更强的约束力和执行效率。
6. 由于许多不同的智能合约可能想要验证关于其用户的某些属性，因此链上可查的身份变得越来越重要。然而，去中心化的身份存储和点对点的身份传递，以及基于零知识证明的身份验证机制，才会使得身份的存储、调用和验证变得简单和高效。身份与智能合约的绑定，将具有超常的价值！
7. 适用范围更广，可适用于全球范围的协作，传统合约受制于各地法律、人文因素等各种因素，完全无法适应当今全球化的需求。
8. 从去中心化去审查的投融资，到构建全球一体的治理体系，智能合约在这一轮“区块链革命”中，将起到举足轻重的作用！

对于以太坊，我们应该认识到：

1. 目标远大：它矢志要成为区块链领域的安卓。
2. 以太坊的爆发使我们认识到，作为区块链2.0代表的公链的核心根本就不仅仅是TPS，而是还需要有最强大脑们构建的千千万万个智能合约标准。

* TPS到达1百万/秒，并不能解决多少问题，它还需要第三次甚至第四次的进化。
* 我们已经看到智能合约标准带来的应用的惊人的爆发力，因此也明白：智能合约标准的建设始终是以太坊里最重要的核心。
* 智能合约标准的作用在于两个方面：第一，带来新的应用；第二，在公平公正公开透明的去中心化去审查的基础之上爆发应用！
* 目前所有公链里，只有以太坊的智能合约标准的发展欣欣向荣！其它公链几乎一片死寂，它们全部加起来的贡献，都恐怕连以太坊的百分之一都没有。

1. 我们有理由相信：以太坊有潜力成为前所未有的合作博弈竞技场。

* 比特币创造性地带来了去中心化的激励机制，但可惜的是它带来的仍旧是非合作博弈，譬如比特币矿业带来的“公地悲剧”和“囚徒困境”，还有比特币发展中的“猎鹿博弈”、“懦夫博弈”等等，已经是非常明显，而且已经被业界作为经典案例。
* 智能合约恰好可以充当一个不会被收买、无处不在的外部监督者，用于执行和监督利益相关者之间的协议。理论上以太坊可以将任何非合作博弈变成合作博弈。在智能合约强大的作用下，此前的利益格局会被打破！
* 从非合作到合作博弈的转变是通过一种名为 Game Warping 的技术实现的，因为智能合约让以太坊成为一个可以根据规则启动的透明、可触发、不可外部干预的链上清算支付系统。它使得合作博弈成为一个合乎逻辑的选择。
* 在以太坊和智能合约可用之前，我们很难找到这样普适且可信的第三方工具。我们现在要做好的，只是要找到适合的机制让博弈的关键操作与链上合约绑定（智能合约标准的建设即为以太坊最重要也最有价值的工作），即可将现实的博弈才非合作变为合作！

总而言之，从囚徒困境到合作博弈，在比特币的基础之上，以太坊为区块链的发展带来了质的飞跃以及无可限量的美好未来！

## 课外阅读

EIP提案说明

[https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1.md](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1.md)

## 参考资料

ERC通证标准全系列（简明介绍版），Dirac：

[https://zhuanlan.zhihu.com/p/49028177](https://zhuanlan.zhihu.com/p/49028177)

Ethereum is game-changing technology, literally.

[https://medium.com/@virgilgr/ethereum-is-game-changing-technology-literally-d67e01a01cf8](https://medium.com/@virgilgr/ethereum-is-game-changing-technology-literally-d67e01a01cf8)

Smart Contracts

[https://www.investopedia.com/terms/s/smart-contracts.asp](https://www.investopedia.com/terms/s/smart-contracts.asp)

