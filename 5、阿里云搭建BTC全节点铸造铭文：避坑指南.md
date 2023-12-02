目前在各种媒体上看到的全节点搭建教程大多是基于windows操作系统，对于没有技术基础的用户来说，我也推荐使用windows系统来搭建，优点是图形界面操作更加便捷，一次搭建永久使用，无需续费。

本篇指南是为了弥补市面上对于云服务器搭建全节点的内容空缺，同时记录一下在搭建过程中碰到的种种问题以及我解决问题的思路。本文适用于有一定技术积累、追求效率和后续想在此基础上开发工具的用户群体。

以下为铸造铭文的步骤：

购买云服务器
同步全节点数据
安装ord钱包
创建钱包
索引区块
铸造铭文


购买云服务器

这一步是比较关键的，选择合适配置的云服务器可以避免后面我碰到的大部分问题，特别是硬盘大小，比特币全节点数据记录着从2009年1月3日开始至今的所有数据，总大小大约在500G左右,并且每个月增长的速率为5-10G，所以这里我推荐使用最少1TB的固态硬盘,以下是我的服务器配置：

图片

图片

其中云盘一共有两块，一块是系统盘，另一块是我挂载的1TB数据盘,这块盘就是用于存放Bitcoin全节点数据的地方。

接下来就是我要说的第一点：当你购买的云盘大小已经存放不下最新的数据的时候该如何去做？

有两种解决方案：1.直接扩容，无需迁移数据  2.挂载一块更大的云盘，将数据迁移。根据测算扩容的租赁成本稍比挂载多一点点。关于如何操作，可以参考阿里云的官方教程，我认为已经写的非常清楚，按照步骤操作即可：

扩容: https://help.aliyun.com/document_detail/35095.html?spm=a2c4g.11186623.0.0.7b745e32003W3M
挂载: https://help.aliyun.com/document_detail/25446.html


同步全节点数据

首先如果你和我一样按照官方文档挂载了另外一块云盘，那么它的挂载目录默认是/mnt目录，这个可以类比成windows系统上的E:\ 。

接着输入以下指令：

cd /mnt   //进入mnt目录
mkdir btc //创建btc目录
cd btc    //进入btc目录
mkdir btc-data //创建保存btc数据的目录，以上目录均可自定义
下载bitcoin-24.0.1版本：

wget https://bitcoincore.org/bin/bitcoin-24.0.1-x86_64-linux-gnu.tar.gz
下载完成后，可以解压压缩包，并将其安装到`/usr/local/bin`目录下，可以让系统直接访问到：

tar -xzf bitcoin-24.0.1-x86_64-linux-gnu.tar.gz   //解压文件
sudo mv bitcoin-24.0.1/bin/* /usr/local/bin/      //拷贝文件
如果不放到系统目录下，也可以按照如下方法添加环境变量：

vim  ~/.bashrc  //编辑环境变量文件
在文件末尾添加bitcoincore文件目录

export BTCPATH=/mnt/btc/bitcoin-24.0.1/bin   //根据自己文件目录修改
export PATH=$BTCPATH:$PATH
使环境变量生效

source ~/.bashrc
在btc-data下添加bitcoind的配置文件bitcoin.conf,并输入如下内容，addnode可以用来添加一个节点，可以加快下载速度，这些节点的ip可以从以下的网站获得：https://bitnodes.io/,优先使用国内的节点。

server=1
daemon=1
txindex=1   //txindex必须要加，因为ord钱包需要bitcoin的索引数据         
addnode=81.68.102.34:8333
addnode=60.205.205.119:8333
addnode=223.167.74.199:8333
addnode=47.104.221.103:8333
addnode=112.19.172.217:8333
addnode=47.109.21.215:8333
addnode=120.244.105.164:8333
addnode=47.108.165.145:8333
输入以下指令开始同步：

bitcoind -datadir=/mnt/btc/btc-data -txindex    //datadir后替换为你自己的目录
Bitcoin数据同步过程已经经过很多年的验证，一般来说不会出现问题，不过你也有可能由于网络原因碰到我下面这个问题，解决方式就是多次尝试，直到同步成功为止。

图片

当同步完成之后，输入如下指令得到最新的区块数，然后和mempool 浏览器（https://mempool.space/zh/ ）中的最新区块进行对比，如果一致，则代表同步成功。

安装ord钱包

你可以使用下面的脚本去安装ord钱包

curl --proto '=https' --tlsv1.2 -fsLS https://ordinals.com/install.sh | bash -s
或者去官方的github直接下载安装包，最新的版本是0.5.1

https://github.com/casey/ord/releases
然后解压到自定义目录，最后记得把ord执行文件的目录按照前面提到的方法加入环境变量文件中。

图片

创建钱包

首先使用以下指令创建一个钱包，使用 --wallet 可以指定钱包的名称，如果不使用则默认名称为“ord”。如果你修改了bitcoin core数据安装目录，则必须使用参数 --bitcoin-data-dir 指定目录，才可以创建钱包，创建钱包成功后，会生成12个助记词，保存好。

ord --wallet example  --bitcoin-data-dir /mnt/btc/btc-data  wallet create
图片

索引区块

输入如下查看钱包账户的指令，说一下第一个参数 --cookie-file 指定了RPC调用验证的cookie文件路径，第三个参数--data-dir,这个参数是用于保存ord钱包索引区块的数据，当索引开始时，在该目录下会生成一个index.redb文件，这是用rust编写的轻量级的数据库。

ord --wallet example  --cookie-file /mnt/btc/btc-data/.cookie --bitcoin-data-dir /mnt/btc/btc-data --data-dir /mnt/btc/ord_data  wallet balance
开始索引后，就会出现如下的进度条，有的时候也许会报错，卡顿很久，耐心等待或者多次尝试即可，整个过程耗时也比较长，差不多需要1至2天，不过现在随着解决方案越来越成熟，很多人已经打包好了区块数据和index.redb文件，下载下来直接用即可，我会在文末放上github链接

图片

铸造铭文

当索引完成后，最后就是享受胜利果实的时候，铸造铭文的指令如下,inscrible后添加你要铸造的文件即可，记得加上 --fee-rate,这个参数可以用于设置费率，你给的越多铸造的越快，当然成本也会更高，15的费率只能算是中上等，前两天在疯狂内卷的时候，费率经常飙到30,费率可以通过mempool查看，进行动态调整。

 ord --wallet example --cookie-file /mnt/btc/btc-data/.cookie  --bitcoin-data-dir /mnt/btc/btc-data --data-dir /mnt/btc/ord_data  wallet inscribe panda.txt --fee-rate 15


其他指令

还有一些其他常用的指令，我已经整理在下图，如果是没有安装在默认位置，记得添加上面所讲的参数.

图片

总结：

以上是用linux服务器搭建全节点铸造nft的过程，希望能帮到你，不过我相信在你自己操作的过程，一定会碰到各种各样的问题，欢迎添加下方微信，备注“BTC全节点”，进入免费交流群。最后，附上全节点和index.redb数据的github链接：https://github.com/CryptoCellLabs/Ordinals-Index-Data   https://github.com/CryptoCellLabs/Bitcoin_FullNode_Data
