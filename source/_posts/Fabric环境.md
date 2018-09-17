---
title: Mac下搭建hyperledger Fabric环境
date: 2018-09-06 15:42:48
tags: Fabric环境搭建
categories: 区块链
---

> 童年，我们总以为什么都不懂；少年，我们总以为自己什么都懂；青年，我们又以为什么都不懂；中年，我们又以为什么都懂；老年，其实我们什么都不懂。此人生五境界论，揭示了人生的秘密：我们在生活面前，永远都是无知的孩子。


----

ubuntu参考[区块链之Hyperledger（超级账本）Fabric v1.0 的环境搭建（超详细教程）](https://blog.csdn.net/so5418418/article/details/78355868/)

本人是mac OS 系统，参考[Mac下Hyperledger Fabric(超级账本)环境搭建](https://www.jianshu.com/p/e108cf655c0f),这篇文章作者已经写的很详细了，但是在操作过程中我还是遇到了困难，搞了很久，最后在朋友的帮助下得以解决，所以在此记录下来，以方便遇到同样问题的朋友参考。

<!--more--->


### 1、前提条件

##### 1.1 安装 Go 
Mac 可以直接通过 `homebrew` 安装 go 环境

```
brew install go
```

或者通过官网下载安装包[传送门](https://golang.org/dl/)

安装完成后检测是否安装成功

```
$ go version
go version go1.11 darwin/amd64
```

##### 1.2 配置环境变量

**a:** 新建一个go的文件夹，路径放在$HOME下：

```
$ cd $HOME	//进入 $HOME 目录下 
$ mkdir go	//新建 go 文件夹
```

**b:** 配置环境变量

```
$ cd $HOME			//进入 $HOME 目录下 
$ vi .bash_profile	//编辑 .bash_profile 文件
```

输入 `i` ,然后在文件末尾加上：

```
#GOPATH
export GOPATH=$HOME/go

#GOBIN
export GOBIN=$GOPATH/bin
export PATH=$PATH:$GOBIN
```

按下 `esc` 键，输入 `:wq` 保存后退出，执行 `source` 命令，使之生效：

```
$ source .bash_profile
```

**c:** 通过 `go env` 检查：

```
$ go env

#输出结果
GOARCH="amd64"
GOBIN="/Users/wasai/go/bin"
GOCACHE="/Users/wasai/Library/Caches/go-build"
GOEXE=""
GOFLAGS=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH="/Users/wasai/go"
.	
.此处截取一部分，省略很多
.
```

##### 1.3 安装Docker并更新镜像源

**a** 安装Docker和docker-compose

在官网下载[Docker CE for Mac](https://store.docker.com/editions/community/docker-ce-desktop-mac)（需要注册dockerId才能下载），完成后安装。

安装后检查docker和docker-compose是否安装成功

```
$ docker --version
Docker version 18.06.1-ce, build e68fc7a
$ docker-compose --version
docker-compose version 1.22.0, build f46880f
```

**b** 更新镜像源


打开刚刚安装的Docker软件，点击导航栏上的Docker图标->Preference->Daemon,在`Registry mirrors`中加入`https://aic2v8yz.mirror.aliyuncs.com`

ps：这里使用上述参考文章作者的加速器,如果你自己有也可以用自己的加速器

完成之后，最后确保docker的状态是`Docker is running`并且在运行Hyperledger的时候，Docker的状态也是`Docker is running`

### 2、下载Hyperledger Fabric项目源码

为 fabric 创建项目路径：

```
$ cd $GOPATH									
$ mkdir -p src/github.com/hyperledger
$ cd src/github.com/hyperledger
$ pwd		
/Users/wasai/go/src/github.com/hyperledger
```

从git上拉取[Hyperledger Fabric](https://github.com/hyperledger/fabric)：

```
$ git clone git@github.com:hyperledger/fabric.git
$ ls
fabric
```

进入项目文件夹，查看tag：

```
$ cd fabric
$ git tag
baseimage-v0.0.11
v0.6.0-preview
v0.6.1-preview
v1.0.0
v1.0.0-alpha
v1.0.0-alpha2
v1.0.0-beta
v1.0.0-rc1
v1.0.1
v1.0.2
v1.0.3
v1.0.4
v1.0.5
v1.0.6
v1.1.0
v1.1.0-alpha
v1.1.0-preview
v1.1.0-rc1
v1.1.1
v1.2.0
v1.2.0-rc1
```

选择使用v1.0.0

```
$ git checkout v1.0.0
$ git branch
* (HEAD detached at v1.0.0)
  release-1.2
```

### 3、启动项目

进入项目文件夹

```
$ cd examples/e2e_cli
```

执行download-dockerimages.sh，程序将会通过docker拉取项目所需镜像, 为了统一版本，请指定拉取镜像的版本号:

```
$ chmod +x download-dockerimages.sh
$ ./download-dockerimages.sh -c x86_64-1.0.0 -f x86_64-1.0.0
```

镜像拉取完成：

```
===> List out hyperledger docker images
hyperledger/fabric-tools       latest              0403fd1c72c7        14 months ago       1.32GB
hyperledger/fabric-tools       x86_64-1.0.0        0403fd1c72c7        14 months ago       1.32GB
hyperledger/fabric-couchdb     latest              2fbdbf3ab945        14 months ago       1.48GB
hyperledger/fabric-couchdb     x86_64-1.0.0        2fbdbf3ab945        14 months ago       1.48GB
hyperledger/fabric-kafka       latest              dbd3f94de4b5        14 months ago       1.3GB
hyperledger/fabric-kafka       x86_64-1.0.0        dbd3f94de4b5        14 months ago       1.3GB
hyperledger/fabric-zookeeper   latest              e545dbf1c6af        14 months ago       1.31GB
hyperledger/fabric-zookeeper   x86_64-1.0.0        e545dbf1c6af        14 months ago       1.31GB
hyperledger/fabric-orderer     latest              e317ca5638ba        14 months ago       179MB
hyperledger/fabric-orderer     x86_64-1.0.0        e317ca5638ba        14 months ago       179MB
hyperledger/fabric-peer        latest              6830dcd7b9b5        14 months ago       182MB
hyperledger/fabric-peer        x86_64-1.0.0        6830dcd7b9b5        14 months ago       182MB
hyperledger/fabric-javaenv     latest              8948126f0935        14 months ago       1.42GB
hyperledger/fabric-javaenv     x86_64-1.0.0        8948126f0935        14 months ago       1.42GB
hyperledger/fabric-ccenv       latest              7182c260a5ca        14 months ago       1.29GB
hyperledger/fabric-ccenv       x86_64-1.0.0        7182c260a5ca        14 months ago       1.29GB
hyperledger/fabric-ca          latest              a15c59ecda5b        14 months ago       238MB
hyperledger/fabric-ca          x86_64-1.0.0        a15c59ecda5b        14 months ago       238MB
hyperledger/fabric-baseos      x86_64-0.3.1        4b0cab202084        16 months ago       157MB
```

最后执行脚本

```
$ ./network_setup.sh up <channel-ID>
```

如果没有`channel-ID`,默认为`mychannel`，执行命令：

```
$ ./network_setup.sh up mychannel
```

我执行完这个命令失败了，出现以下结果：

```
Error: Error endorsing chaincode: rpc error: code = Unknown desc = Error starting container: API error (404): {"message":"network e2ecli_default not found"}

Usage:
  peer chaincode instantiate [flags]

Flags:
  -C, --channelID string   The channel on which this command should be executed (default "testchainid")
  -c, --ctor string        Constructor message for the chaincode in JSON format (default "{}")
  -E, --escc string        The name of the endorsement system chaincode to be used for this chaincode
  -l, --lang string        Language the chaincode is written in (default "golang")
  -n, --name string        Name of the chaincode
  -P, --policy string      The endorsement policy associated to this chaincode
  -v, --version string     Version of the chaincode specified in install/instantiate/upgrade commands
  -V, --vscc string        The name of the verification system chaincode to be used for this chaincode

Global Flags:
      --cafile string              Path to file containing PEM-encoded trusted certificate(s) for the ordering endpoint
      --logging-level string       Default logging level and overrides, see core.yaml for full syntax
  -o, --orderer string             Ordering service endpoint
      --test.coverprofile string   Done (default "coverage.cov")
      --tls                        Use TLS when communicating with the orderer endpoint

!!!!!!!!!!!!!!! Chaincode instantiation on PEER2 on channel 'mychannel' failed !!!!!!!!!!!!!!!!
================== ERROR !!! FAILED to execute End-2-End Scenario ==================

```

`ctrl+c`退出

**解决办法：** 找到本地文件夹进入项目：`/fabric/examples/e2e_cli/base/peer-base.yaml`，打开 `peer-base.yaml`文件

```
# Copyright IBM Corp. All Rights Reserved.
#
# SPDX-License-Identifier: Apache-2.0
#

version: '2'
services:
  peer-base:
    image: hyperledger/fabric-peer
    environment:
      - CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
      # the following setting starts chaincode containers on the same
      # bridge network as the peers
      # https://docs.docker.com/compose/networking/
      - CORE_VM_DOCKER_HOSTCONFIG_NETWORKMODE=e2ecli_default
      #- CORE_LOGGING_LEVEL=ERROR
      - CORE_LOGGING_LEVEL=DEBUG
      - CORE_PEER_TLS_ENABLED=true
      - CORE_PEER_GOSSIP_USELEADERELECTION=true
      - CORE_PEER_GOSSIP_ORGLEADER=false
      - CORE_PEER_PROFILE_ENABLED=true
      - CORE_PEER_TLS_CERT_FILE=/etc/hyperledger/fabric/tls/server.crt
      - CORE_PEER_TLS_KEY_FILE=/etc/hyperledger/fabric/tls/server.key
      - CORE_PEER_TLS_ROOTCERT_FILE=/etc/hyperledger/fabric/tls/ca.crt
    working_dir: /opt/gopath/src/github.com/hyperledger/fabric/peer
    command: peer node start
```

其中将`- CORE_VM_DOCKER_HOSTCONFIG_NETWORKMODE=e2ecli_default`改为`- CORE_VM_DOCKER_HOSTCONFIG_NETWORKMODE=e2e_cli_default`

然后执行命令（如果不执行这个，会出现新的问题）

```
# 在e2e_cli目录下
$ ./network_setup.sh down
```

重新启动Docker（如果不重启，会出现新的问题）

再重新执行命令

```
$ ./network_setup.sh up mychannel
```

成功后输出

```
===================== Query on PEER3 on channel 'mychannel' is successful =====================

===================== All GOOD, End-2-End execution completed =====================


 _____   _   _   ____            _____   ____    _____
| ____| | \ | | |  _ \          | ____| |___ \  | ____|
|  _|   |  \| | | | | |  _____  |  _|     __) | |  _|
| |___  | |\  | | |_| | |_____| | |___   / __/  | |___
|_____| |_| \_| |____/          |_____| |_____| |_____|


```

`ctrl+c`退出

### 4、测试Fabric网络

这里有官方提供的小例子，在官方例子中，channel名字是mychannel，链码的名字是mycc。 
首先进入CLI，然后重新打开一个命令行窗口，输入： 

```
$ docker exec -it cli bash
```

然后运行以下命令可以查询a账户的余额： 

```
peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'
```

输出

```
root@e484bcb8e6da:/opt/gopath/src/github.com/hyperledger/fabric/peer# peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'
2018-09-06 10:11:25.945 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-09-06 10:11:25.945 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-09-06 10:11:25.945 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-09-06 10:11:25.945 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-09-06 10:11:25.945 UTC [msp/identity] Sign -> DEBU 005 Sign: plaintext: 0A95070A6708031A0C08CDFAC3DC0510...6D7963631A0A0A0571756572790A0161 
2018-09-06 10:11:25.946 UTC [msp/identity] Sign -> DEBU 006 Sign: digest: A7714812CFA5315FB0649B429267ED7DDE45CD1C082A55D2C03A09F3ACF2DE72 
Query Result: 90
2018-09-06 10:11:25.963 UTC [main] main -> INFO 007 Exiting.....
root@e484bcb8e6da:/opt/gopath/src/github.com/hyperledger/fabric/peer# 
```

`Query Result: 90` 结果余额90

下面可以进行转账`invoke`操作,a转给b 50：

```
peer chaincode invoke -o orderer.example.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n mycc -c '{"Args":["invoke","a","b","50"]}'
```

现在转账完毕， 我们试一试再查询一下a账户的余额，重复之前的查询指令，结果为： 

```
root@e484bcb8e6da:/opt/gopath/src/github.com/hyperledger/fabric/peer# peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'
2018-09-06 10:14:42.158 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-09-06 10:14:42.159 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-09-06 10:14:42.159 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-09-06 10:14:42.160 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-09-06 10:14:42.162 UTC [msp/identity] Sign -> DEBU 005 Sign: plaintext: 0A94070A6608031A0B0892FCC3DC0510...6D7963631A0A0A0571756572790A0161 
2018-09-06 10:14:42.162 UTC [msp/identity] Sign -> DEBU 006 Sign: digest: C6521250C0ACBF25D2B862D61ADF60EEC64015A1362CB091E9C3D5DAC8C2AA43 
Query Result: 40
2018-09-06 10:14:42.183 UTC [main] main -> INFO 007 Exiting.....
root@e484bcb8e6da:/opt/gopath/src/github.com/hyperledger/fabric/peer# 
```

`Query Result: 40` 结果剩余40了，正确。

最后，我们需要关闭Fabric，这里先使用exit命令退出cli容器。然后执行命令

```
$ ./network_setup.sh down
```

搭建并且测试结束！

---

*到此结束，一把辛酸泪，好记性不如烂笔头。*