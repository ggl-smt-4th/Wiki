### 需要了解的基础知识
1. 什么是`DAPPS`?

### 设计
1. 员工系统中需要哪几个组件（前端）？
    - 通用信息：当前合约的金额、员工数、可支付次数
    - 员工组件：员工向合约发送支付请求
    - 雇主组件：删除员工、更新员工、新增员工、打钱
2. 缺点
    - 界面简陋
    - 对用户身份的假设：假设第一个账户是雇主
    - 交互不人性化

### 要点
1. 部署合约时，所有合约（`Ownable, SafeMath, Payroll`）都需要部署到`2_deploy_contracts.js`中，且`Ownable, SafeMath link Payroll`
2. `componentDidMount()`：当组件被加载到浏览器`DOM`中的时候再运行的代码
3. `solidity`中时间以秒为单位存储，在`javascript`中时间以毫秒为单位：从`solidity`中读取出来后需要`* 1000`再交给`javascript`处理
4. `const { account, payroll, web3, mode } = this.state;`：从`state`中抽取各项属性分别赋值给局部变量`account, payroll, web3, mode`
5. 与`web3`交互时传入的`js object`
    ```
    {
        from: account,              表示调用地址
        value: web3.toWei(fund),    表示传入金额
        gas: 10000                  表示此操作最多消耗的gas
    }
    ```
6. `web3`中返回的结果，如果是数字的话有一个封装，要获取实际的数字需要调用`toNumber()`
7. `ant design`是`react`社区中非常流行的前端库，由阿里巴巴贡献
8. 布局的栅格化系统，我们是基于行（row）和列（col）来定义信息区块的外部框架，以保证页面的每个区域能够稳健地排布起来
    - 通过`row`在水平方向建立一组`column`（简写col）
    - 你的内容应当放置于`col`内，并且，只有`col`可以作为`row`的直接元素
    - 栅格系统中的列是指1到24的值来表示其跨越的范围。例如，三个等宽d的列可以使用`.col-8`来创建
    - 如果一个`row`中的`col`总和超过24，那么多余的`col`会作为一个整体另起一行排列
    - 栅格常常需要和间隔进行配合，你可以使用 `Row` 的 `gutter` 属性，我们推荐使用 `(16+8n)px` 作为栅格间隔。(n 是自然数)
9. 使用table组件时，如何获取这一行的数据：
    - 是配置`rowSelection`，在`onSelect`函数被调用的时候，可以获取当前行以及其子行的数据
    - 配置`Column`中的`render`属性，这个属性对应一个函数，`fun(text, record, index){}`, 这是个渲染函数，参数分别为当前行的值，当前行数据，行索引，`return`可以决定表格里最终存放的值。

### 小结


### 作业
- 按照课程要求，完成基本用户交互界面


### 问题
