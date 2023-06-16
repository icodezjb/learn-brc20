# learn brc20

1. [https://unisat.io/](https://unisat.io/)
2. [https://brc-20.io/](https://brc-20.io/)
3. brc20 基于ordinals协议
   [https://docs.ordinals.com/](https://docs.ordinals.com/)
4. brc-20-experiment
   [https://domo-2.gitbook.io/brc-20-experiment/](https://domo-2.gitbook.io/brc-20-experiment/)
5. [https://github.com/casey/ord/blob/master/bip.mediawiki](https://github.com/casey/ord/blob/master/bip.mediawiki)
6. [https://github.com/casey/ord](https://github.com/casey/ord)
7. 比特币浏览器 [https://mempool.space/zh/](https://mempool.space/zh/)
8. 比特币地址格式 [https://en.bitcoin.it/wiki/List_of_address_prefixes](https://en.bitcoin.it/wiki/List_of_address_prefixes)
   *Address on invalid network*
9. [https://github.com/BlockchainCommons/Learning-Bitcoin-from-the-Command-Line](https://github.com/BlockchainCommons/Learning-Bitcoin-from-the-Command-Line)
10. [Bitcoin Transaction Editor](https://bc-2.jp/tools/txeditor2.html)
11. [Ordinal NFT实现原理及Bitcoin Regtest测试网Mint教程](https://learnblockchain.cn/article/5376)
12. [Ordinals Inscription铸造过程解析](https://learnblockchain.cn/article/5782)
13. [逐个字节分析Ordinal的铸造交易](https://learnblockchain.cn/article/5822)
14. [如何估算Ordinals铸造费用](https://buidler.space/Ordinals-0a26ef31ca8a42e8b0df419f15d44327)
15. [比特币钱包地址可以包含 34 到 62 个字母数字字符](https://www.doubloin.com/learn/how-long-are-bitcoin-addresses)

16. 本地搭建 ordinals inscriptions测试环境

    ```bash
    (1) ord和bitcoin-core
    ord 下载编译 https://github.com/casey/ord
    bitcoin-core下载 https://github.com/bitcoin/bitcoin/releases
    
    (2) 本地bitcoin测试网
    
    # walelt: ~/.bitcoin/regtest/wallets/test
    bitcoin-wallet -chain=regtest -wallet=test create
    bitcoin-qt -chain=regtest -wallet=test -txindex=1 -server -debug -printtoconsole -fallbackfee=0.0002 -txconfirmtarget 1
    # walelt: ~/.bitcoin/regtest/wallets/ord
    # 手动挖矿
    bitcoin-cli -chain=regtest -rpcwallet=test -generate 101
    
    (3) ordinals
    # 创建ord钱包
    ord --chain=regtest wallet create
    # 创建taproot地址接收sats
    ord --chain=regtest wallet receive
    
    (4) 在bitcoin-qt页面操作, 向ord地址转一些bitcoin
    
    (5) 查询余额
    ord --chain=regtest wallet balance
    ord --chain=regtest wallet outputs
    
    (6) 创建 inscriptions
    ord --chain=regtest wallet inscribe --fee-rate 10000  ./inscribe.txt
    {
      "commit": "7c0a36bba5bac13ca99f685f50ed3fe6ca9e78ec1812172ed66a887e2621c6a3",
      "inscription": "c0b634f4f069ffdecf5ea2bbc01f168b390703cfc56254c7f1f00052589dc33ei0",
      "reveal": "c0b634f4f069ffdecf5ea2bbc01f168b390703cfc56254c7f1f00052589dc33e",
      "fees": 2940000
    }
    打包区块
    bitcoin-cli -chain=regtest -rpcwallet=test -generate 6
    
    注意: 在ord资源管理器中可以查询 c0b634f4f069ffdecf5ea2bbc01f168b390703cfc56254c7f1f00052589dc33ei0
    (1) 运行 ord --chain=regtest server --address 127.0.0.1 --http-port 8080
    (2) 浏览器打开 http://localhost:8080/inscription/c0b634f4f069ffdecf5ea2bbc01f168b390703cfc56254c7f1f00052589dc33ei0
    
    (7) 查询 inscriptions
    ord --chain=regtest wallet inscriptions
    ord --chain=regtest wallet transactions
    
    (8) 发送 inscriptions
    ord --chain=regtest wallet send --fee-rate 10000 bcrt1pj6qaq5lxl454mr375x7ttfu2n68kad7xaavuf7trh88yxazhh96q4df0nd c0b634f4f069ffdecf5ea2bbc01f168b390703cfc56254c7f1f00052589dc33ei0
    bitcoin-cli -chain=regtest -rpcwallet=test -generate 6
    
    ```

17. 总结
    ```txt
    ordinals(https://docs.ordinals.com/introduction.html)
    将bitcoin的最小单位聪(satoshi), 按照被挖出来的顺序编号, 使得每个聪都有唯一的编号.
    ordinals 提供了wallet,索引数据库和浏览器等基础设施,使得可以将任意数据锚定到聪(这一过程称为inscribe, 铭刻),并能方便的查看聪上绑定的数据(inscription, 铭文).
    
    ordinals 提供的这套基础设施,是bitcoin nft的基础,同时也衍生出了BRC-20.
    
    BRC-20 是指由名为“Domo”的匿名加密货币开发者于2023年3月推出的实验性令牌标准(https://domo-2.gitbook.io/brc-20-experiment/).
    与以太坊上的 ERC-20 标准一样,BRC-20 允许用户使用比特币的ordinals协议发行同质化代币.
    
    unisat 提供了插件wallet, brc20索引数据库，brc20发行和转账等工具，以及BRC20交易市场, 这些配套设施加速了BRC-20的传播.
    
    但是基于二次索引解析的BRC20严重依赖中心化的索引数据库, 在跨服务场景中始终面临"双花攻击"
    https://www.investing.com/news/cryptocurrency-news/unisat-halts-marketplace-following-doublespend-attacks-3062947
    https://www.binance.com/en/feed/post/455238
    
    理论上，可以基于ordinals铭文构建任何Token规则，只需要建立二次索引解析这些铭文
    近期，ORC-20是由币安和OKX联合推出的另一种Bitcoin同质化代币标准，有待市场检验
    ```
