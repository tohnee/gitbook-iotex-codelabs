---
description: 教程一：基于IoTeX轻松构建剪刀石头布的Dapp
---

# gitbook-iotex-codelabs

## 1.总览

 本教程讲述了使用Antenna SDK构建一个简单的全栈的rock-paper-scissors DApp的例子。我们将带领大家逐步完成整个过程：首先构建智能合约的后端，然后使用React构建基于Antenna SDK的前端。

 最终结果如下图所示：

![](https://uploader.shimo.im/f/z3vsdbYQygs8p4NL.png!thumbnail)

 首先介绍一下什么是Antenna SDK。Antenna SDK使用了gRPC或者gRPC-web的连接，从而保障了用户可以与本地或者远程的区块链节点进行交互。允许用户与区块链进行智能合约部署和数据获取的交互。

 通过这个教程你可以学习到：

1. 如何构建一个RPS的智能合约后端
2. 如何使用Antenna SDK的前端
3. 如何连接前端与后端构建功能完备的dapp
4. 如何使用IoTeX Desktop Wallet

 在学习教程前，你需要准备的是：

1. 一台电脑（of course）
2. Nodejs/npm 安装完成
3. 一个iotex 账户
4. 一个IOTX（IoTeX代币，别急我会告诉你如何获取）

 如果你是新手，还没有听说过智能合约。可以先食用[deploying a simple smart contract](https://codelabs.iotex.io/codelabs/deploy-smart-contract/index.html?index=..%2F..index#0) 

 这个教程在IoTeX主网和测试网均可使用，如果你现在还没有IOTX，可以将此智能合约部署在测试网上，或者在上[faucet](https://member.iotex.io/faucet/)获取测试用的IOTX。

1. 游戏设计

 我们需要设计一个对战机器人。让我们想一想这个对战机器人应该具备的功能，这个对战机器人需要和所有登陆Dapp的玩家玩剪刀石头布的游戏，用户需要质押一枚IOTX作为赌注，并设定自己的手型，选择剪刀、石头或者布。机器人会生成一个随机的手型，然后和用户的加以比较，判定正、负、平。

 所以一共会有三种结果出现（以用户角度）：

* 平局：机器人会返还给用户一枚IOTX
* 胜利：机器人会返还给用户两枚IOTX
* 输了：机器人不会返还给用户IOTX

1. 准备开始

### React Setup/React建立

 因为将要用到React构建我们的Dapp，我们会使用Create-React-App去引导我们Dapp，我们把这个项目叫做rps（rock-paper-scissors）。

 如果是不熟悉React的小伙伴也不必担忧，你仍然可以依据此教程去学习基本的代码使用方式。其实项目的前端可以使用任意的framework，只是在这里我们使用了react来完成这个demo。

![](https://uploader.shimo.im/f/GY4rtgqOwYoVP44F.png!thumbnail)

 使用这条命令可以自动构建react app，并安装一些webpack和babel用于我们的简易开发。接下来创建一个文件夹存放智能合约。

![](https://uploader.shimo.im/f/br7spFFzww00ZnIU.png!thumbnail)

### Install node modules/安装node模块

 我们将会使用iotex-antenna sdk来和智能合约交互。

在rps文件夹里输入如下命令。

![](https://uploader.shimo.im/f/g3vAdGpQMTs0js96.png!thumbnail)

安装sleep-promise，用来等待下一个区块。

![](https://uploader.shimo.im/f/KCTmq6vLttg6j3Vg.png!thumbnail)

安装react-bootstrap，用来美化我们的Dapp。

![](https://uploader.shimo.im/f/weabIOBB2KAvMYnV.png!thumbnail)

### Installing IoPay-Desktop/安装IoPAY客户端

 我们将会使用IoPAY-Desktop来处理账户授权当我们需要和智能合约发生交互时。在这里[here](https://github.com/iotexproject/iotex-explorer/releases)下载IoPAY-Desktop。

1. 构建智能合约

 我们需要为我们的剪刀石头布游戏创建一个后端，也就是一个运行在区块链上的智能合约。在这个教程里我们假设你已经对Solidity有了一定的基础。创建一个 rps.sol的文件，具体代码可以参考下面，我们建议您认真阅读理解。

