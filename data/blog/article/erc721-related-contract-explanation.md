---
title: ERC721相关合约解释
date: '2023-06-13'
tags: ['solidity', 'blockchain', 'ethereum', 'contract', 'nft']
draft: false
summary: Create New Hardhat(TS)+Solidity Project.
---

# ERC721 相关合约解释

## 基础 ERC721 合约

- **Context**：提供有关当前执行上下文的信息，包括交易的**发送者**及其**数据**；
- **ERC165**：用于检测**合约实现的接口**的功能；
- **IERC721**：是**ERC721**的基础接口；
- **IERC721Metadata**：用于提供合约的元数据，实现 name、symbol、tokenURI 接口；

> openzeppelin 官方文档：https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721；
>
> EIP165：https://eips.ethereum.org/EIPS/eip-165

## OpenZeppelin 拓展合约/接口

### ERC721Enumerable

增加了一个枚举功能，使得开发者可以方便地获取代币所有者的地址、代币 ID 以及代币总数等信息，**提高合约中 NFT 的可访问性**。

在`ERC721Enumerable`中，代币的所有者可以按照代币的索引值进行枚举，这意味着开发者可以通过代币的索引值来查询代币的所有者地址，或者查询某个地址所拥有的代币的索引值。这种枚举功能可以为一些应用场景提供便利，比如游戏中的排行榜、NFT 市场中的搜索和过滤等。

`ERC721Enumerable`的实现方式是在 ERC721 合约中增加一个名为“`_tokenOfOwnerByIndex`”的方法，它可以根据代币所有者的地址和代币的索引值来返回代币的 ID。同时，还增加了一个名为“`totalSupply`”的方法，用于返回代币的总数。

例如：`ERC721Enumerable`增加了一个 `tokenOfOwnerByIndex()` 方法，该方法允许按照`ownerAddress` 和`index` 来获取该地址下的某个 NFT，而`index` 参数的范围应该在 0 到 `balanceOf(owner) - 1` 之间。

总结：**在原先`ERC721`只能根据`NFT ID`查找`Owner`的功能上，新增了根据`index`索引查找`NFT ID`的方法。**

> openzeppelin 官方文档：https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721Enumerable

### ERC721Pausable

增加了暂停功能，使得合约的部分功能可以被暂停或恢复，以应对紧急情况或者维护需求。

在`ERC721Pausable`中，合约的所有者可以通过调用`pause`方法来暂停合约的部分功能，例如代币的转移、授权等操作。此时，任何人都无法执行被暂停的操作，直到合约的所有者调用`unpause`方法来恢复合约的功能。

`ERC721Pausable`的实现方式是在 ERC721 合约中增加一个名为“paused”的状态变量，用于记录合约是否被暂停。同时，还增加了一个名为“pause”和“unpause”的方法，用于控制合约的暂停和恢复。

总结：**可暂停合约 NFT 代币的传输、铸造和燃烧，但必须有权限控制**。

> openzeppelin 官方文档：https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721Pausable

### ERC721Burnable

增加了销毁功能，使得代币可以被销毁并从代币合约中移除。

在`ERC721Burnable`中，代币的所有者可以通过调用`burn`方法来销毁自己的代币。此时，代币将被从代币合约中移除，不再存在于代币合约中。

`ERC721Burnable`的实现方式是在 ERC721 合约中增加一个名为“\_burn”的方法，用于销毁代币并从代币合约中移除。同时，还增加了一个名为“\_exists”的方法，用于判断代币是否存在于代币合约中。

总结：**可以销毁自己的 ERC721 Token**。

> openzeppelin 官方文档：https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721Burnable

### ERC721URIStorage

具有存储每个 NFT 的元数据 URI 的功能，元数据 URI 是指包含有关 NFT 的元数据信息的 URL。

在 `ERC721URIStorage` 中，**每个 NFT 都有一个与之关联的 URI**，可以通过 `tokenURI` 函数获取。此外，`ERC721URIStorage` 还提供了一些函数来管理和更新 NFT 的 URI，例如 `setTokenURI` 和 `clearTokenURI`。

这使得开发人员可以更轻松地管理 NFT 的元数据信息，而不必将这些信息存储在智能合约中。

