摘自：https://twitter.com/0xFatDog/status/1730189598855438721。 感谢@0xFatDog，感兴趣的朋友请关注其twitter


  
-------------------------------
0.  打开bitcoin core 软件
1. 下载并安装磁力链接下载工具qbittorrent https://pc.qq.com/detail/12/detail_23192.html
2. 打开qbittorrent，左上角从url下载，然后复制这个磁力链接进去 magnet:?xt=urn:btih:7KW5OXSWUQ2EFF57URE42GBRL2XCN5AI&dn=ElectrumX-Data-20231114 下载数据（需要十小时左右）
3. 在硬盘上（需大于100g空闲容量）新建文件夹 atomicals-rpc ,在rpc下新建文件夹electrumx-data下载好了以后解压electrumx-data该文件夹，解压后原压缩包别删先留着
4.  在你的电脑开启虚拟化：控制面板-程序和功能-开启Hyper-V，下面的两个全部勾选，还有一个适用于linux的windows子系统也要勾选，勾选完成后会重启电脑 （部分cpu可能还需要从bios中开启虚拟化，具体可百度关键词：你的主板型号+开启虚拟化，比如微星开启虚拟化）
5.  5. 安装wsl：管理员模式打开powshell，执行wsl --install -d Ubuntu
6. 下载并安装docker https://docker.com/products/docker-desktop ，安装过程中勾选“Use WSL 2 instead of Hyper-V(recommended)”，安装完以后需要注册登录，也可能需要重启电脑
7. 用powshell 执行ipconfig，获取ipv4地址，并记录下来
8. 打开https://github.com/Next-DAO/atomicals-electrumx-proxy-docker/tree/main，后续步骤将按照该文档的指引
9.  打开本地btc全节点的配置文件，追加写入如下代码
rpcauth=nextdao:cca838b4b19bdc6093f4e0312550361c$213834a29e8488804946c196781059a7ee0ac2b48dbf896b4c6852060d9d83dd
rpcallowip=127.0.0.1
rpcallowip=172.0.0.0/8
rpcallowip=192.168.0.0/16
rpcbind=0.0.0.0

10.  打开刚才新建的atomicals-rpc文件夹，从该地址下载docker-compose.yml的文件http://raw.githubusercontent.com/Next-DAO/atomicals-electrumx-proxy-docker/main/docker-compose.yml，然后把倒数第五行中${IP:?}要改为你自己的ipv4地址。
11.  在atomicals-rpc文件夹中打开powershell，依次执行 docker-compose pull 和 docker-compose up -d ，执行完成后你会发现你的docker中多了个container
12.  等待同步数据（三四个小时），可在electrumx-1右边action中的view details中查看日志
13.  等到http://localhost:8080/proxy/health 中的出现success: true, health:true ，两个true以后才算成功


   
---------
本文写的比较技术，转GPT对该文的解释：


文章的逻辑是关于在 Windows 系统下安装和配置一个私有的 RPC（远程过程调用）节点，用于连接到比特币网络。这里是文章的逻辑梳理：

1. **准备工作**：
   - 打开 Bitcoin Core 软件：Bitcoin Core 是一个完整的比特币节点软件，用于维护比特币区块链的完整副本。
   - 安装 qbittorrent：这是一个下载工具，用于下载 ElectrumX 所需的数据。

2. **下载 ElectrumX 数据**：
   - 通过 qbittorrent 下载 ElectrumX 的数据。这些数据大概需要 100GB 的存储空间。

3. **开启虚拟化**：
   - 在 Windows 中启用 Hyper-V 和适用于 Linux 的 Windows 子系统（WSL）。这些是运行虚拟化软件所必需的。

4. **安装 WSL 和 Ubuntu**：
   - 通过 PowerShell 安装 WSL，并选择 Ubuntu 作为子系统的发行版。

