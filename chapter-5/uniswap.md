# 5.6 通用的去中心化交易协议Uniswap

也许是受了Bancor Network的启发，亦或是英雄所见略同，Uniswap也采用智能合约存储和供应代币，算法根据市场需求定价的组合模式。但无论是智能合约，还是算法，两者都有蛮大差别。Uniswap 使用的算法是V神提出的被称为“恒定乘积做市商模型”（Constant Product Market Maker）的算法，我们从后面的图表7中的K值，很直观地了解到这一算法的特点。

另外值得一提的是，Uniswap的智能合约是用新的语言Vyper开发的，其执行效率要明显优于智能合约开发语言Solidity。

**交易者须知**

举个 ETH / DAI 交易对的简单例子。假设做市商已经为这一流动性池注入了 100,000 DAI 和 1,000 ETH 的资金。Uniswap 将这两个数量相乘（100,000 x 1,000 = 100,000,000）。

| **Initial Conditions** | **DAI Liquidity** | **ETH Liquidity** | **Product** |
| :--- | :--- | :--- | :--- |
|  | 100,000 | 1,000 | 100,000,000 |
|  | x | y | k |

Uniswap 针对这一特定交易对的目标是：无论交易活动多少，该产品都将始终保持 1 亿的交易对乘积数量（因此称为“恒定乘积做市商”）。要记住 x \* y = k 这一关键公式，其中 x 和 y 是流动性池中的代币数量，k 是乘积。要想保持 k 恒定，x 和 y 只能相互反向变动。比如某一交易者在此合约中用 DAI 购买 ETH，则他们正在增加 x（因为增加了流动池中的 DAI），同时也减少了 y（因为减少了流动池中的 ETH）。但这个反向变化不是线性增长的关系。如果现在你要购买 100ETH 而不是 10ETH，那么在购买 10ETH 所需的 DAI 的基础上增加 10 倍，可能并不够用。事实上所需的 DAI 是渐进式增加的。最简单的理解方法是绘制 x \* y = k 曲线。

