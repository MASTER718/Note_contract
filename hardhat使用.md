## 学习中的问题：

###  目前问题
* !locked!
*  abi 二进制接口的意义用法   大概解决
*  事件的watch



####  函数知识
02.23
-  1.  const [owner,addr1,addr2...] = await ethers.getSigners()   ---------  //这个函数是返回20个账户
    ethers.js中的Signer 代表以太坊账户对象。 它用于将交易发送到合约和其他帐户。 
     在这里，我们获得了所连接节点中的帐户列表，在本例中节点为Hardhat Network，
     并且仅保留第一、二、三个帐户
- 2. 
      ```js 
           Token = await ethers.getContractFactory("Token");    //Token相当于合约本体
           const hardhatToken = await Token.deploy();          //合约部署完，harddatToken就是合约的方法集合体
        
            //解释 ethers.js中的ContractFactory是用于部署新智能合约的抽象，
            //因此此处的Token是用来实例代币合约的工厂。
            // 在ContractFactory上调用deploy()将启动部署，并返回解析为Contract的Promise。 
       
      ```
---
02.24
- solidity的一些注意：
  - constructor{}构造函数只执行一次，不需要写入public修饰


- 部署合约：
  - 需要在合约初始化传入参数的时候，在部署合约的时候填入参数
  - 








####  hardhat网络的使用
- 1 ：运行节点 ：
    ```js
    npx hardhat node
    ```
- 2 ：部署脚本（理解为部署合约），在这种情况下，如果不使用--network 
  参数来运行它，则代码将再次部署在Hardhat network 上，因此，
  当Hardhat network 关闭后，部署实际上会丢失，但是它用来测试
  我们的部署代码时仍然有用,要部署到诸如主网或任何测试网之类的线上网络，你需要在hardhat.config.js 文件中添加一个network条目
    ```js
    npx hardhat run scripts/deploy.js --network <network-name>
    ```
- 3 : hardhat节点关闭，本地部署的也消失
