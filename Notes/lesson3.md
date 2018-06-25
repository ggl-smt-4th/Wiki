### Solidity 的继承
Solidity通过复制包括多态的代码来支持多重继承。

所有函数调用是虚拟(virtual)的，这意味着最远的派生方式会被调用，除非明确指定了合约。

当一个合约从多个其它合约那里继承，在区块链上仅会创建一个合约，在父合约里的代码会复制来形成继承合约。

基本的继承体系与python有些类似，特别是在处理多继承上面。

[官方文档reference](http://www.tryblockchain.org/Solidity-Inheritance-%E5%A4%9A%E7%BB%A7%E6%89%BF.html)

### C3 Linearization
C3算法最早被提出是用于Lisp的，应用在Python中是为了解决原来基于深度优先搜索算法不满足本地优先级，和单调性的问题。 本地优先级：指声明时父类的顺序，比如C(A,B)，如果访问C类对象属性时，应该根据声明顺序，优先查找A类，然后再查找B类。 单调性：如果在C的解析顺序中，A排在B的前面，那么在C的所有子类里，也必须满足这个顺序。

[C3算法reference](https://www.jianshu.com/p/a08c61abe895)

### Solidity mapping key
(整理by 顺达学委)

- mapping是在EVM中的storage中出现的数据结构，storage天生就是Key-Value实现的，所以mapping直接基于storage，将key映射到一个内存地址。mapping中所有可能的键已被虚拟化的创建，被映射到一个默认值，这也导致mapping没有长度，是不可遍历的。

- [官方文档中关于mapping的说明](http://solidity.readthedocs.io/en/latest/types.html#mappings)

- [Store data in mapping vs. array](https://ethereum.stackexchange.com/questions/2592/store-data-in-mapping-vs-array)

- [mapping和hash table的区别](https://ethereum.stackexchange.com/a/17410)