5. **安装 Docker**：
   - 下载并安装 Docker。Docker 是一个开发平台，用于构建、运行和分享应用程序。在这种情况下，它用于容器化 ElectrumX 服务器。

6. **配置比特币全节点**：
   - 修改比特币全节点的配置文件以允许 RPC 通信。这一步是为了使 ElectrumX 服务器能够与比特币全节点通信。

7. **配置和运行 ElectrumX 服务器**：
   - 从 GitHub 下载所需的 Docker 配置文件（docker-compose.yml）并进行相应的修改。
   - 在 Docker 中运行 ElectrumX 服务器容器。

8. **等待数据同步**：
   - 运行 ElectrumX 容器，并等待数据同步完成。同步完成后，您的 ElectrumX 服务器应该能够连接到比特币网络并开始处理请求。

总结来说，文章的目的是指导用户在 Windows 系统下通过使用 Docker 和 WSL 设置一个 ElectrumX 服务器。这样做的优点是方便管理和维护，同时利用 Docker 可以确保 ElectrumX 的安装和配置过程更加标准化和简化。这个服务器将作为一个私有的 RPC 节点，能够处理比特币网络的请求。

问：这个在alchemy上调用的以太坊节点rpc的原理类似吗？首先alchemy、infura上都有rpc服务，这跟普通开发者调用的关系是什么，介绍一下；其次，如上的电脑上有比特币全节点，有electronx服务器，成为了一个rpc，是不是类似可以想alchemy一样提供以太坊节点服务了？

是的，Alchemy 或 Infura 提供的以太坊节点 RPC (Remote Procedure Call) 服务的原理与个人设置比特币全节点和 ElectrumX 服务器提供 RPC 服务的原理类似，但也有一些关键区别。让我们分两部分来讨论：

### 1. Alchemy/Infura 的以太坊节点 RPC 服务

- **服务概述**：
  - Alchemy 和 Infura 是提供托管的以太坊节点服务的平台。这些服务允许开发者通过 RPC 接口与以太坊区块链进行交互，而无需自己维护和运行一个完整的节点。
  
- **开发者的使用**：
  - 对于开发者来说，使用 Alchemy 或 Infura 的主要好处是简化了与以太坊网络的交互。开发者可以通过这些平台提供的 API 接口发送交易、查询区块链数据等，而不需要担心节点的维护、同步或相关的基础设施问题。
  - 这对于构建去中心化应用（DApps）、执行智能合约交互等场景尤为重要，因为它们需要可靠且高性能的接入点到以太坊网络。

### 2. 个人设置比特币全节点和 ElectrumX 服务器

- **搭建过程**：
  - 如您之前描述的，一个人可以在自己的电脑上运行比特币全节点（如 Bitcoin Core）和 ElectrumX 服务器。这样，他们就能提供一个私有的 RPC 接口，供自己或他们选择共享的人使用。
  
- **与 Alchemy/Infura 的比较**：
  - 这种设置在本质上类似于 Alchemy 或 Infura 的服务，因为它提供了一个 RPC 接口来与区块链网络交互。不过，个人搭建的节点和 ElectrumX 服务器通常用于私有目的，而不是作为一个公共服务。
  - 个人搭建的节点通常有限的带宽和资源，可能不适合处理大量的请求或提供与 Alchemy 和 Infura 相当的高可用性和性能。

### 结论

- **类似之处**：无论是使用 Alchemy/Infura 的服务还是自己搭建比特币全节点和 ElectrumX 服务器，都是在提供 RPC 接口以便于与区块链网络进行交互。
- **不同之处**：Alchemy 和 Infura 提供的是面向公众的、高性能和高可用性的服务，适用于需要稳定和大规模区块链数据访问的商业和开发场景。而个人搭建的节点和服务器更多是为了个人使用或小规模的私有目的。

因此，尽管个人搭建的比特币全节点和 ElectrumX 服务器在技术上可以提供类似 Alchemy 的服务，但在实际应用、性能、稳定性和规模方面有很大的不同。
