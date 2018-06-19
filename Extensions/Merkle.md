### Merkle Tree

Merkle Tree是存储hash值的一棵树。Merkle树的叶子是数据块的hash值，非叶节点是其对应子节点串联字符串的hash。

在最底层，把数据分成小的数据块，有相应地哈希和它对应。往上走，把相邻的两个哈希合并成一个字符串，然后运算这个字符串的哈希，这样每两个哈希就结婚生子，得到了一个”子哈希“。如果最底层的哈希总数是单数，那到最后必然出现一个单身哈希，这种情况就直接对它进行哈希运算，所以也能得到它的子哈希。于是往上推，依然是一样的方式，可以得到数目更少的新一级哈希，最终必然形成一棵倒挂的树，到了树根的这个位置，就剩下一个根哈希了，我们把它叫做 Merkle Root。

![](http://ojtlrnjhy.bkt.clouddn.com/2018-01-02-834896-20160527163537178-321412097.png)

参考文章

[http://www.cnblogs.com/fengzhiwu/p/5524324.html](http://www.cnblogs.com/fengzhiwu/p/5524324.html)


### Merkle Patricia Trees 在 Ethereum的应用

每个以太坊区块头不是包括一个Merkle树，而是为三种对象设计的三棵树，称之为Merkle Patricia tree，包括：

	交易tx root
	收据receipt root
	状态state root

其中，状态树的情况会更复杂些。因为账户状态信息在Ethereum中会经常变更，新的账户会被插入，合约中的storage也会经常变动，因此需要一种数据结构，能够在插入，删除，更新等操作之后，快速计算出新的root值，不需要重新计算整棵树。因此Ethereum中的状态树基本上包含了一个键值映射，其中的键是地址（账户地址&合约地址），而值包括nonce,balance,codeHash以及storageRoot（storageRoot是Merkle树根，存储合约中的storage数据）。

state root (仅包含key) 结构图：

![](http://ojtlrnjhy.bkt.clouddn.com/2018-01-02-KZILu.png)

state root (包含key - value) 结构图

![](http://ojtlrnjhy.bkt.clouddn.com/2018-01-02-020723.png)

参考文章

[https://blog.ethereum.org/2015/11/15/merkling-in-ethereum/](https://blog.ethereum.org/2015/11/15/merkling-in-ethereum/)

[https://github.com/ethereum/wiki/wiki/Patricia-Tree](https://github.com/ethereum/wiki/wiki/Patricia-Tree)

[http://gavwood.com/Paper.pdf](http://gavwood.com/Paper.pdf)


### 合约的storage读取

合约中storage状态存储在上面介绍的storageRoot的Merkle树中，首先通过合约地址为键，在stateRoot状态树中进行搜索，找到对应value中的storageRoot，接着在storageRoot树中对合约storage进行键值映射查找。

在合约中的所有storage状态，是从第一个按序进行索引的，一个索引键占用256个字节。

对于合约中不同类型的storage，键的计算方式可以参考文章 

[https://medium.com/aigang-network/how-to-read-ethereum-contract-storage-44252c8af925](https://medium.com/aigang-network/how-to-read-ethereum-contract-storage-44252c8af925)

最后，通过使用eth_getStorageAt方法，传入合约地址，storage的键，区块号（或者latest, earliest,pending三种状态的区块）进行storage的value查询，参考文章

[https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getstorageat](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getstorageat)
