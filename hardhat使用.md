//学习中的问题：

// !locked!
// abi 二进制接口的意义用法
// 事件的watch


1.const [owner] = await ethers.getSigners()  //返回20个账户
thers.js中的Signer 代表以太坊账户对象。 它用于将交易发送到合约和其他帐户。 
在这里，我们获得了所连接节点中的帐户列表，在本例中节点为Hardhat Network，
并且仅保留第一个帐户
2.const Token = await ethers.getContractFactory("Token");   //Token相当于合约本体
ethers.js中的ContractFactory是用于部署新智能合约的抽象，
因此此处的Token是用来实例代币合约的工厂。
3.const hardhatToken = await Token.deploy();               //合约部署完，harddatToken就是合约的方法集合体
在ContractFactory上调用deploy()将启动部署，并返回解析为Contract的Promise。 
该对象包含了智能合约所有函数的方法。


1.运行节点
npx hardhat node  

2.部署脚本（理解为部署合约），在这种情况下，如果不使用--network 
参数来运行它，则代码将再次部署在Hardhat network 上，因此，
当Hardhat network 关闭后，部署实际上会丢失，但是它用来测试
我们的部署代码时仍然有用

npx hardhat run scripts/deploy.js --network <network-name>

要部署到诸如主网或任何测试网之类的线上网络，你需要在hardhat.config.js 
文件中添加一个network条目


3.hardhat节点关闭，本地部署的也消失