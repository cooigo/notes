# 以太坊开发

## 测试币申请

<https://steemit.com/ethereum/@ericfish/3qraaz>

<http://faucet.ropsten.be:3001/>

## 使用 web3j 构建以太坊钱包

- web3j 官网

  <https://web3j.io/>

- 以太币转账

  <https://www.jianshu.com/p/1b716180bc4b>

  <https://www.cnblogs.com/hongpxiaozhu/p/8574986.html>

- 智能合约转账

  <https://www.codetd.com/article/118043>

  <https://blog.csdn.net/u010123087/article/details/79637260>

  <https://github.com/ethjava/web3j-sample/blob/master/src/main/java/com/ethjava/TokenClient.java>

- 建立代币

  <https://learnblockchain.cn/2018/01/12/create_token/>

  <http://ethbtc.org/news/105-cn.html>

- 创建合约工具

  <https://remix.ethereum.org>

- 去中心化应用

  <https://learnblockchain.cn/2018/01/12/first-dapp/>

- 智能合约创建工具 <https://truffleframework.com/docs> <https://www.jianshu.com/p/683ea7d62a39>

## 申请免费测试节点

<https://infura.io/setup?key=qQ7KN5WY1fryIXkIuJxv>

## 查看测试结果

<https://ropsten.etherscan.io/>

## 多任务

<https://my.oschina.net/jack90john/blog/1506474>

## ETH Gas 查询

<https://ethgasstation.info/index.php>

## web.js 发币

- web3.js api 文档 <https://github.com/ethereum/wiki/wiki/JavaScript-API>

- <https://github.com/MikeMcl/bignumber.js/>

- web3.js 编译 Solidity，发布，调用全部流程（手把手教程）<http://web3.tryblockchain.org/web3-js-in-action.html>

- ERC-20 标准

  <https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md>

  中文 <https://blog.csdn.net/diandianxiyu_geek/article/details/78082551>

- 教你分分钟实现 imtoken 添加即空投代币 <http://blog.2liang.me/2018/02/25/erc-20-add-wallet-airdrop-md/>

- 如何使用 web3 部署以太坊智能合约 <https://segmentfault.com/a/1190000013841167>

- angular6 问题  `Can't resolve 'crypto'`

  package.json

  ```json 
  {
    "scripts": {
      "postinstall": "node patch.js",
      
    }
  }
  ```

  patch.js

  ```javascript 
  const fs = require('fs');
  const f = 'node_modules/@angular-devkit/build-angular/src/angular-cli-files/models/webpack-configs/browser.js';

  fs.readFile(f, 'utf8', function (err,data) {
    if (err) {
      return console.log(err);
    }
    var result = data.replace(/node: false/g, 'node: {crypto: true, stream: true}');

    fs.writeFile(f, result, 'utf8', function (err) {
      if (err) return console.log(err);
    });
  });

  ```
