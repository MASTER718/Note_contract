#  ether.js的使用方法
## 注意：
     - 读区块链数据不需要消耗gas
     - 写区块链数据需要连接账户且消耗gas


this.erc20_contarct = new ethers.Contract(addr.address, abi, new ethers.providers.Web3Provider(this.provider).getSigner())





###   02.21学习：
*  网络节点
   - provider：相当于连接以太坊网络的一个节点，用与查询以太坊网络状态或者发送更改状态的交易
   - 得到节点方法：
      - this.provider = window.ethereum;   ？方法哪里来的  -> 从前端的属性中得到 //得到网络节点
   - 签名器： this.signer = new ethers.providers.Web3Provider(this.provider).getSigner()   //连接metamask的节点 //连接当前签名器   **对区块链进行写操作时需要连接钱包签名**

<br/>

###    02.22学习：
