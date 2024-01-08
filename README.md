---
description: https://www.youtube.com/watch?v=zXlvkOSVM9M
---

# 🦜 PART 1 - syncing Bitcoin core, installing ord

如果不想花43分钟看casey和erin打情骂俏，那么我们把这个视频内容摘录一下，再加上我自己的踩坑心得，希望可以让大家少走弯路。再次说明，我只是一个不成功的投资人和一个蹩脚的程序员，因为觉得铭文在未来2-3年都有创业和投资机会而学习，所以对不同投资水平和技术水平的人来说，我写的东西有的可能太浅了，有的可能太深了，还请多多包涵。



Casey一开始打开Notepad就是写了一个日程，大概的流程如下：

:tada:\[x] homebrew install

:tada:\[x] bitcoincore install

:tada:\[x] start bitcoin&#x20;

:tada:\[x] install cargo with rustup

:tada:\[x] install ord server

里面Casey给美女show了很多电脑技巧，也做了一个很好的MAC电脑使用和优秀程序员使用工具的科普。我想起大学的时候给学文科的同学show命令行和快捷键，Casey这个视频满足搞技术的男生的一切幻想。但是，大学里就是在同学面前吹吹牛皮，没有钱；在Crytpo领域，你技术好就可以在开源社区挣大钱了，不用再间接跟产品和销售打交道，有了钱自然就有颜如玉，从这一点看，技术男不搞币圈就入错行了。

先说下我配置的情况，我是2023年的MacBOOK Pro，苹果的M2芯片，我先是在Bitcore.org里面下了图形化界面的安装包（bitcoin-25.0-arm64-apple-darwin.dmg），但是它安装后只有qt命令，没有我更喜欢更酷的bitcoind和bitcoin-cli等后台命令，所以还是要用到Casey第一小节提到的homebrew。

:tada:\[x] homebrew install

_Homebrew是一个包管理软件，自称是_The Missing Package Manager for macOS。我们Google Homebrew可以打开[https://brew.sh/](https://brew.sh/)， 里面有一个安装脚本

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

在Macbook的终端里，输入就可以安装好。安装好以后我们进入第二步。

:tada:\[x] bitcoincore install&#x20;

只需要在终端输入

```
// brew install bitcoin
```

就可以安装好bitcoin最新版。这非常强大。很多教程都是让去官网下载，然后解压，然后把bitcoincore里面的命令如bitcoin-cli，从usr的bin拷到全局的bin文件夹里，还要更新环境变量，没一点计算机基础的很快就晕了，有点基础的也容易在环境变量等地方反复配置，让人容易泄气。所以_Homebrew把这些都安排好了，你只需要一句话安装，就ok。_

:tada:\[x] start bitcoin&#x20;

别小看这一步，不就是启动bitcoincore这个程序吗，很多坑。一般默认的bitcoincore，它的数据目录是在一个默认的地方比如 \~/Library/Application Support/Bitcoin/ ，但大家的硬盘都不够，流行的办法是要外挂一个2T的SSD（闪存），价格基本是600-800元，所以我们要启动程序，要用命令。这一点是Casey的视频没讲的，他们的电脑一上来就有1T的自带硬盘。我的硬盘名字叫Element，默认是挂载在/Volumes下，所以我启动需要这个命令，-datadir是说把数据目录放到Element盘下的bitcoin目录， -txindex要求一开始就索引。

```
// bitcoind -datadir=/Volumes/Element/bitcoin -txindex
```

几个小点：

A)我买的硬盘一开始在同步的时候老是同步失败，Bitcoin Core客户端在尝试将数据写入磁盘时遇到了一个严重的I/O（输入/输出）错误。错误信息“FileCommit: fcntl F\_FULLFSYNC failed: 25”表明尝试对文件进行完全同步到磁盘的操作失败了。一开始我以为是权限问题，用sudo什么的都试过了，还是不行。

后来我在网上看有一个经典的帖子讨论[https://github.com/bitcoin/bitcoin/issues/27972#issuecomment-1611123854\
](https://github.com/bitcoin/bitcoin/issues/27972#issuecomment-1611123854)，是因为硬盘的格式问题，我是windows和mac通用的ExFAT格式，这对bitcoin数据的写入有些不兼容，要先格式化为APFS。

B)要现在Elements盘里加一个文件夹bitcoin才行，不然也通不过。

C)最好一开始就保存好一个文件，bitcoin.conf，我总结如下内容最佳

> txindex=1&#x20;
>
> server=1&#x20;
>
> daemon=1&#x20;
>
> rpccookiefile=.cookie&#x20;
>
> datadir=/Volumes/Element/bitcoin blocksdir=/Volumes/Element/bitcoin rpcauth=nextdao:cca838b4b19bdc6093f4e0312550361c$213834a29e8488804946c196781059a7ee0ac2b48dbf896b4c6852060d9d83dd&#x20;
>
> rpcallowip=127.0.0.1
>
> rpcallowip=172.0.0.0/8&#x20;
>
> rpcallowip=192.168.0.0/16
>
> rpcbind=0.0.0.0

然后把这个文件贴到/Volumes/Element/bitcoin 里面，这是权限最高的配置，bitcoincore就按这个来运行。 这里面都是啥含义？把它全部粘贴问chatgpt。

D)这里的rpcauth=nextdao:cca838b4b19bdc6093f4e0312550361c$213834a29e8488804946c196781059a7ee0ac2b48dbf896b4c6852060d9d83dd 本质就是用户名为nextdao，密码是nextdao，这是粘贴之前nextdao给atomical协议写的教程，也算致敬他们直接用了。有了这段话，就可以用bitcoin-cli -rpcuser=yourusername -rpcpassword=yourpassword getblockchaininfo 来登录，并运行代码，比如看getblockchaininfo （这是视频里casey花了很多时间回忆的命令，最后google到了，看这一段很花时间）。