总结：**可以让每个 ERC721 Token 都有自己的（不同的）URI，用于节约 Gas**。

> openzeppelin 官方文档：https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721URIStorage

### ERC721Consecutive

添加了连续的 NFT 编号功能，在标准 ERC721 中，每个 NFT 都有一个唯一的标识符（ID），但这些 ID 并不一定是连续的整数。

而在 `ERC721Consecutive` 中，NFT 的 ID 是连续的整数，这使得开发人员可以更轻松地管理 NFT，并且可以更方便地进行排序和计数。

这些函数使得开发人员可以更加灵活地管理 NFT 的 ID，而不必担心 ID 的重复或不连续。

总结：**可以在创建合约时批量 mint 大量 ID 连续的 NFT**。

> openzeppelin 官方文档：https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721Consecutive
>
> EIP-2309：https://eips.ethereum.org/EIPS/eip-2309

### ERC721Votes

添加了投票功能，其中每个 NFT 计为 1 个投票单位。

它允许 NFT 持有者对某些事情进行投票，例如投票选举、决策制定或社区治理。这个标准定义了投票的接口和事件，以及 ERC721 标准中定义的标准接口和事件。

在`ERC721Votes`中，每个 NFT 代表一个选民，NFT 的所有者可以使用其代表的选民身份进行投票。

此外，该标准还定义了一个投票委托机制，使选民可以将其投票权委托给其他人，以便他们代表选民进行投票。

总结：**可使用 NFT 进行投票。**

> openzeppelin 官方文档：https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721Votes

### ERC721Royalty

添加了版税功能，使用 ERC2981 NFT 版税标准扩展 ERC721，这是一种检索版税支付信息的标准化方法，它允许 NFT 持有者在其 NFT 被转让时收取一定比例的版税。

这个标准定义了版税的接口和事件，以及 ERC721 标准中定义的标准接口和事件。

在 ERC721Royalty 中，NFT 的所有者可以设置版税比例和收款地址，当 NFT 被转让时，版税将自动发送到指定的收款地址。

此外，该标准还定义了一个查询接口，用于查询 NFT 当前的版税比例和收款地址。

这个标准可以为 NFT 所有者提供一种新的收入来源，并使他们能够从其 NFT 的成功销售中获得一定比例的收益。

总结：**可自愿为 NFT 创作者支付版税。**

> openzeppelin 官方文档：https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721Royalty
>
> ERC-2981：https://eips.ethereum.org/EIPS/eip-2981

### ERC721Wrapper

允许将其他资产包装为 ERC721 代币并进行交易。

这个标准定义了一个接口，用于将其他资产包装为 ERC721 代币，并在代币被转让时将资产转移给新的所有者。

例如，可以使用 ERC721Wrapper 将现实世界中的艺术品、房地产或其他资产包装为 ERC721 代币，并在区块链上进行交易。

这个标准还定义了一个查询接口，用于查询代币所包装的资产。

ERC721Wrapper 为现实世界的资产提供了一个数字化的交易平台，使其更容易流通和交易。

总结：**可以将 NFT 包装（兑换）为另一种 NFT，并可以随时兑换回来。**

> openzeppelin 官方文档：https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721Wrapper

## 其它拓展合约/接口

### ERC721A

优化了令牌元数据的浪费存储，将所有权状态更新限制为每批铸造一次，而不是每铸造 NFT 一次，节约了大量 Gas 费用。

存储 NFT 信息时，只存储第一个 NFT ID 的信息，在铸造一组连续 ID 的 NFT 时有效节约 Gas。

假设 Alice 铸造了代币 #100、#101、 #102，Bob 铸造了代币 #103、#104，那么即是 #100 存储了 Alice ，#103 存储了 Bob，查找 #102 的话，`ownerOf`会不断向上寻找直到有`owner`。

转移一个没有明确所有者地址集的 tokenID，合约在所有 tokenID 上运行一个循环，直到它到达第一个具有明确所有者地址的 NFT 以找到有权的所有者转移它，然后设置一个新的所有者，从而多次修改所有权状态以保持正确的分组。

总结：**通过减少存储信息来优化 Gas。**

> ERC721A：https://www.azuki.com/erc721a
