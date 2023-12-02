原文https://mirror.xyz/fomoplz.eth/8pO8xEV4LptbsbWUPPw2NFPzCj3jfpnjCY0s2CzXDJ4，感谢熊猫撸白.nft fomoplz.eth

  ----------
  前几天刚写了linux搭建全节点的教程，不过云服务器搭建门槛稍高，应读者要求，今天再来写一篇windows搭建全节点铸造铭文的教程，这篇教程适用于没有技术背景的小白，我会详细到每一个步骤，我相信每个人都能拥有一个属于自己的全节点（需要准备一个1T存储以上的固态硬盘）。

搭建步骤：

BitcoinCore客户端安装

全节点数据同步

Ord钱包安装

索引区块

铸造铭文

BitcoinCore客户端安装
直接点击链接https://bitcoincore.org/en/download/ 来到客户端的下载页面，点击蓝色按钮，下载最新的版本。


下载完成后打开安装程序，点击"Next"


下一步就是选择安装目录，可以使用默认的安装路径,也可以点击“Browser“自定义安装路径。选择好之后，继续点击"next"。



等待安装完成，最后点击finish即可。


接下来的两步比较关键，首先要选择存放区块数据的目录，需要大于502GB的存储空间，这里点击后面的”...“按钮，选择1T的固态硬盘所在的目录，比如”E:\BtcData“, 其次就是不能勾选下方箭头所指的”勾选框“，如果勾选上就代表使用裁剪模式，而铸造铭文必须使用完整的数据。最后点击"ok"按钮。


进入到钱包界面后，点击"设置"->”选项“


第一个自动登入可选可不选，勾上的话开机之后会自动同步最新的数据

第二个设置的地方是将数据库缓存大小改为你内存大小的25%，比如16GB内存就设置4096MB比较合适。

第三个勾选的地方是启用RPC服务器，这个是允许客户端可以与Bitcoin core节点进行通信。


整个客户端的设置到这儿基本也就设置好了。

全节点数据同步

接下来就是去同步全节点的数据，这里我建议大家不要从头开始同步，我们要踩在巨人的肩膀上帮我们提高效率，可以使用别人已经下载好的数据。

点击下方的链接：

GitHub - CryptoCellLabs/Bitcoin_FullNode_Data: Provide complete bitcoin node data to facilitate everyone to quickly synchronize complete bitcoin nodes
GitHub - CryptoCellLabs/Bitcoin_FullNode_Data: Provide complete bitcoin node data to facilitate everyone to quickly synchronize complete bitcoin nodes
Provide complete bitcoin node data to facilitate everyone to quickly synchronize complete bitcoin nodes - GitHub - CryptoCellLab…
github.com
可以直接下载全节点数据，不过这个是同步到2022年8月15日区块的数据，下载之后需要继续同步至最新的区块。


全节点数据需要用7-zip进行解压，然后放到之前创建的E:\BtcData目录下，就像这样。


在继续同步之前我们还需要在此目录下新建一个配置文件"bitcoin.conf"，并写入如下内容，addnode可以用来添加一个节点，可以加快下载速度，这些节点的ip可以从以下的网站获得：https://bitnodes.io/,优先使用国内的节点，最后保存即可。

server=1
daemon=1
txindex=1           
rpccookiefile=.cookie 
addnode=81.68.102.34:8333
addnode=60.205.205.119:8333
addnode=223.167.74.199:8333
addnode=47.104.221.103:8333
addnode=112.19.172.217:8333
addnode=47.109.21.215:8333
addnode=120.244.105.164:8333
addnode=47.108.165.145:8333
addnode=47.105.52.43:8334
addnode=60.205.205.119:8333
addnode=182.100.67.50:8333
为了让Bitcoin core客户端能找到全节点数据，我们还需要右键桌面快捷方式->单击"属性"。在"目标"这一栏添加上

-datadir=E:\BitcoinData -txindex

等号后面替换成存储比特币区块数据的目录  -datadir前面要留有一个空格,-txindex 用于下载比特币区块的索引数据。


将以上工作全部做完，启动客户端等待同步完成之后，可以来到控制台。

在下方输入getblockcount，将得到的区块数和mempool 浏览器（https://mempool.space/zh/ ）中的最新区块进行对比，如果一致，则代表同步成功。


输入getindexInfo,可以查看索引数据同步的情况，当”synced”变为true时则表示索引同步完成。


Ord钱包安装
点击下方链接来到下载页面：https://github.com/casey/ord/releases

下载最新版本0.5.1的windows压缩包


你可以在E盘新建一个ord文件夹，将解压后的可执行文件放在下面。


之后，将ord.exe的目录添加到系统环境变量中,编辑环境变量的方式如下：




添加完成后，我们使用“WIN+R”，输入cmd,打开命令行窗口，输入 ”ord --version“，查看版本是否正确。若能正确输出，则表示环境变量添加成功。


索引区块
我们也没有必要从头开始索引区块，可以先在E盘下创建一个文件夹 ”OrdData“ 用于保存索引的数据。然后点击下方的链接，https://github.com/CryptoCellLabs/Ordinals-Index-Data ，直接下载索引数据index.redb,并将其放在OrdData下。


下载好之后，我们首先通过命令行创建一个钱包，可以使用--wallet 自定义名称，否则默认名称为ord,--bitcoin-data-dir 是用于指定你全节点数据存放的目录。执行成功后就会生成一段12个单词的助记词，记得保存好。

ord --wallet test --bitcoin-data-dir E:\BitCoinData wallet create

生成的钱包可以在BitCoinData目录下查看


输入下面的指令是查看钱包中的余额，同时也会自动开始索引区块，当进度条达到最大值时就会索引完成,显示余额，--data-dir 指定index.redb存放的目录。

ord --wallet test --bitcoin-data-dir E:\BitcoinData --data-dir E:\ordData wallet balance


铸造铭文
当区块索引完成之后，就可以开始铸造铭文了，输入下方的命令，一定要自己指定--fee-rate,默认的是最低优先级的，抢不过别人。

ord --wallet panda --bitcoin-data-dir E:\BitcoinData --data-dir E:\ordData wallet inscribe pic\ord.txt --fee-rate 8

该如何合理设定fee-rate，可以来到如下网站查看当前费率的优先级

mempool - Bitcoin Explorer
mempool - Bitcoin Explorer
Explore the full Bitcoin ecosystem with The Mempool Open Source Project®. See the real-time status of your transactions, get net…
mempool.space

想要查看当前自己的交易还要排队多久，可以将上面铭刻时，输出的commit后面的字符粘贴到这个网站的搜索框中，再按回车即可。


其他指令
还有一些其他常用的指令，我已经整理在下图，如果是没有安装在默认位置，记得添加上面所讲的参数.


总结：
以上是用windows搭建全节点铸造nft的过程，希望能帮到你，如果你在搭建的过程中碰到什么问题，欢迎添加下方微信，备注“BTC全节点”，进入免费交流群

