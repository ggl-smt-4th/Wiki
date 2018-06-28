### 需要了解的基础知识
1. MAPPING：
    - 类似于`map(C++)`：`<key-value>`“健-值”对
    - `key`：bool, int, address, string类型
    - `value`：任何类型
    - 语法：`mapping(address => Employee) employees`
    - **特性：只能作为成员变量，不能作为本地局部变量**
2. MAPPING底层实现
    - 不使用数组+链表，不需要扩容
    - `hash`函数为`keccak256hash`，在`storage`上存储，理论无限大的`hash`表
    - 局限：无法使用`for循环`遍历整个`mapping`
    - 赋值：`employees[key] = value`
    - 取值：`value = employees[key]`
        - `value`是引用，在`storage`上存储，可以直接修改
        - **当`key`不存在时，`value = type's default`，不会抛出异常**

3. 函数参数返回进阶
    - 命名参数返回
    - 命名返回参数直接赋值
    
4. 可视度（可视度与继承）
    - `public`：谁都可见
    - `external`：只有“外部调用”可见
    - `internal`：外部调用不可见，内部和子合约可见
    - `private`：只有当前合约可见

    状态变量：`public/internal/private`
    - 默认`internal`
    - `public`：自动定义取值函数（可以获取变量的值）
    - `private`：不代表别的人无法**肉眼看到**，只代表别的区块链智能合约无法看到
        > 在`ethereum`当中所有的数据都是公开的

    函数：`public/external/internal/private`
    - 默认`public`


5. 继承（可视度与继承）
    - 继承——基本语法：`contract A is B{}`
    - 继承——抽象合约：父类的`function`只有定义没有实现
    - 继承——`interface`：除了`function`定义，啥都没有
        ```
        interface Parent {
            function someFunc() returns (uint);
        }
        ```
    - 继承——多继承
        - 最平凡的多继承
        - 重名函数的`override`次序：继承顺位从后往前
        - `super`：动态绑定上级函数
        - 多继承`Method Resolution Order`使用`C3 Linearization`

6. `modifier`：类似`python`中的`decorator`
7. `SAFE MATH`和`LIBRARY`
    - `OpenZeppelin/openzeppelin-solidity`：很好的开源库
    
    导入库以便引用：
    - `import "./SafeMath.sol"`：在合约中`import SafeMath`以便调用
    - `a = SafeMath.sub(a, 100)`：`import SafeMath`之后的调用方式
    
    简化调用：
    - `using SafeMath for uint8;`：用于简化调用
    - `a = a.sub(100);`：使用`using`后，可将`a`传入`sub`作为第一个参数使用
    

### 设计
如何减少GAS的消耗？
- 使用`mapping`来替换数组的使用：**`mapping`可以实现精确查找，而不再需要循环处理（耗费`gas`）**


### 要点
1. `mapping`中只能使用`bool, int, address, string`这四种类型作为`key`
2. `mapping`是在`storage`中存储，在`EVM`中`storage`本身就是一个巨大的`key-value store`，用`keccak256hash`对`key`进行`hash function`可以理论上实现无限大的`hash`表：**由此带来`mapping`无法遍历**
3. `private`：不代表别的人无法**肉眼看到**，只代表别的区块链智能合约无法看到
    > 在`ethereum`当中所有的数据都是公开的

4. 函数的`external`可视度：告诉合约和以后的开发者，此函数不希望被其他内部函数所调用
    - 小技巧：可使用`this`关键字调用

5. 父类的构造函数在子类的构造函数调用之前会默认调用
    ```
    contract Child is parent {
        uint y;
        function Child(uint _y) Parent (_y * _y) { }
    }
    ```

6. `super`表示的上级函数并不一定是继承的类
    ```
    contract foundation {
        function func1() {....}
    }
    
    contract Base1 is foundation {
        function func1() {super.func1();}
    }
    
    contract Base2 is foundation {
        function func1() {super.func1();}
    }
    
    contract Final is Base1, Base2 {}
    
    contract test {
        Final f = new Final()
        f.func1();
    }
    
    ==============================================
    函数调用次序：Base2.func1 => Base1.func1 => foundation.func1
    ```
7. `modifier`函数中，`_`后的代码会最后执行，但是会在`return`之前就执行

### 小结
1. `MAPPING`
    - `mapping(address => Employee) employees`
    - `employees[key] = value`
2. 可视度
    - `public`
    - `external`
    - `internal`
    - `private`
    - 取值方程
3. 功能
    - 继承
    - `modifier`
    - `library`：`zeppelin`
4. 其他
    - 命名返回参数


### 作业
1. 完成今天所开发的合约产品化内容
2. 增加`changePaymentAddress()` 函数，更改员工的薪水支付地址，思考一下能否使用 modifier 整合某个功能
3. 加分题：自学`C3 Linearization`, 求以下`contract Z`的继承线
    ```
    contract O
    contract A is O
    contract B is O
    contract C is O
    contract K1 is A, B
    contract K2 is A, C
    contract Z is K1, K2
    ```


### 问题
#### 1. 产品化是什么意思？

答：将这个员工系统利用`solidity`更高级的语言特性来完善：可靠性、高效性、安全性

#### 2. `external`的存在意义是？

答：`external`在处理大数组的数据时比`public`效率更高，即消耗的`gas`更少。

一个简单的例子如下：
```
pragma solidity^0.4.12;

contract Test {
    function test(uint[20] a) public returns (uint){
         return a[10]*2;
    }

    function test2(uint[20] a) external returns (uint){
         return a[10]*2;
    }
}
```
每个`function`调用一遍，`public function`消耗了552`gas`，而`external function`消耗了317`gas`。

因为在`public function`中，`solidity`将数组参数复制到了`memory`；而`external function`则是直接从`calldata`中读取数据。`memory`分配相对`calldata`而言更贵。
[external vs public best practices](https://ethereum.stackexchange.com/questions/19380/external-vs-public-best-practices)

