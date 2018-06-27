# HELLO WORLD智能合约
```
//程序版本
pragma solidity ^0.4.21;

//合约声明
contract SimpleStorage {
    //状态变量
    uint storedData;
    
    //合约方法
    function set(uint x) {
        storedData = x;
    }
    
    //此处constant只是建议，不起作用，函数还能修改
    function get() constant returns (uint) {
        return storedData;
    }
}
```
- 写明程序版本：有利于开源社区的互动
- 在最新版本solidity中，constant -> view（仍然不是enforced）：在函数中仍然可以改变其它合约中的参数
- pure：比constant的要求更加严格（不光无法 写/改变 成员变量，甚至无法读），用于做库的时候比较有用——比如算椭圆加密曲线或者算hash
- 执行**状态改变**需要消耗gas


# 简单的员工系统
### 背景介绍
- 传统的员工系统：*大公司外包，小公司老板直接转账*
- 问题：
    1. 成本
    2. 信任（拖欠工资）
- 目标
    1. 高效低成本
    2. 防止违约，拖欠工资
### 需要了解的基础知识
1. 类型系统
- 静态类型：需要申明类型
2. 布尔型BOOL
- true
- false
- 操作符
    - !
    - &&
    - ||
    - ==
    - !=
3. 整型UINT/INT
- 操作符
    - 比较：<=, < , ==, !=, >=, >
    - 位操作：&, |, ^(非), ~(异或), >>, <<
    - 计算：+, -, *, /, %, **

    **solidity当中没有浮点数**
4. 地址ADDRESS：**看成一个原生的比较复杂的类型**
- address.balance：重要的成员变量
- address.transfer(value)：能将调用后产生的异常返回给调用者手中，**转钱时推荐使用**
- address.send(value)：调用后返回 true|false 给调用者
- address.call, address.callcode, address.delegatecall：**更多地用于智能合约之间相互调用**

5. **ether单位**
- wei
- szabo = 10^12 wei
- finney = 10^15 wei
- ether = 10^18 wei

6. 时间单位
- seconds
- minutes
- hours
- days
- weeks
- years

7. **block块**：
> 像singleton，整个程序里面只有一个，谁都能access

- block.blockhash(uint blockNumber) returns (bytes32)
- block.coinbase(address)：谁挖到了这个block
- block.difficulty(uint)
- block.gaslimit(uint)
- block.number(uint)：这是第几个block
- block.timestamp(uint)：block目前创建的时间
- now：相当于block.timestamp的快捷方式

8. **msg消息**：全局变量，包含智能合约中函数调用者的信息和数据
- msg.data：包含函数调用所有的raw信息，如名字等
- msg.gas(uint)：查看函数调用者附带了多少gas
- msg.sender(address)：谁调用了这个函数
- msg.sig
- msg.value(uint)：以wei为单位的msg中带有的钱


### 设计
1. 薪水将全部基于以太币
    - uint salary
    - address frank
2. 每30天发放一次
    - uint payDuration
    - uint lastPayday
3. 如何发放薪水
    - 定时器？：**只有被动调用的方式**
    - 设计范式
        > 通过合约做可信中介，月初时老董先存够30天工钱，frank开始干活，30天结束后才可以从合约上取钱
    
    - 在合约上存钱，frank在每30天领取薪水
        - function addFund
        - function calculateRunway：获取余额还能支付几次
        - function hasEnoughFund
        - function getPaid


### 要点
1. 在一个合约中，如果要一个函数能够接受“钱”的话，必须加上`payable`
    > `function addFund() payable`

2. 在一个合约中，`this`表示对这个合约的引用，是一个`address`类型
3. `this.`的调用模式在`EVM`中是通过`msg`来实现的，**代价非常高，需要支付很多`gas`**；如果采用直接调用合约中方法的方式，在`EVM`中是`jump`的方式，消耗少很多
4. 执行失败需要给出异常提示，`throw`已经deprecated，应该采用`revert()`
    > 如果采用`throw`抛出异常，所有的剩余`gas`都会消耗殆尽；而采用`revert()`的话，会回滚以前所有`transaction`的状态，同时交还没有消耗完的`gas`
5. **一定要在内部变量修改完之后，再给外部转账**
6. **每个运算都要花费真金白银，重复的运算绝对不可以**
7. `solidity`中变量的作用域类似`javascript`，与`java`不同：只要在`function`中定义了局部变量，这个局部变量的作用域会穿透`{}`，在整个`function`中都有效

### 小结
1. 合约的基本结构
    - 程序版本
    - contract声明
    - 状态变量声明
    - function声明
2. 类型系统
     - bool
     - uint/int
     - address
3. 全局变量
    - Ether变量
    - 时间变量
    - block
    - msg
4. 关键词
    - constant
    - payable
    - this
    - revert
5. 其他
    - if
    - 本地变量

### 作业
可调整地址和薪水的员工系统

### 问题
#### 1. 关于 constant/view 以及 pure，假如是严格执行的话，限制的是自己本身这个function还是限制修改其它的合约状态呢？

答：无法修改本合约的其他状态变量

在Solidity v4.17之前，只有constant，后续版本将constant拆成了view和pure。view的作用和constant一模一样，可以读取状态变量但是不能改；pure则更为严格，pure修饰的函数不能改也不能读状态变量，只能操作函数内部变量，否则编译通不过。

#### 2. 全局变量block可以理解为链上的那个block吧？

答：目前看来应该是的。

#### 3. `function addFund() payable`为什么没有传入参数？

答：调用合约函数相当于发起一次tranction 所以是可以转账的 只不过一般的调用默认转账金额是0 这个在后面课程会介绍怎么使用eth主网


#### 4. 使用require（condition）和使用if，会有gas消耗上的差别吗？

答：gas消耗还不太确定，但感觉不过比使用if多。
主要的原因还是会“确保”提高合约代码逻辑条理清晰

#### 5. `employeeAddress != 0x0` 是怎样理解的

答：While the zero address is only intended for contract registration, it sometimes receives payments from various addresses. There are two explanations for this: 
- either it is by accident, resulting in the loss of ether 
- or it is an intentional ether burn (see [burning_ether]). 
    >If you want to do an intentional ether burn, you should make your intention clear to the network and use the specially designated burn address instead
