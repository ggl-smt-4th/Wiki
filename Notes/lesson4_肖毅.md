### 需要了解的基础知识
1. **TRUFFLE**
    - 编译
    - 部署
    - 测试
    - 包管理
    - 前端
2. **主要命令**
    - `truffle compile`
    - `truffle migrate`
    - `truffle console`
    - `truffle test`
3. **`WEB3.JS`**
    - 一个`javascript`和区块链交互的桥梁
4. **`WEB3`调用合约方程**
    - 方程名.call()：只能用于只读的方程，修改状态变量会失败
    - 方程名.send() or 方程名()
5. 前端开发
    - `truffle box`
    - `web3`
6. 测试
    - `javascript`
        - `Mocha`
        - `Chai`
    - `solidity`
    - 运行测试：`truffle test`

### 要点
1. `ABI(Application Binary Interface)`：详细记述整个`function`的结构，给外部的程序(`javascript/go/java`)提供接口，来生成相对`offchain`的映射
2. `migration`
    - `migration script`：按照顺序编号，部署时按顺序执行（分阶段）
    - `migration contract`：永久记录项目合约部署到了第几步
    - 好处：可以追加部署，减少消耗
3. `truffle migrate`之后，`ABI`中就增加了`networks`信息（网络地址）
4. `tx.origin`表示合约的创建人
5. `javascript`测试：可以用来模拟前端的使用，整合性测试。
    - 拥有更强大的语法
    - 易于实现异常的捕捉
6. `solidity`测试：非常简洁，可以用来进行单元测试
    - 能对内部`function`进行调用：通过继承的方式

### 小结
1. 安装`truffle`
2. `truffle init`：初始化一个`truffle`项目
3. `truffle compile`：编译智能合约
4. `truffle migrate`：将智能合约部署到`ethereum`网络上
5. `truffle console`：与部署的智能合约进行交互
6. `truffle test`：测试我们的智能合约
7. `truffle unbox`：下载已配置好的前端环境，来开发前端应用
8. `truffle` 前端

### 作业
1. 将第三课完成的`payroll.sol`程序导入`truffle`工程
2. 在`test`文件夹中，写出对如下两个函数的单元测试：
    ```
    function addEmployee(address employeeId, uint salary) onlyOwner
    
    function removeEmployee(address employeeId) onlyOwner employeeExist(employeeId)
    ```
    思考一下我们如何能覆盖所有的测试路径，包括函数异常的捕捉
3. （加分题，选作）
    写出对以下函数的基于`solidity`或`javascript`的单元测试
    ```
    function getPaid() employeeExist(msg,sender)
    ```
    Hint：思考如何对`timestamp`进行修改，是否需要对所测试的合约进行修改来达到测试的目的？

### 问题
