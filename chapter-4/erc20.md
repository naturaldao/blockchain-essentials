# 4.5 ERC-20通证标准及其爆发性应用

## ERC-20通证标准

ERC-20（又常被写作ERC20）通证标准是在以太坊平台上通过智能合约制订的通证标准。

我们常常把以ERC-20通证标准发行出来的通证，叫做“代币”。

ERC20让开发者能够基于智能合约执行以下操作：

1. 制定代币总供应量
2. 获得账户余额
3. 转让代币
4. 批准花费代币（可以锁定，达到条件解锁后才能花费）

以下内容翻译自：[https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md)

1.  **概述**

    ERC20是token的一种标准接口。
2.  **摘要**

    本标准允许在智能合约中部署token的标准API。 该标准提供了转移token的基本功能，并允许token被批准，以便链上其它第三方可以使用它们。
3.  **动机**

    这一标准接口可以让以太网上的任何token可以被其他应用程序再利用：从钱包到去中心化的交易所。
4.  **技术参数**

    **1）token**

    1.1. 方法

    **注意：**调用者必须处理returns（bool success）返回的false。调用者绝对不能假设永远不会返回false。

    **name**：返回令牌的名字，例如“MyToken”。\
    可选——这个方法可以用来提高可用性，但接口和其它合约不能依赖该值的存在。

    function name() view returns (string name)

    **symbol**：返回令牌的符号，例如“HIX”。\
    可选——这个方法可以用来提高可用性，但接口和其它合约不能依赖该值的存在。

    function symbol() view returns (string symbol)

    **decimals**：返回token使用的小数位数，比如 8，表示将记录在链上的token数除以100000000后显示给用户。\
    可选——这个方法可以用来提高可用性，但接口和其它合约不能依赖该值的存在。

    function decimals() view returns (uint8 decimals)

    **totalSupply**：返回token的总供应量。

    function totalSupply() view returns (uint256 totalSupply)

    **balanceOf**：返回另一个账户地址\_owner的账户余额。

    function balanceOf(address \_owner) view returns (uint256 balance)

    **transfer**：转移\_value数量的token到地址\_to，并且必须触发Transfer事件。 如果\_from帐户余额没有足够的token来支出，该方法应该throw。\
    **注意**：\_value=0必须被视为正常转账并触发Transfer事件。

    function transfer(address \_to, uint256 \_value) returns (bool success)

    **transferFrom**：从地址\_from发送\_value个token到地址\_to，必须触发Transfer事件。\
    transferFrom方法用于提现流程，允许合约为你转移token。这可以用于允许合约为你转让代币或收取费用。除非帐户\_from有意通过某种机制授权消息的发送者，否则该方法应该throw。

    **注意**：\_value=0必须被视为正常转账并触发Transfer事件。

    function transferFrom(address \_from, address \_to, uint256 \_value) returns (bool success)

    **approve**：允许\_spender多次从你的帐户提现，最高数量是\_value。 如果再次调用此函数，它将以\_value覆盖当前的值。

    注意：为了防止向量攻击，客户端需要确认以这样的方式创建用户接口，即在为同一个花费者设置另一个值之前，先将它的值设置为0。虽然合约本身不应该强制执行，以前部署的合同允许向后兼容。

    function approve(address \_spender, uint256 \_value) returns (bool success)

    **allowance**：返回被允许从\_owner提取到\_spender余额。

    function allowance(address \_owner, address \_spender) view returns (uint256 remaining)



    **2）event**

    **Transfer**：当token被转移(即使是0值)时必须被触发。

    event Transfer(address indexed \_from, address indexed \_to, uint256 \_value)

    **Approval**：当成功调用approve(address \_spender, uint256 \_value)后必须被触发。

    event Approval(address indexed \_owner, address indexed \_spender, uint256 \_value)


5.  **实施**

    在以太坊网络上已经部署了大量符合ERC20标准的代币。从节省gas到提高安全性，不同权衡的团队已经编写了各种不同的合约方案。

    合约实例：

    [StandardToken](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2FOpenZeppelin%2Fzeppelin-solidity%2Fblob%2Fmaster%2Fcontracts%2Ftoken%2FERC20%2FStandardToken.sol)

    [EIP20](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2FConsenSys%2FTokens%2Fblob%2Fmaster%2Fcontracts%2Feip20%2FEIP20.sol)

    \
    在调用approve 之前强制设为0的实例：

    [MiniMeToken](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2FGiveth%2Fminime%2Fblob%2Fmaster%2Fcontracts%2FMiniMeToken.sol)

    ****
