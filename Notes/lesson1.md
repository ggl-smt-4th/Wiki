## 1.Solidity 数据类型
http://solidity.readthedocs.io/en/develop/types.html#value-types

## 2.单位系统  Address 类型， Block类型，msg类型
[http://solidity.readthedocs.io/en/develop/units-and-global-variables.html#](http://solidity.readthedocs.io/en/develop/units-and-global-variables.html#)
## 3.Solidity constant 对变量的影响
[Constant Variable in doc](http://solidity.readthedocs.io/en/develop/contracts.html#constant-state-variables)

## 4.当你部署合约的时候，你其实干了什么
智能合约可以简单的理解为一段可执行的程序片段，具体的代码经过solidity编写之后，发布到区块链上。而以太坊的智能合约也可以理解为一个特殊的交易（包括可执行代码的），被发送出去后会被矿工打包记录在某一个区块中，当需要调用这个智能合约的方法时只需要向这个智能合约的地址发送一笔交易即可。
因为触发的条件和打钱地址都已经被编写在代码里，存储在区块链上，所以可以最大程度的排除人为因素的干扰。

## 5.Gas是什么，为什么会被消耗。 
reference:https://zhuanlan.zhihu.com/p/24012669

当你激活一个智能合约的时候，你在要求整个网络内的每个矿工个体分别执行里面的运算。这会花费他们的时间和精力，Gas是你为这项服务向矿工们支付的机制。
报酬是小额的以太币，想要运行智能合约的人的需要支付报酬来使合约工作。类似于投放一个硬币到自动唱机里。
付款款项（单位以太币）＝ Gas数量（单位Gas） x Gas价格（单位以太币／Gas）
### 5.1 Gas数量
智能合约越复杂（计算步骤的数量和类型，占用的内存等），用来完成运行就需要越多Gas。类比自动唱机，歌曲的时间越长，音量越大，让它工作你需要支付的则越多。
### 5.2 Gas 价格
任何特定的合约所需的运行合约的Gas数量是固定的，由合约的复杂度决定，而Gas价格由想运行合约的人规定，在他们提交运行合约请求的时候（有点类似于比特币的交易费）。每个矿工会根据Gas的价格的高低来决定他们是否想作为区块的一部分去运行此合约。如果你希望矿工运行你的合约，你最好提供高一点的Gas价格。在某种程度是这是一场基于合约运行有多愿意付费驱动下的竞价。
### 5.3 为什么需要Gas
让智能合约花费Gas/以太币/钱可以防止人们随意激活合约， 解决了垃圾交易以及相关问题，如果运行智能合约免费，此类问题会发生。
