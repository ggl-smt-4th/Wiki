### 异常处理(revert, assert和require)
(http://solidity.readthedocs.io/en/develop/control-structures.html#error-handling-assert-require-revert-and-exceptions)

回滚机制（reverting）是为了确保transaction的原子性，主要分为require样式和assert样式。

    Internally, Solidity performs a revert operation (instruction 0xfd) for a require-style exception and executes an invalid operation (instruction 0xfe) to throw an assert-style exception. In both cases, this causes the EVM to revert all changes made to the state. The reason for reverting is that there is no safe way to continue execution, because an expected effect did not occur. Because we want to retain the atomicity of transactions, the safest thing to do is to revert all changes and make the whole transaction (or at least call) without effect. Note that assert-style exceptions consume all gas available to the call, while require-style exceptions will not consume any gas starting from the Metropolis release.

assert用于检查程序运行条件是否满足（例如数组访问是否越界），require用于检查用户输入或外部调用返回值。

    The assert function should only be used to test for internal errors, and to check invariants. The require function should be used to ensure valid conditions, such as inputs, or contract state variables are met, or to validate return values from calls to external contracts.

异常一般会由子调用向上传递。但是send和底层调用call、delegatecall、callcode除外，它们只会返回false（注意：如果被调用账户不存在，call、delegatecall和callcode返回true）。

补充阅读：

- [对于revert、assert和require的使用](https://medium.com/blockchannel/the-use-of-revert-assert-and-require-in-solidity-and-the-new-revert-opcode-in-the-evm-1a3a7990e06e)
- [REVERT instruction](https://github.com/ethereum/EIPs/pull/206)
- [拜占庭分叉后yellow book变化](https://github.com/ethereum/yellowpaper/issues/229)
### 数据位置(storage、memory、calldata)
http://solidity.readthedocs.io/en/develop/types.html#data-location

    ### Summary

    - Forced data location:
        parameters (not return) of external     functions: calldata
        state variables: storage
    - Default data location:
        parameters (also return) of functions: memory
        all other local variables: storage
补充阅读：

[Solidity的数据位置特性深入详解](http://me.tryblockchain.org/solidity-data-location.html)

    storage转换为storage: 引用
    memory赋值给storage(状态变量): 拷贝
    memory赋值给storage(局部变量): 报错
    storage转为memory: 拷贝
    memory转为memory: 引用

### 其他
1. [函数四种可见性(external、public、internal、private)](https://solidity.readthedocs.io/en/develop/contracts.html#visibility-and-getters)

2. [区块链编程语言Solidity语言函数可见性深入详解](http://www.jianshu.com/p/c3e3ccb466ec)
3. [判断函数调用者身份时可以采用函数修饰符(modifier)的方式](https://solidity.readthedocs.io/en/latest/contracts.html#function-modifiers)

(by 顺达、至诚)