## erc1820

- solidity 

  - soliidity 0.6.8 引入了SPDX，使用时要在文件第一句加上SPDX-License-Identifier: <> 格式的语句。
  - SPDX许可列表网址：https://spdx.org/licenses/

-  this关键字

  - 合约内部可以使用this作为关键字，address（this）可以转换地址类型

    type（c）获得合约的类型信息

-  可见性：

  - interal - 函数和状态变量在==当前合约和继承的合约==中使用
  - private- 函数和转态变量仅在==当前定义他们的合约==中使用
  
- ######  ==函数选择器==

  - 调用函数时，使用前面四个字节的函数选择器来指定调用的函数，==函数选择器是函数签名的Keccak哈希的前四个字节==

    ```js
    bytes4(keccak256("count()")
    ```

- ######  ==函数签名==

  - 包含函数名与参数类型的字符串，例如上面中的“count()” ，具体查看P180

-  