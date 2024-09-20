# EIP-7702：设置EOA账户代码 [](https://ethereum-magicians.org/t/eip-set-eoa-account-code-for-one-transaction/19923) [](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7702.md)

### 添加一种新的交易类型，在执行过程中为EOA设置代码

| 作者   | Vitalik Buterin ([@vbuterin](https://github.com/vbuterin)), Sam Wilson ([@SamWilsn](https://github.com/SamWilsn)), Ansgar Dietrichs ([@adietrichs](https://github.com/adietrichs)), Matt Garnett ([@lightclient](https://github.com/lightclient))                                                                      |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 创建日期 | 2024-05-07                                                                                                                                                                                                                                                                                                             |
| 所需   | [EIP-2](https://eips.ethereum.org/EIPS/eip-2), [EIP-2718](https://eips.ethereum.org/EIPS/eip-2718), [EIP-2929](https://eips.ethereum.org/EIPS/eip-2929), [EIP-2930](https://eips.ethereum.org/EIPS/eip-2930), [EIP-3541](https://eips.ethereum.org/EIPS/eip-3541), [EIP-3607](https://eips.ethereum.org/EIPS/eip-3607) |

### [](https://eips.ethereum.org/EIPS/eip-7702#abstract) 摘要  

新增一种交易类型，添加一系列 [chain_id, address, nonce, y_parity, r, s] 授权元组。对于每个元组，向签名账户的代码写入一个委托设计器 (0xef0100 ++ address)。所有读取代码的操作必须加载设计器所指向的代码。
## [](https://eips.ethereum.org/EIPS/eip-7702#motivation) 动机
人们对为外部账户（EOAs）增加短期功能改进非常感兴趣，这可以提高应用的可用性，并在某些情况下提升安全性。特别的三个应用包括：

**批处理**：允许同一用户在一个原子交易中进行多个操作。一个常见的例子是 ERC-20 授权后跟随的支出，这在去中心化交易所（DEX）中是一个常见的工作流程，目前需要两个交易。批处理的高级用例偶尔涉及依赖关系：第一个操作的输出是第二个操作的输入的一部分。  
**赞助**：账户 X 代表账户 Y 支付一个交易。账户 X 可以用其他 ERC-20 代币获得这项服务的报酬，或者可以是一个应用程序运营商为其用户免费处理交易。  
**权限降级**：用户可以签署子密钥，并给予它们特定的权限，这些权限远低于对账户的全局访问权限。例如，可以想象一种权限，允许支出 ERC-20 代币但不允许支出 ETH，或者每天支出总余额的 1%，或者仅与特定应用程序进行交互。

## [](https://eips.ethereum.org/EIPS/eip-7702#specification) 技术规范

### [](https://eips.ethereum.org/EIPS/eip-7702#parameters) 参数

| 参数                       | 值       |
| ------------------------ | ------- |
| `SET_CODE_TX_TYPE`       | `0x04`  |
| `MAGIC`                  | `0x05`  |
| `PER_AUTH_BASE_COST`     | `2500`  |
| `PER_EMPTY_ACCOUNT_COST` | `25000` |

### [](https://eips.ethereum.org/EIPS/eip-7702#set-code-transaction) 设置代码交易

我们引入了一种新的EIP-2718交易，即“设置代码交易”，其中 `TransactionType`（交易类型）为`SET_CODE_TX_TYPE`，并且 `TransactionPayload` （交易装载）是以下内容的RLP序列化：

```
rlp([chain_id, nonce, max_priority_fee_per_gas, max_fee_per_gas, gas_limit, destination, value, data, access_list, authorization_list, signature_y_parity, signature_r, signature_s])

authorization_list = [[chain_id, address, nonce, y_parity, r, s], ...]
```

外部交易的字段 `chain_id`、`nonce`、`max_priority_fee_per_gas`、`max_fee_per_gas`、`gas_limit`、`destination`、`value`、`data` 和 `access_list` 遵循 [EIP-4844](https://eips.ethereum.org/EIPS/eip-4844) 的相同语义。请注意，这意味着空的（null） destination 是无效的。

`authorization_list` 是一个元组列表，存储（即提供）签名者希望在其 EOA 的应用场景中执行的地址与代码。如果 `authorization_list` 的长度为零，交易将被视为无效。

当授权元组（authorization tuple）中的任何字段无法在以下范围内时，该交易也被视为无效：

```
assert auth.chain_id < 2**256
assert auth.nonce < 2**64
assert len(auth.address) == 20
assert auth.y_parity < 2**256
assert auth.r < 2**256
assert auth.s < 2**256
```

此交易的 [EIP-2718](https://eips.ethereum.org/EIPS/eip-2718) `ReceiptPayload`为 `rlp([status, cumulative_transaction_gas_used, logs_bloom, logs])`（分别对应：状态, 累计交易gas值, 日志_布隆, 日志）。
#### [](https://eips.ethereum.org/EIPS/eip-7702#behavior) 行为

在执行交易开始时，在增加发送者的nonce后，对于每个[chain_id, address, nonce, y_parity, r, s]元组，执行以下操作：  

1. 验证链ID是0或当前链的ID。
2. `authority = ecrecover(keccak(MAGIC || rlp([chain_id, address, nonce])), y_parity, r, s]`
3. 将`authority`添加到`accessed_addresses`（如[EIP-2929](https://eips.ethereum.org/EIPS/eip-2929)中定义）。  
4. 验证`authority`的代码要么为空，要么已经被委托。
5. 验证`authority`的 nonce 等于 `nonce`。
6. 如果`authority`存在于trie（前缀树）中，则将`PER_EMPTY_ACCOUNT_COST - PER_AUTH_BASE_COST`的gas添加到全局退款计数器中。
7. 将`authority`的代码设置为0xef0100 || address。这是一个委托标识。
8. 将`authority`的nonce增加1。  

如果上述任何步骤失败，立即停止处理该元组，并继续处理列表中的下一个元组。在多个元组对应同一`authority`的情况下，将使用最后一个有效出现的地址设置代码。  

请注意，授权元组的签名者可能与交易的`tx.origin`不同。
##### [](https://eips.ethereum.org/EIPS/eip-7702#delegation-designation) 代理（授权）指示

代理指示使用了[EIP-3541](https://eips.ethereum.org/EIPS/eip-3541)中禁止的操作码`0xef`，以指定代码具有特殊用途。这个指示要求所有检索代码操作都遵循地址指针，以填充可观察的账户代码。受影响的指令包括：`EXTCODESIZE`、`EXTCODECOPY`、`EXTCODEHASH`、`CALL`、`CALLCODE`、`STATICCALL`和`DELEGATECALL`。

例如，`EXTCODESIZE`将返回由 `address`（地址）指向的代码的大小，而不是代表代理指示的 `23`。`CALL` 将类似地从 `address`加载代码，并在 `authority`（授权）的应用场景中执行它。

如果一个代理指示者指向另一个指示者，形成潜在的指示者链或循环，客户端必须只检索第一个代码，然后停止跟随指示者链。
#### [](https://eips.ethereum.org/EIPS/eip-7702#gas-costs) gas费用
 
新交易的内在成本继承自EIP-2930，具体为`21000 + 16 * 非零calldata字节 + 4 * 零calldata字节 + 1900 * 访问列表存储键计数 + 2400 * 访问列表地址计数`。此外，我们还增加了一个费用，费用为`PER_EMPTY_ACCOUNT_COST * 授权列表长度`。

交易发送者将为所有授权元组支付费用，无论其有效性或重复性。

如果代码读取指令在解析代表授权代码时访问一个冷账户，则在正常费用上增加2600 gas的EIP-2929 `COLD_ACCOUNT_READ_COST`费用，并将账户添加到`accessed_addresses`中。否则，评估`100`的`WARM_STORAGE_READ_COST`费用。
#### [](https://eips.ethereum.org/EIPS/eip-7702#transaction-origination) 交易来源

修改 [EIP-3607](https://eips.ethereum.org/EIPS/eip-3607) 所施加的限制，以允许其代码是有效的代表授权设计（即`0xef0100 || address`）的外部拥有账户（EOAs）继续发起交易。任何其他代码值的账户均不得发起交易。
## [](https://eips.ethereum.org/EIPS/eip-7702#rationale) 理由

### [](https://eips.ethereum.org/EIPS/eip-7702#no-initcode) 无初始化代码
 （注：Initdode是创建"存储在链上的字节码"的代码。 即通常指的是使用create2操作码时需要的字节码。此处翻译为“初始化代码”）
 许多原因使得执行初始化代码并不理想。主要的担忧是它不自然。初始化代码的目的是初始化和部署合约。根据这个EIP，它将承担一个新的角色，以决定是否适合将代码部署到外部账户（EOA）。假设用户只希望在交易的普通调用数据中有相关操作时，代码被部署到他们的账户。这赋予了EOA独特的权力，控制代码在他们账户中何时以及执行什么。虽然EIP-7702如其所写仍在某种程度上允许这样做，但决策缺乏可编程性将迫使钱包不签署许多授权元组，而是专注于只签署指向可配置代理的元组。这使得EOA体验类似于智能合约钱包 。

 此外，交易中的初始化代码倾向于在交易内部传播。这意味着它需要包含在授权元组中并进行签名。最小的初始化代码大约为15字节，仅仅是将合约代码从外部地址复制过来。总成本大约为 `16 * 15 = 240` 的调用数据成本，加上 [EIP-3860](https://eips.ethereum.org/EIPS/eip-3860) 的成本 `2 * 15 = 30`，再加上大约150的运行时成本。因此，光是准备账户就将花费近500额外的 gas；如果没有从外部账户复制，可能成本更高，达到1200+的 gas。 
### [](https://eips.ethereum.org/EIPS/eip-7702#creation-by-template) 通过模板创建 

 不论是否有初始化代码（Initcode），用户如何指定他们打算在账户中运行的代码也是一个问题。两个主要选项是直接在交易中指定字节码或指定指向代码的指针。最简单的指针就是链上某段代码的地址。 

 成本分析使答案变得明晰。最小的代理大约为50字节，而地址为20字节。30字节的差距并未提供任何有用的附加功能，并且将在链上低效地复制数十亿次。 

 此外，直接指定代码的方式将再次使得EOA能够拥有新的独特能力，在交易调用数据中执行任意指定的代码。
### [](https://eips.ethereum.org/EIPS/eip-7702#lack-of-instruction-prohibition) 缺乏指令禁止
 
从实现的角度和用户理解的角度来看，一致性都是以太坊虚拟机（EVM）的一个重要属性。虽然在外部账户（EOA）的应用场景中考虑过对几类指令的禁令，但作者认为没有足够的理由去这样做。这将迫使智能合约钱包和EOA智能合约钱包走上不同的合约开发道路。

可以建立禁令的主要指令家族是与存储相关的指令和与合约创建相关的指令。我们决定不禁止存储指令主要基于其对智能合约钱包的重要性。尽管可以有一个外部存储合约供智能合约钱包调用，但这效率不高。在未来，新的状态方案甚至可能允许以更低的成本访问某些存储槽。智能合约钱包非常希望利用这一点，而存储合约则无法支持。

创建指令在其他类似EIP的情况下曾被考虑禁止，但由于这个EIP允许EOA在交易内花费价值，因此在交易内增加nonce并使待处理交易失效的担忧并不大。一个有趣的附带效果是，通过结合EIP-7702和CREATE2，可以在不承诺任何费用市场参数的情况下，承诺将特定的字节码部署到一个地址。这解决了长期以来的跨链合约部署的普遍问题。

创建指令在其他类似的以太坊改进提案（EIP）中曾被考虑禁用，然而由于该EIP允许EOA在交易内花费价值，因此在交易内递增 nonce 并使得待处理交易失效的问题并不显著。一个有趣的附带效果是，通过结合 EIP-7702 和 CREATE2，可以在不提交任何费用市场参数的情况下，保证在特定地址部署特定的字节码。这解决了长期以来跨链合约部署的普遍问题。
### [](https://eips.ethereum.org/EIPS/eip-7702#signature-structure) 签名结构  
这个EIP中的签名方案支持灵活的设计模式，可以实现地址 `address` 的全权委托和更受保护的地址 `address`委托。
#### [](https://eips.ethereum.org/EIPS/eip-7702#code-pointer) 代码指针  

在签署代码指针时需要考虑的一个因素是该地址在另一条链上可能指向什么代码。在某些情况下，验证部署是否是确定性的可能并不是很必要。在这种情况下，可以设置链 ID 以减少授权的范围。对于其他更倾向于普遍部署的情况，例如，委托给钱包代理。在这些情况下，可以将链 ID 设置为 0，以便在所有 EIP-7702 链上都具有效力。钱包维护者将能够将一个单一的 EIP-7702 授权消息硬编码到他们的钱包中，这样跨链代码可塑性就不会成为问题。

添加链 ID 的另一种选择是对地址指向的代码进行签名。这似乎在保留账户中实际运行的代码的特定性同时，最大限度地减少了链上认证元组的大小。然而，这种格式的一大缺点是它要求进行数据库查找，以确定每个认证元组的签名者。这种要求本身似乎在交易传播中产生了足够的复杂性，因此决定避免这种情况，直接对地址进行签名。
#### [](https://eips.ethereum.org/EIPS/eip-7702#in-protocol-revocation) 协议内撤销
与该 EIP 之前的版本以及类似的 EIP 不同，委托指定可以随时通过签署并发送一条带有账户当前 nonce 的新目标的 EIP-7702 授权来撤销。如果不采取此类行动，委托将永久有效。
### [](https://eips.ethereum.org/EIPS/eip-7702#self-sponsoring-allowing-txorigin-to-set-code) 自费：允许 `tx.origin` 设置代码 
允许 `tx.origin` 设置代码可以简化交易批处理，外部交易的发送者将成为签署账户。目前，ERC-20 的批准后转账模式需要两笔单独的交易，而通过这一提案，可以在一笔交易中完成。

一旦代码存在于外部拥有账户（EOA）中，任何时候该 EOA 的代码发起调用时，自费的 EIP-7702 交易都可以使 `msg.sender == tx.origin`。在没有 EIP-7702 的情况下，这种情况只能出现在交易的最顶层执行层。因此，该 EIP 打破了这一不变性，因此会影响包含  `require(msg.sender == tx.origin)` 检查的智能合约。该检查主要用于至少三个目的：

1. 确保 `msg.sender` 是一个 EOA（因为 `tx.origin` 总须是 EOA）。这个不变性不依赖于执行层的深度，因此不受影响。
2. 防范像闪电贷这样的原子性三明治攻击，这种攻击依赖于在执行目标合约之前和之后修改状态的能力，作为同一原子交易的一部分。这个保护将被此 EIP 打破。然而，以这种方式依赖 `tx.origin` 被视为不好的实践，并且已经可以通过矿工有条件地将交易包含在区块中来规避。（译者：[道易程的核心合约开发者Elon提交过一个防范闪电贷攻击的EIP](https://ethereum-magicians.org/t/union-lock-based-on-tstore-tload-can-avoid-flash-loan-attacks/19676)。） 
3. 防止重入攻击。  

(1) 和 (2) 的例子可以在以太坊主网部署的合约中找到，其中 (1) 更常见（且不受此提案影响）。另一方面，用例 (3) 受到此提案的更大影响，但该 EIP 的作者未发现任何这种形式的重入保护示例，尽管搜索并不全面。

这种情况——很多 (1)，一些 (2)，没有 (3)——正是该 EIP 的作者所预期的，因为：
- 在没有 `tx.origin` 的情况下确定 `msg.sender` 是否为 EOA 是困难的（如果不是不可能的话）。  
- 唯一一个安全的原子性三明治攻击执行环境是最顶层的环境，而 `tx.origin == msg.sender` 是检测该环境的唯一方法。
- 相比之下，有许多直接且灵活的方法来防止重入（例如，使用瞬态存储变量）。由于 `msg.sender == tx.origin` 只有在最顶层环境中为真，因此和其他更常见的方法，它是防止重入的冷门的工具。

还有其他方法可以减轻这种限制而不破坏不变性：

- 在 EOA 的应用场景中使用 `CALL*` 指令时，将 `tx.origin` 设置为一个常量 `ENTRY_POINT` 地址。
- 将 `tx.origin` 设置为从发送者或签字者地址派生的特殊地址。
- 禁止 `tx.origin` 设置代码。这将使简单批处理的用例变得不可能，但将来可以放宽。
### [](https://eips.ethereum.org/EIPS/eip-7702#forward-compatibility-with-future-account-abstraction) 未来账户抽象的向前兼容性 
 该 EIP 旨在使得最终的账户抽象技术方案高度向前兼容，而不过度依赖 [ERC-4337](https://eips.ethereum.org/EIPS/eip-4337) 或 RIP-7560 的任何细节。

具体而言：
- 用户签名的地址`address` 可以直接指向现有的 ERC-4337 钱包代码。
- 所使用的“代码路径”在许多情况下（尽管可能并非全部）在纯智能合约钱包世界中“仍然有意义”。
- 因此，它避免了“创建两个独立代码生态系统”的问题，因为在很大程度上，它们将是同一个生态系统。在这个解决方案下，确实有一些工作流程需要采取权宜之计，而在“最终账户抽象”下做得会更好，但这相对是一小部分。
- 它不需要添加任何操作码，这些操作码在后 EOA 世界中会变得无用。  
- 它允许 EOA 伪装成合约以便被包括在 ERC-4337 包中，这种方式与现有的 `EntryPoint` 兼容。
## [](https://eips.ethereum.org/EIPS/eip-7702#backwards-compatibility) 向后兼容性  
这个EIP打破了账户余额只能因来自该账户的交易而减少的规则。它还打破了在交易执行开始后EOA nonce不能增加的规则。这些破坏对内存池设计和其他EIP（如包含列表）都有影响。不过，由于账户在外部交易中是静态列出的，所以可以修改交易传播规则，以便不会转发冲突的交易。
## [](https://eips.ethereum.org/EIPS/eip-7702#security-considerations) 安全考虑  

### [](https://eips.ethereum.org/EIPS/eip-7702#secure-delegation) 安全代理
以下是代理合约需要谨慎注意的检查或陷阱或条件的非详尽清单，并需要获取账户授权的签名：
- 重放保护——（例如一个nonce）应该由代理方实现并签名。如果没有，恶意行为者可以重复使用签名，重复其影响。
- `value`—— 如果没有，恶意赞助商可能会导致被调用函数（callee）出现意想不到的效果。
- gas——如果没有，恶意赞助商可能会导致被调用方（callee）耗尽 gas 而失败，从而给被赞助方带来麻烦。
- `target` / `calldata`——如果没有，恶意行为者可能会在任意合约中调用任意函数。  

实现不佳的委托代理可能让恶意行为者**几乎完全控制签名者的EOA**。
### [](https://eips.ethereum.org/EIPS/eip-7702#setting-code-as-txorigin) 将代码设置为 `tx.origin`
允许EIP-7702的发送方也设置代码，可能会：
- 破坏依赖于 `tx.origin` 的原子三明治保护；
- 破坏类型为 `require(tx.origin == msg.sender)` 的重入保护。 
该EIP的作者认为，出于理由部分所述的原因，允许这样做的风险是可以接受的。
### [](https://eips.ethereum.org/EIPS/eip-7702#sponsored-transaction-relayers) 赞助交易中继者  

授权账户可以使赞助交易中继者支付 gas，而无需通过无效化授权（即增加账户的 nonce）或将相关资产转出账户来获得补偿。中继者（relayers）的设计应考虑这些情况，可能需要存入保证金或实施声誉系统。
### [](https://eips.ethereum.org/EIPS/eip-7702#front-running-initialization) 前置运行初始化
智能合约钱包开发者必须考虑在未执行的情况下设置账户代码的影响。合约通常是通过执行初始化代码来部署的，以确定要放入账户的确切代码。这使得开发者有机会同时初始化存储槽。账户的初始值不能被观察者替换，因为它们要么在创建交易的情况下由外部账户（EOA）签名，要么通过从初始化代码的哈希中确定合约地址来提交。

该 EIP 并未给开发者提供在委托代理期间运行初始化代码和设置存储槽的机会。为了防止观察者通过控制的账户前置运行委托的初始化，智能合约钱包开发者必须验证初始的 `calldata` 由 EOA 的密钥使用 `ecrecover`所做的签名。这确保账户只能用期望的值进行初始化。
### [](https://eips.ethereum.org/EIPS/eip-7702#transaction-propagation) 交易广播
允许 EOA 通过委托指定的方式表现得像智能合约，会给交易广播带来一些挑战。传统上，EOA 只能通过交易发送价值。这一不变性使节点能够静态地确定该账户交易的有效性。换句话说，单个交易只能使发送者账户的待处理交易失效。

通过这个 EIP，可能会导致其他账户的交易变得过时。这是因为一旦 EOA 委托给代码，该代码可以在交易的任何时刻被任何人调用。以静态方式判断账户的余额是否被转移变得不可能。

虽然有一些缓解措施，作者建议客户端对任何具有非零委托指定的 EOA 不接受超过一个待处理交易。这最小化了单个交易可能导致的交易失效的数量。另一种选择是扩展 EIP-7702 交易，附带调用者希望在交易过程中“激活”的账户列表。这些账户在包括它们的 EIP-7702 交易中仅表现为委托代码，从而使客户端能够静态分析和推理待处理交易。

一个相关的问题是，EOA 的 nonce 可能在每个交易中递增多次。由于客户端已经需要在更糟糕的情况下保持稳健（如上所述），这应该并非一个多大的安全注意事项。然而，客户端应意识到这种行为是可能的，并相应地设计他们的交易广播。
## [](https://eips.ethereum.org/EIPS/eip-7702#copyright) 版权
通过 [CC0](https://eips.ethereum.org/LICENSE) 放弃版权及相关权利。
## [](https://eips.ethereum.org/EIPS/eip-7702#copyright) 引用
请将此文献引用为：  
Vitalik Buterin ([@vbuterin](https://github.com/vbuterin)), Sam Wilson ([@SamWilsn](https://github.com/SamWilsn)), Ansgar Dietrichs ([@adietrichs](https://github.com/adietrichs)), Matt Garnett ([@lightclient](https://github.com/lightclient)), "EIP-7702: Set EOA account code [DRAFT]," _Ethereum Improvement Proposals_, no. 7702, May 2024. [Online serial]. Available: https://eips.ethereum.org/EIPS/eip-7702.
