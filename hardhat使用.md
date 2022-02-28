## Hardhat配置与相关问题：





###  目前问题

* !locked!                           等待用的时候解决    
*  abi 二进制接口的意义用法   ==已经解决==
*  事件的watch                     等待用的时候解决  



###  需要记得一些语法点

-   ...[..]  传入参数的时候进行解构赋值
- 



### 函数知识

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
  - 需要在合约初始化传入参数的时候，在部署合约的时候填入参数,需要参数的话在部署合约的时候传入。
  
    



### Hardat的运行

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









###Hardat的部署脚本基本代码



####  1.deploy.js



- 02.28

```js
//引入包
const { ethers, network } = require("hardhat");
const { writeAddr } = require('./artifact_log.js');

async function main() {
  
  //获取签名器
  let [owner] = await ethers.getSigners();
  console.log(owner.address)
  console.log(await owner.getAddress())

  let name = 'MyNft';
  let params = ['MyNft', "MN"];
  
  //获得合约工厂，包含各种方法
  const MyNft = await ethers.getContractFactory(name);
   
  //解构赋值，相当于拆包，传入俩个或者多个参数
  const Mynft = await MyNft.deploy(...params); 
  
  let   nft   = await Mynft.deployed();          
  await writeAddr(nft.address, "MyNft");
  //console.log(`${name} deployed to:      ${contract.address.toLowerCase()}`);
  //tolowercase是啥
  console.log(`${name} address:`,Mynft.address);
  console.log("-------------------------------------------------");
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });


```





####   2.需要引入的artifact_log.js



- 02.28

```js
const { network } = require("hardhat");

const fs = require('fs');
const path = require('path');
const util = require('util');

const writeFile = util.promisify(fs.writeFile);
/**
 * 记录合约发布地址
 * @param {*} deployments json
 * @param {*} name 类型
 */
async function writeAddr(addr, name){

  const chainid = network.config.chainId;
  const saveDir = path.resolve(__dirname, `../deployments/${chainid}/`);

  if (!fs.existsSync(saveDir)) {
    fs.mkdirSync(saveDir);
  }
  
  const deploymentPath = saveDir + `/${name}.json`; 

  const deployments = {};
  deployments["address"] = addr;

  await writeFile(deploymentPath, JSON.stringify(deployments, null, 2));
  console.log(`Exported deployments into ${deploymentPath}`);

}

module.exports = {

  writeAddr
  
}
```













































