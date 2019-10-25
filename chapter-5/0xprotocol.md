# 5.4 去中心化交易协议0x Protocol

0x Protocol（常常被简称为0x）是一个去中心化交易协议。其off-chain order转发和on-chain settlement的逻辑图如下：

![&#x56FE;&#x7247;&#x5305;&#x542B; &#x6587;&#x5B57;

&#x81EA;&#x52A8;&#x751F;&#x6210;&#x7684;&#x8BF4;&#x660E;](../.gitbook/assets/0%20%2811%29.png)

图表 5. 0x protocol 整体逻辑图

上图中灰色的矩形和圆形分别代表以太坊上的智能合约和账户。具体的逻辑：

* maker同意DEX\(去中心化交易所合约\)获取他们关于Token A的信息。
* Maker创建一笔关于Token A兑换为Token B的订单，并且声明了期望的交易rate和过期时间。将以上订单签名。
* Maker 通过网络传输层将此订单广播
* Taker接收到这笔订单，并且决定来完成此项订单。
* Taker同意DEX来获取他的关于Token B的信息。
* Taker提交maker的那条以签名的order到DEX合约。
* DEX合约认证maker的签名，验证通过后，然后将两者的Token按照拟定的rate进行交换。

其中，关于上述的操作3和操作4 off-chain的行为，0x protocol的网络传输层和会话层以及应用层来负责Order的relay，展示，以及推送。当然，用户也可以将签好名的order通过email，twitter等方式广播出去，Taker一旦获取之后，将order发送至DEX合约即可完成交易。

0x的点对点订单允许双方使用他们更喜欢relayer直接互相交换代币。 构成订单的数据包是可以通过电子邮件，Facebook消息，耳语或任何类似服务发送的十进制数百字节。 订单只能由指定的taker地址填充，使得该订单对窃听者或外部用户无效。

特色：

* ![](../.gitbook/assets/1%20%284%29.png)快速为自有社区的 Token 打造一个交易市场；
* 在自有体系中快速生成一个交易数字产品和资产的交易市场；非常有利于钱包的拓展。
* 可交易任何 ERC-20 或 ERC-721 数字资产。
* 和雷电网络一样，它是链下订单中继，链上最终结算。兼顾交易的速度及安全性。
* 0x的Relayer带来的18种业务（链接请扫二维码）

问题：

* Relayer可以理解是任何实现了0x协议和提供了链下订单簿服务的做市商、交易所、Dapp（Ethfinex，Augur，District0x和Melonport）等等。0x 协议构建的生态中可能出现中继角色发展趋中心化。
* 未提供去中心化的价格发现机制。

