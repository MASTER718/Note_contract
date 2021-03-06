

## Hardhat配置与相关问题：



###   1.目前问题

* !locked!                           等待用的时候解决    
*  abi 二进制接口的意义用法   ==已经解决==
*  事件的watch                     等待用的时候解决  
*  ethers.utils                      ==已经解决==  -- ethers里的一个工具包，详细看手册
*  nodejs中的util                         待解决



###  2.需要记得一些语法点

-   ...[..]  传入参数的时候进行解构赋值
- java中都是对象，错的语法可以从这个角度思考
- js只有一个执行线程
- nodejs中_dirname总是指向被执行js文件的绝对路径，引入文件是路径为相对路径



### 3.函数知识

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
  
    



### 4.Hardat的运行

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





###5.Hardat的部署脚本基本代码(重要)



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
  deployments["address"] = addr;    //添加键值对
  //下面将这个对象转换为json字符串存储起来，便于读取

 //json.stringify()用法见下
  await writeFile(deploymentPath, JSON.stringify(deployments, null, 2));
  console.log(`Exported deployments into ${deploymentPath}`);

}

module.exports = {

  writeAddr
  
} 
```



#####   1.fs、path、util的部分用法解释

-  fs 的部分用法

  ​     -----标准的异步读法,==内含回调函数==-----

  - fs.appendFile   ----创建文件

  - fs.readFile(....<表示文件路径和名称>，<回调函数>(err,data)=>{   })    ----- 参考用法

  - fs.unlink          ----删除文件

  - fs.mkdir          ----创建文件夹

  - fs.readir         ----读取目录下的文件及文件夹

  - fs.rmdir          ----删除空目录

     ------同步读法，不含回调函数-----
  
  - 加入Sync后缀



- path的涉及函数

  - fs.resolve([from...],to)

    ```js
    //从源地址到目的地址的绝对路径，类似在shell中执行一系列的cd命令，就是得到一个绝对路径
    --例子1
    path.resolve('foo/bar', '/tmp/file/', '..', 'a/../subfile')
    类似于：
    cd foo/bar
    cd /tmp/file/
    cd ..
    cd a/../subfile
    pwd
    
    path.resolve(".")   //'.'或者'./'令是对于自己的工作目录，也就是node命令的目录
    ```



- util的涉及函数解释
  - 





-  在读取二进制文件的时候，==Node.js中，Buffer对象就是一个包括一个零个或任意个字节的数组。==

  - buffer =>string   

  - string =>buffer  

      ```js
      //buffer => string
      //data - 字符串
      var text = data.toString('utf-8')
      
      //string => buffer  
      //buf1是字符串
      var buf  = Buffer.from(buf1,'utf-8')
      
      
      ```

​       

#####   2.json.stringify()的用法  —— 输出json字符串

- json.stringify()包含三个参数
  - 第一个是json对象，表示将它转换为字符串输出
  - 第二个为[]或者单个key([]里面是多个key)，表示输出我们想要的东西，相当于筛选条件
    - 如果第二个是函数，执行即可
  - 第三个参数为字符串或数值时，字符串会以该字符向前填充





#  ether.js/web3.js的使用方法

## 注意：

     - 读区块链数据不需要消耗gas
     - 写区块链数据需要连接账户且消耗gas

this.erc20_contarct = new ethers.Contract(addr.address, abi, new ethers.providers.Web3Provider(this.provider).getSigner())



##  签名器和钱包 （2-4）



####    1. 钱包与签名器与节点

+ 钱包的属性

  -  .address    ......   //地址、助记词、签名方法等
  -  各种方法     与链交互

+ 签名器：wallet就是签名器的一个继承实

  > 使用方法
  > -1   object . getAddress ( )  =>  Promise<Address>     返回一个可获取账号的Promise
  >
  > -2  还有发送交易和gas的其他方法

+ Provider

  - Provider是一个连接以太网络的抽象

  - >-- 连接已有的web3
    >
    >```js
    >使用web3Provider时，自动检测网络
    >let  currentProvider = new web3.providers.HttpProvider('http://localhost:8545');
    >let web3Provider = new ethers.providers.Web3Provider(currentProvider);
    >```
    >
    >-- Provider 的属性
    >
    >具体手册，获取网络状态和以太坊状态等......
    >
    >

     

####  2.代码形式Study：



```js
wallet 实现了Signer API 需要签名签名器可以使用Wallet，包含了签名器的所有属性

---案例1
<!---
new Wallet ( privateKey [ , provider ] )
从参数 privateKey 私钥创建一个钱包实例， 还可以提供一个可选的 提供者Provider 参数用于连接节点。
--->
//利用私钥产生一个钱包
let privateKey = "0x0123456789012345678901234567890123456789012345678901234567890123";
let wallet = new ethers.Wallet(privateKey);
 //连接钱包到主网
let provider = ethers.getDefaultProvider();
let walletWithProvider = new ethers.Wallet(privateKey, provider);


---案例2
<!--
使用节点和signer的实例代码
-->

-- 以部署合约为例

前端交互
this.provider = window.ethereum;                 // 得到当前的Provider
this.accounts = await window.ethereum.enable()  //  获取
this.signer = new ethers.providers.Web3Provider(this.provider).getSigner() //获取当前用户

----得到合约的addr 与abi
const addr = require(`../../deployments/${this.chainId}/${ContractName}.json`);
const abi = require(`../../deployments/abi/${ContractName}.json`);

```

一些注释：

```js
ES6js引入ethers
import {ethers}  from 'ethers';

this.provider = window.ethereum                              //获取当前网络节点
this.signer = new ethers.providers.Web3Provider(this.provider).getSigner()   //连接metamask的节点 //连接当前签名器   对区块链进行写操作时需要连接钱包签名

```



#### 3. 网络节点

- provider：相当于连接以太坊网络的一个节点，用与查询以太坊网络状态或者发送更改状态的交易
- 得到节点方法：
  -    this.provider = window.ethereum;   ？方法哪里来的  -> 从前端的属性中得到 //得到网络节点
- 签名器： this.signer = new ethers.providers.Web3Provider(this.provider).getSigner()   //连接metamask的节点 //连接当前签名器   **对区块链进行写操作时需要连接钱包签名**

































