```
// bitcoin-cli -rpcuser=nextdao -rpcpassword=nextdaogetblockchaininfo

```

rpcauth很重要。如果你的比特币全节点在一个电脑，你要用另外一个电脑RPC，如果是明文用户名和密码容易被拒绝。这时你可能要排除bitcoin.conf的配置，防火墙问题，反复查很久原因。其实是因为新版的RPC，要求更安全的连接方式。

如果你成功运行bitcoincore了，你的数据目录Element\bitcoin是很干净的，里面还有几个重要文件夹如blocks(里面一大堆dat文件）,chainstate（里面一大堆ldb文件），indexes（里面也有很多ldb），文件如debug.log，bitcoin.conf

如果你要看看日志，在终端新建窗口，输入

```
tail -f debug.log
```

在这一小节，还有一些命令经常用，比如停止bitcoin程序。因为你粘贴了bitcoin.conf，所以要重启

```
Bitcoin-cli stop
```

:tada:\[x] install cargo with rustup

这主要是安装rust语言环境以及安装包，cargo 类似Python里的pip，Nodejs的npm。看视频的[28:30](https://www.youtube.com/watch?v=zXlvkOSVM9M\&t=1710s) Install cargo with rustup 或者直接问chatgpt.



:tada:\[x] install ord server

一句代码

```
ord server
```

安装索引器。在安装之前，建议大家用一个python程序，看看你能否远程对你运行的bitcoincore编程？这是我跟chatgpt磨合很久写出来的

```
import requests
import json

# 指定比特币RPC服务器的地址和端口
# rpc_url = "http://192.168.10.57:8332"
# rpc_url = "http://127.0.0.1:8332"
# rpc_url = "http://192.168.10.11:8332"
# rpc_url = "http://221.216.144.195:8332"
# rpc_url = "http://192.168.1.96:8332"
rpc_url = "http://192.168.1.86:8332"
# 指定比特币RPC用户名和密码
rpc_credentials = ("nextdao", "nextdao")


def bitcoin_rpc(method, params):
    payload = {
        "method": method,
        "params": params,
        "jsonrpc": "2.0",
        "id": 0,
    }
    print("Request payload:", payload)  # 打印请求内容
    response = requests.post(rpc_url, json=payload, auth=rpc_credentials)

    # 打印响应的状态码和文本内容
    print("Response status code:", response.status_code)
    print("Response text:", response.text)

    if response.status_code == 200:
        return json.loads(response.text)
    else:
        raise Exception("RPC request failed with status code {}: {}".format(response.status_code, response.text))


# 示例：调用getblockchaininfo和getnetworkinfo 和 getblockcount
try:
    result = bitcoin_rpc("getblockchaininfo", [])
    print(result)
except Exception as e:
    print(f"Error: {str(e)}")
try:
    result = bitcoin_rpc("getnetworkinfo", [])
    print(result)
except Exception as e:
    print(f"Error: {str(e)}")
try:
    result = bitcoin_rpc("getblockcount", [])
    print(result)
except Exception as e:
    print(f"Error: {str(e)}")
```

如果成功，会报200，给类似如下的结果。注意我记录当日当前比特币是824858个区块，80多万个吧。如果你的数字差距太大要注意。

```
Request payload: {'method': 'getblockchaininfo', 'params': [], 'jsonrpc': '2.0', 'id': 0}
Response status code: 200
Response text: {"result":{"chain":"main","blocks":824858,"headers":824858,"bestblockhash":"00000000000000000003c42c52579dfb4bd3a9dbc64997b634bf94c0a323772d","difficulty":73197634206448.34,"time":1704718871,"mediantime":1704713627,"verificationprogress":0.9999998181796409,"initialblockdownload":false,"chainwork":"000000000000000000000000000000000000000064a1bfc6d211b8d17258c8e3","size_on_disk":612770777418,"pruned":false,"warnings":""},"error":null,"id":0}

{'result': {'chain': 'main', 'blocks': 824858, 'headers': 824858, 'bestblockhash': '00000000000000000003c42c52579dfb4bd3a9dbc64997b634bf94c0a323772d', 'difficulty': 73197634206448.34, 'time': 1704718871, 'mediantime': 1704713627, 'verificationprogress': 0.9999998181796409, 'initialblockdownload': False, 'chainwork': '000000000000000000000000000000000000000064a1bfc6d211b8d17258c8e3', 'size_on_disk': 612770777418, 'pruned': False, 'warnings': ''}, 'error': None, 'id': 0}
Request payload: {'method': 'getnetworkinfo', 'params': [], 'jsonrpc': '2.0', 'id': 0}

```

A)多用which，看看你装的cargo， ord 服务器都在哪？这也是视频多次提到的。

B)我运行ord的时候出现找不到cookie: error: cookie file \`/Users/aaronzhang/Library/Application Support/Bitcoin/.cookie\` does not exist 这是因为登录的文件找不到了，需要复制一下

```
ln -s /Volumes/Element/bitcoin/.cookie /Users/aaronzhang/Library/Application\ Support/Bitcoin/.cookie
```

Ord运行后，就一直监听80窗口，一旦比特币核心程序有新的区块同步，他就索引。

打开 http://localhost，如果你看到跟http://ordinals.com类似的样子，你就安装这个索引器成功了。

看看视频里男主和美女是如何庆祝的，[41:20](https://www.youtube.com/watch?v=zXlvkOSVM9M\&t=2480s) Load local Ordinals Block Explorer 看那个clock,如果你的程序里也有，你也会有成就感。恭喜你，开始挣钱了。

\








