# 4.10 以太坊EIPs

## 什么是EIPs？

EIPs即以太坊改进提案（Ethereum Improvement Proposals）。根据官方定义，EIPs是以开放协作模式为以太坊平台构建各种标准（包括核心协议规范、客户端API和合约标准）的一个专业化的提案协作区。EIP应阐明相应功能的简明技术规范和基本原理。此外，EIP作者有责任就对应提案在社区内建立共识并记录异议。

### **举例：**ERC-165

_状态：已定稿（Final）_

_提交记录：https://github.com/ethereum/EIPs/issues/165_

_标准说明：https://github.com/ethereum/EIPs/blob/master/EIPS/eip-165.md_

_ERC-165是一个标准接口检测协议。_

_ERC-165标准提供一个标准的接口来发布和检测智能合约方法的实现情况。此标准的提出是为了解决某些 ERC 标准对于接口实现情况查询、接口版本查询的需求。ERC-165协议中标准化了以下几条：_

* _如何识别接口；_
* _一个智能合约如何发布它执行的接口；_
* _如何检测一个智能合约是否执行了ERC-165协议；_
* _如何检测一个智能合约是否执行了一个给定的接口；_

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

1. Core（核心）

   核心提案是需要通过社区共识并实行一个分叉才能得以改进的提案（例如EIP5，EIP101），以及不一定是共识攸关（无需进行分叉）的变更，但是可能与“核心开发”的讨论有关（例如，EIP86的矿工或节点策略改变之2、3和4）。  

2. Networking（网络） 网络提案包括对devp2p \(EIP8\)和Light Ethereum Subprotocol（以太坊轻客户端子协议）相关的改进，以及对“whisper ”和“swarm”的网络协议规范的改进。 
3. Interface（接口） 接口提案包括与客户端API/RPC规范及标准相关的改进提议，也包括某些语言层面的标准，比如方法名（如EIP6）以及合约ABI。标签“接口”与接口回购协议一致，讨论应该在EIP提交给EIPs存储库之前主要在其存储库中进行。 
4. ERC（ERC提案） 该提案为应用级标准及惯例，包括智能合约标准，比如代币标准（ERC20），名称登记（ERC26，ERC137），库/软件包格式（EIP82）和钱包格式（EIP75，EIP85）。

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

