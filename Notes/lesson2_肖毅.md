### 需要了解的基础知识
1. 动态静态数组的不同
    - 固定长度数组：`uint[5] a`
        - length
    - 动态长度：`uint[] a`
        - length
        - push()：添加成员
2. `delete`：初始化一个变量
3. `var`可以指代任意类型的变量
4. 循环与遍历的安全性
5. 程序运行错误检查和容错：`assert`与`require`
    - 两者功能基本相同，只有语义区别：
        - `assert`：查看程序运行后数据是否有错误，“确认”
        - `require`：检查程序输入值是否满足要求，“要求”
6. 数据存储 DATA LOCATION
    - storage：存储在EVM上永久空间
    - memory：存储在EVM中临时空间，`function`运行完之后就会释放
    - calldata：类似`memory`，在`function`执行完后抹除
    - 强制
        - 状态变量：`storage`
        - `function`输入参数：`calldata`
    - 默认
        - `function`返回参数：`memory`
        - 本地变量：`storage`
7. 规则
    - 相同存储空间变量赋值
        - 传递`reference`：EVM上的内存地址
    - 不同存储空间变量赋值
        - 拷贝


### 设计
1. 通过数组来存储多个员工的信息
    ```
    struct Employee {
        address id;
        uint salary;
        uint lastPayday;
    }
    ```
    - `Employee[] employees`
    - `function addEmployee`
    - `function removeEmployee`


### ==要点==
1. 创建新的`struct`对象不需要`new`：`Employee(employee, salary, now)`
2. 在循环中，若要求的操作已完成，记得使用`return`退出，避免`gas`的无谓消耗
3. `var (empl, i)`可用于接收两个返回值
4. 自定义`struct`不能暴露出去，有`function`需要传入或者返回`struct`类型时需要设置为`private`
5. 直接修改返回的变量改变的只是`memory`中的拷贝；通过`index`来修改变量的话，改变的就是`storage`中的值

### 小结
1. 错误检测
    - `assert(bool)`
    - `require(bool)`
2. 数组
3. `struct`
4. 数据存储
5. 语法
    - `for`
    - `delete`
6. 其他
    - 辅助`function`
    - 可见度


### 作业
- 完成智能合约添加100ETH到合约中
- 加入10个员工，每个员工的薪水都是1ETH
- 每次加入一个员工后调用`calculateRunway`这个函数，并且记录消耗的`gas`是多少
- `gas`变化么？如果有为什么？
- 如何优化`calculateRunway`这个函数来减少`gas`的消耗？
- 提交：智能合约代码，`gas`变化的记录，`calculateRunway`函数的优化


### 问题
1. ==`transaction cost`与`execution cost`的区别？==

答：**`Transaction cost`** are based on the cost of sending data to the blockchain. There are 4 items which make up the full transaction cost:
- the base cost of a transaction (21000 gas)
- the cost of a contract deployment (32000 gas)
- the cost for every zero byte of data or code for a transaction.
- the cost of every non-zero byte of data or code for a transaction.

**`Execution costs`** are based on the cost of computational operations which are executed as a result of the transaction.

![Gas Fee](https://s1.ax1x.com/2018/06/27/PPHfQ1.jpg)