6.  **历史**

    本标准的相关历史链接：

    * [Vitalik Buterin的最初提议](https://link.jianshu.com/?t=%255Bhttps%3A%2F%2Fgithub.com%2Fethereum%2Fwiki%2Fwiki%2FStandardized_Contract_APIs%2F499c882f3ec123537fc2fccd57eaa29e6032fe4a%255D%28https%3A%2F%2Fgithub.com%2Fethereum%2Fwiki%2Fwiki%2FStandardized_Contract_APIs%2F499c882f3ec123537fc2fccd57eaa29e6032fe4a%29)
    * [Reddit讨论](https://link.jianshu.com/?t=%255Bhttps%3A%2F%2Fwww.reddit.com%2Fr%2Fethereum%2Fcomments%2F3n8fkn%2Flets_talk_about_the_coin_standard%2F%255D%28https%3A%2F%2Fwww.reddit.com%2Fr%2Fethereum%2Fcomments%2F3n8fkn%2Flets_talk_about_the_coin_standard%2F%29)
    * [原Issue #20](https://link.jianshu.com/?t=%255Bhttps%3A%2F%2Fgithub.com%2Fethereum%2FEIPs%2Fissues%2F20%255D%28https%3A%2F%2Fgithub.com%2Fethereum%2FEIPs%2Fissues%2F20%29)
7.  **版权**

    版权和相关权利通过[CC0](https://link.jianshu.com/?t=https%3A%2F%2Fcreativecommons.org%2Fpublicdomain%2Fzero%2F1.0%2F)许可协议放弃。

## ICO爆发：ERC-20通证标准的应用

ICO是Initial Coin Offering的简称，顾名思义，就是项目方以初始产生或者首批公开发行的通证（代币）作为回报的一种众筹资金的方式。

ICO在很大程度上借鉴了证券业的Initial Public Offering（首次公开发行，IPO）。之所以没有沿用IPO而改为ICO，主因是传统的IPO（首次公开募股，initial public offerings）的门槛非常高而且涉及到跨国就更是难上加难。区块链业界删繁就简，采用了新的概念及新的模式，在突破障碍的同时规避了法律风险。

用最简单的话说：ICO就是项目方发行一种自己的通证，投资者用ETH或其它通证来兑换该通证。

有个别通证+收益分红模式（不同于股权模式）的区块链项目的众筹，采用过ITO（Initial Token Offering）这一名称，但因为我们知道只有符合ERC-20的通证才用于众筹，大众又更熟悉Coin，所以现在约定俗成，都采用ICO（Initial Coin Offering）。

典型的ICO流程与特点为：

1. 核心团队通过专业会议等各种渠道发布ICO消息。
2. 核心团队发布ICO规则，包括ICO的时间安排和融资额的安排。譬如最低融资额，如果达不到该额度，该ICO即告失败，所有投资都会被退回。有的项目同时会设定最高投资额（硬顶），达到该投资额，则ICO提前结束。
3. ICO开启后，现在越来越多的项目只接受ETH，因为通过以太坊的智能合约，可以让ICO过程实现零人工、零差错。个别项目还像以前那样接受以太币、比特币等多种数字加密货币。
4. ICO结束后，代币就会开始上交易所。

2015年11月创建、2017年9月11日定稿的ERC20通证标准，2017年4月提前爆发于ICO。这里面有多重原因：

* 真正抗审查
* 真正实现了全球范围的投融资
* 极低的投融资成本
* 极高的投融资效率（超快的转账速度、超快的融资速度）
* 第一次实现了平等的投资权利
* 真实的市场估值
* 最高的流动性溢价
* 零差错、零人工

以下为各种投融资手段的对比：

|       | ICO  | IPO | 众筹 | VC风投 |
| ----- | ---- | --- | -- | ---- |
| 投融资门槛 | 非常低  | 极高  | 低  | 高    |
| 透明程度  | 高    | 高   | 高  | 低    |
| 资金退出  | 非常容易 | 容易  | 难  | 难    |
| 流动性   | 非常好  | 好   | 差  | 差    |
| 监 管   | 目前很低 | 极高  | 低  | 低    |

前面我们提过，以太坊早在2014年就通过ICO筹集了发展基金，为什么是2017年ICO才大爆发？

这里面有区块链知识传递和区块链项目发展的原因，还有一个很重要的原因就在于ETH和ERC20通证标准。2017年前ICO的主要币种为比特币，但比特币到账时间长达1小时，ETH一次确认只需要9秒，到帐通常不足2分钟。而且比特币网络堵塞，有很多转账无法完成；加上比特币2017年没有智能合约可用，通常还需要用户到官网注册，ERC20通证标准的出现，才实现了ICO的质变！

ICO的爆发，归功于ERC20通证标准，也是智能合约的第一个爆点！

数据表明：ICO的爆发已经在区块链领域拔除风险投资VC在投融资领域的霸主地位！因此，它本身已经是一场金融革命的第一场胜利之战。