![&#x56FE;&#x7247;&#x5305;&#x542B; &#x5BA4;&#x5185;

&#x81EA;&#x52A8;&#x751F;&#x6210;&#x7684;&#x8BF4;&#x660E;](../.gitbook/assets/0%20%286%29.png)

图表 6. 代币A越大的买单，获得代币B的兑换率约低

在这个系统中需要注意的是关键点在于：报价直接取决于订单的大小。越往曲线右端移动，单位投入获得的收益越少。假设当前的 ETH / DAI 价格为100，不同数量订单所需支付的溢价见下表：

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#x8D2D;&#x5F97;ETH</b>
      </th>
      <th style="text-align:left"><b>DAI&#x652F;&#x51FA;</b>
      </th>
      <th style="text-align:left"><b>ETH<br />&#x5355;&#x4F4D;&#x6210;&#x672C;</b>
      </th>
      <th style="text-align:left"><b>&#x6EA2;&#x4EF7;</b>
      </th>
      <th style="text-align:left"><b>DAI&#x6D41;&#x52A8;&#x6027;&#x66F4;&#x65B0;</b>
      </th>
      <th style="text-align:left">
        <p><b>ETH&#x6D41;&#x52A8;&#x6027;</b>
        </p>
        <p><b>&#x66F4;&#x65B0;</b>
        </p>
      </th>
      <th style="text-align:left"><b>K&#x503C;</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left">x</td>
      <td style="text-align:left">y</td>
      <td style="text-align:left">k</td>
    </tr>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">100.10</td>
      <td style="text-align:left">100.10</td>
      <td style="text-align:left">0.10%</td>
      <td style="text-align:left">100100.10</td>
      <td style="text-align:left">999</td>
      <td style="text-align:left">1&#x4EBF;</td>
    </tr>
    <tr>
      <td style="text-align:left">10</td>
      <td style="text-align:left">1,010.10</td>
      <td style="text-align:left">101.01</td>
      <td style="text-align:left">1.01%</td>
      <td style="text-align:left">101010.10</td>
      <td style="text-align:left">990</td>
      <td style="text-align:left">1&#x4EBF;</td>
    </tr>
    <tr>
      <td style="text-align:left">50</td>
      <td style="text-align:left">5,263.16</td>
      <td style="text-align:left">105.26</td>
      <td style="text-align:left">5.26%</td>
      <td style="text-align:left">105263.16</td>
      <td style="text-align:left">950</td>
      <td style="text-align:left">1&#x4EBF;</td>
    </tr>
    <tr>
      <td style="text-align:left">100</td>
      <td style="text-align:left">11,111.11</td>
      <td style="text-align:left">111.11</td>
      <td style="text-align:left">11.11%</td>
      <td style="text-align:left">111111.11</td>
      <td style="text-align:left">900</td>
      <td style="text-align:left">1&#x4EBF;</td>
    </tr>
    <tr>
      <td style="text-align:left">200</td>
      <td style="text-align:left">25,000.00</td>
      <td style="text-align:left">125.00</td>
      <td style="text-align:left">25.00%</td>
      <td style="text-align:left">125000.00</td>
      <td style="text-align:left">800</td>
      <td style="text-align:left">1&#x4EBF;</td>
    </tr>
    <tr>
      <td style="text-align:left">500</td>
      <td style="text-align:left">100,000.00</td>
      <td style="text-align:left">200.00</td>
      <td style="text-align:left">100.00%</td>
      <td style="text-align:left">200000.00</td>
      <td style="text-align:left">500</td>
      <td style="text-align:left">1&#x4EBF;</td>
    </tr>
    <tr>
      <td style="text-align:left">800</td>
      <td style="text-align:left">400,000.00</td>
      <td style="text-align:left">500.00</td>
      <td style="text-align:left">400.00%</td>
      <td style="text-align:left">500000.00</td>
      <td style="text-align:left">200</td>
      <td style="text-align:left">1&#x4EBF;</td>
    </tr>
    <tr>
      <td style="text-align:left">999</td>
      <td style="text-align:left">99,900,000.00</td>
      <td style="text-align:left">100,000.00</td>
      <td style="text-align:left">99900.00%</td>
      <td style="text-align:left">100,000,000.00</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">1&#x4EBF;</td>
    </tr>
    <tr>
      <td style="text-align:left">1000</td>
      <td style="text-align:left">Infinity</td>
      <td style="text-align:left">Infinity</td>
      <td style="text-align:left">Infinity</td>
      <td style="text-align:left">Infinity</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">1&#x4EBF;</td>
    </tr>
  </tbody>
</table>图表 7. Uniswap兑换带来的数据变化

如表所示，当购买 ETH 的数量超过流动性池的 2％ 时，购买大量的 ETH 就会很昂贵。不过要记住，这些溢价乃是由当前流动性池的规模决定的。如果流动性池大了 100 倍（即 1000 万 DAI 和 10 万ETH），这时候只购买 50ETH 就没那么贵了。最终，支付价格反映的是交易规模对 x/y 比率的改变程度。流动性池越大，处理大订单就越容易（这正是我们所期望的）。

值得注意的一点是是抢先交易（front running），当今以太坊上的所有 DEX 都存在这个问题。为缓解这一问题，Uniswap 允许用户在下单时指定最高价格。这样，即便矿工抢先交易某一订单，用户也不会被迫接受更高的价格。虽然用户可能会错过这笔交易，但他们不需要支付更高价格。Uniswap 的另一个特点是“订单到期”，该功能可以防止矿工搁置已签名的交易并等到价格变动后再处理。

ERC-20 到 ERC-20 的代币互换无需使用专门的流动性池。例如，在处理 REP &lt;&gt; ZRX 订单时，系统将先通过 REP/ETH 交易对，再自动通过 ZRX/ETH 交易对。

**流动性提供者须知**

流动性提供者面临的情况要更复杂。举个 ETH/DAI 市场的例子。首先，流动性提供者（以及交易者）要注意，x/y 比率代表交易对的价格。在我们当前使用例子中，x/y = 100,000 DAI / 1,000 ETH = 100。假设 ETH 在 Coinbase 上价格是 100 美元。如果 x/y 不等于 100，那么Uniswap和其它交易所譬如Coinbase之间将存在套利机会。

当流动性提供者向池中增加流动性时，他不能只向交易对的一方提供流动性，这样会改变两种代币间的比率、设定出一个新交易价格（这是个危险操作，因为他将立即被交易对手套利，从而赔钱）。例如，某一流动性提供者仅增加了 1,000ETH，则合约的新比率为 100,000 / 2,000 = 50。套利者将蜂拥而至，直至该比率再次变成100：1。 流动性提供者必须为交易对的两个币种提供等值的资金（Uniswap 接口将确保不会出现错误操作）。

假设在增加 10,000DAI和 100ETH（总市值 20,000 美元）之后，流动性池现在总共有 100,000 DAI 和 1,000ETH。由于注入的金额相当于总流动性池的 10％，因此合约会产出一种“流动性代币（liquidity token）”，将其提供给做市商，使其有权获得池中可用流动性的 10％。这些代币不是用于交易的投机币，而只是一种用于记录流动性提供方债权的会计或者说记账工具。如果其他人随后存币或者取币，则将会产生或者销毁新的流动性代币，保证每个人在流动资金池中的相对份额保持不变。

现在假设 Coinbase 上 ETH 的价格从 100 美元涨到 150 美元。在经过一番套利之后，Uniswap合约也将反映出这一价格变化。交易者将增加 DAI，减少 ETH，直到新比率变成 150 : 1。这会对流动性提供者造成什么影响呢？合约反映出的数字会接近 122,400DAI 和 817ETH（做个算数检查一下这些数字的是否准确：122,400 \* 817 = 100,000,000（恒定乘积），122,400 / 817 = 150（新价格））。如果把我们有权获得的 10％ 提取出来，那么现在会变成 12,240DAI 和 81.7ETH。此时的总市值为 24,500 美元。做市导致错失了大约 500 美元的利润。

显然，没有人愿意抱着做慈善的心态提供流动性，收益又不能从炒币中获得（因为也没得炒）。所以，总交易量的 0.3％ 会按比例分配给所有流动性提供者。默认情况下，这些费用会重新注入流动性池，但可随时收取。在不知道中间交易量的情况下，很难说交易费的收入和做市的损失孰高孰低。对流动性提供者来说，显然行情波动越多越好。

**相比于班科，Uniswap它通过以下改进发生了质的飞跃：**

1. 去中心化并且抗审查的上币模式。

班科需要项目方填写上币申请表，质押代币以及等额的BNT，而且已经发生BNT可以被随意冻结。这是一种带审查性质的上币机制和管理模式。Uniswap将自己的模型称为“自动做市商”（AMM），其本质是去审查和去中心化结合的上币模式。Uniswap不审核任何上币者，而是以上币者能够获得一定的收益为奖励机制，吸引大家上币。

1. 彻底的去中心化。

2018年7月9日，因安全漏洞班科受到黑客攻击，当时一个钱包正在升级某些智能合约时发现受到攻击，24,984个以太坊（约合1200万美元），以及30万Pundi X（价值约100万美元）和价值约1000万美元的BNT被窃。他们通过Bancor协议内置机制冻结了被盗的BNT，这种机制就是为了防止在安全漏洞爆发时，让班科有效恢复系统，同时阻止非法黑客转移窃取的加密代币。它的确有效阻止了黑客将BNT卷走（ETH以及其他被盗币种已流失在外）。班科还暂时关闭了业务运营。这次 Bancor 平台被盗事件与 BancorConverter 合约有关，攻击者（黑客/内鬼）极有可能获取了 0x009bb5e9fcf28e5e601b7d0e9e821da6365d0a9c 账户的私钥。而此账户正是某个转换代币合约 BancorConverter的 owner，同样拥有极高权限。owner 作为该合约的所有者和管理员，有唯一的权限通过 withdrawTokens\(\) 方法提走合约中的全部 ERC20 Token 至任意地址。owner 对 SmartToken 合约具有以下权限：

* owner 可通过 disableTransfers\(\) 任意禁用转账功能。
* owner 可通过 issue\(\) 任意增发代币。
* owner 可通过 destroy\(\) 任意销毁代币。

以上这些功能均通过 ownerOnly 进行限定，换句话说，owner 对 Bancor 合约拥有最高权限。譬如，至少理论上Bancor可以在缺乏监管的现状下，在任意地址随意铸造和燃烧存储通证。

如果一个交易所会丢失用户资金或者能冻结用户资金，则它不是一个真正意义的去中心化交易所。

1. 未发行自己的代币，规避掉产生私利的可能。而且直接使用ETH作为算法所需的标配币种，大大简化了智能合约代码。相比之下，班科发行了自己的代币BNT，上币需要以BNT作为标配币种，因此使得其智能合约的代码复杂度大大提高，在交易拥堵的时候，合约执行的失败率非常高，手续费也非常高。还有，相比ETH，BNT市值小、应用单一，价格容易被大庄联合操控。
2. 更低的gas基准。Uniswap的gas基准低于班科的十分之一。

| **Exchange** | **Uniswap** | **Bancor** | **0x** |
| :--- | :--- | :--- | :--- |
| ETH to ERC20 | 46,000 | [440,000](https://etherscan.io/tx/0x462a3ad9dd05ce18cb33412fde02ee8cfa782d69a9df85be97ac8216a5c8b422) | [113,000](https://etherscan.io/tx/0xe2ca9f47926e2b262cf9b060f735c5ebfda1a4edd55af236d95b274c75be5449)\* |
| ERC20 to ETH | 60,000 | [403,000](https://etherscan.io/tx/0x550961cbe81995c2300550b75654c75fb112fa5ba4a20ab7af1c02ed218af8e1) | [113,000](https://etherscan.io/tx/0xe2ca9f47926e2b262cf9b060f735c5ebfda1a4edd55af236d95b274c75be5449)\* |
| ERC20 to ERC20 | 88,000 | [538,000](https://etherscan.io/tx/0x4f0595d122a2202022960d8c89773de206f4e4ab4da26336b68f1993691334b2) | [113,000](https://etherscan.io/tx/0xe2ca9f47926e2b262cf9b060f735c5ebfda1a4edd55af236d95b274c75be5449) |

\*wrapped ETH

1. 班科需要项目方质押2％～５％的代币以及等值的BNT，这对于项目方来说，相当于一笔必不可少的成本。
2. 当今以太坊上的所有 DEX 都存在抢先交易（front running）风险。抢先交易在区块链领域是指矿工在执行普通用户买卖前，先替自己的账户买卖的操作，矿工这样做通常是因为他认为普通用户的买卖将改变市场价格，因此抢先买卖以图利。抢先交易是高频交易中的主要策略。为缓解这一问题，Uniswap 允许用户在下单时指定最高价格。这样，即便矿工抢先交易某一订单，用户也不会被迫接受更高的价格。虽然用户可能会错过这笔交易，但他们不需要支付更高价格。
3. Uniswap 的“订单到期”能可以防止矿工搁置已签名的交易并等到价格变动后再处理。

Uniswap有待观察与解决的关键问题：

1. Uniswap试图用去中心化且抗审查的模式，辅助激励机制（总交易量的 0.3％ 会按比例分配给所有流动性提供者），让任何人都可以自由上币。严重的问题是，它本身没有ETH的全局定价或者ETH与法币的定价机制，它无法保证上币者真的能赚钱。以图表６（Uniswap兑换带来的数据变化）来看，假如其它用户用99,900,000个DAI（理解为其它某个代币更好）兑换掉999个ETH，资金池里只剩1个ETH和已经在大众眼里归零的DAI，这1个ETH并没有因为有人抛售了巨多的DAI，而身价暴涨——目前ETH的价格甚至取决于中心化交易所。结果是：资金池的提供者几乎血本无归。
2. 目前存在各种类型的交易所，Uniswap和其它交易所之间将存在套利机会。投机者很容易找到方法，通过其它交易所和虚假新闻等方式的配合套利。资金池的提供者承受的风险非常大。
3. 如果资金池的提供者投机，那么情况也许更可怕：通过其它交易所和虚假新闻等方式的配合，引发市场挤兑，假如其它用户用99,900,000个DAI（理解为其它某个代币更好）兑换掉999个ETH，然后投机分子开始反向操控，那么他可以用999个ETH获得99,900,000个DAI后，通过其它交易所，低成本拉高DAI的价格，从而获得巨额回报！

