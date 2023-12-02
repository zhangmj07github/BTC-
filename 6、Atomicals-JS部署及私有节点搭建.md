原文链接：https://mirror.xyz/cyberscavenger.eth/rayhn9RT1VMUMo4Iios3g0A6G6AtZ-EXfQ8F_kj8HhQ

--------
教程有更新内容，已经全部操作完前面步骤的朋友可以拉到最后看更新部分。

第一次看教程的可以从第一步开始操作，到私有节点搭建部分时，结合着更新内容一起做，把更新部分做完后，再一起运行docker。

部署Atomicals-JS-Master钱包
1.安装Node.js
Node.js
Node.js
Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.
nodejs.org
在Node.js官网下载最新版本安装文件，按照安装向导进行安装。

安装最新版即可
安装最新版即可
安装完成后，可以在 Powershell 或 cmd 命令提示符窗口输入： node -v 验证安装是否成功。返回值如果正确显示版本号，即代表安装成功。（教程中我的版本由于安装的较早，所以跟官网最新版本不一样，我没做升级，大家直接下载最新版即可）

使用node -v命令验证安装结果
使用node -v命令验证安装结果
2.下载&配置Atomicals-JS-Master
GitHub - atomicals/atomicals-js: Atomicals CLI and Javascript Library
GitHub - atomicals/atomicals-js: Atomicals CLI and Javascript Library
Atomicals CLI and Javascript Library. Contribute to atomicals/atomicals-js development by creating an account on GitHub.
github.com
在Atomicals官方Github主页，点击Code，在弹出的窗口中选择 Download ZIP。

下载程序
下载程序
解压缩程序到目录，然后切换到该目录，并在资源管理器地址栏输入cmd，回车，打开命令提示符窗口：

在Atomicals-JS-Master文件夹内打开cmd
在Atomicals-JS-Master文件夹内打开cmd
依次运行下面命令：

npm install -g typescript

npm install -g typescript
npm install -g typescript
npm run build

npm run build
npm run build
npm install -g yarn

npm install -g yarn
npm install -g yarn
yarn install

yarn install
yarn install
yarn cli wallet-init

yarn cli wallet-init
yarn cli wallet-init
最后一步是创建钱包，命令运行后，会在文件夹内生成一个wallet.json的文件，这里保存着助记词和私钥。

说明：创建钱包后，会自动生成2个地址，Primary Address及Funding Address，其中Primary是用于接收Atomicals生态资产的，比如ARC20代币、图片NFT等，Funding是用于铸造（挖矿）过程的中转钱包，通常是往Funding地址中存入相应数量的BTC，用于铸造。

至此，部署已经完成，下面是常用的命令：

铸造FT币命令：

yarn cli mint-dft dmint --satsbyte 30

#注意这里设置的gas，与实际上链的gas是1.8~2倍的关系，比如这里设置30，实际上链gas是54~60sats/vB。

铸造Realm命令：

yarn cli mint-realm "btc" --satsbyte 30 --satsoutput 1000 --bitworkc 3165

铸造图片NFT命令：

yarn cli mint-nft "E:\Crypto\NFT\CryptoPunks\punk0000.png" --satsbyte 30 --satsoutput 1000 --bitworkc 3165

查询余额命令：

npm run cli balances

搭建私有Atomicals节点（需要本地BTC全节点支持）
1.安装Docker
Install Docker Desktop on Windows
Install Docker Desktop on Windows
Get started with Docker for Windows. This guide covers system requirements, where to download, and instructions on how to instal…
docs.docker.com
在Docker官网下载最新版本安装文件，按照安装向导进行安装。

点击下载，并安装
点击下载，并安装
根据安装向导进行安装，选择Use WSL 2 instead of Hyper-V(recommended)

选择WSL2
选择WSL2
等待安装
等待安装
点击Close and log out注销并重新登录
点击Close and log out注销并重新登录
完成安装后需要注销，并重新登录Windows。

重新登陆后，在弹出的窗口，点击 Accept 接受协议。

注册并登录Docker（可以使用Google、Github登录）。

2.下载&配置Atomicals-Electrumx-Docker
GitHub - Next-DAO/atomicals-electrumx-docker
GitHub - Next-DAO/atomicals-electrumx-docker
Contribute to Next-DAO/atomicals-electrumx-docker development by creating an account on GitHub.
github.com
在Next-DAO的Github页面，下载压缩包，并解压缩到文件夹*（步骤和方法与安装Atomicals-JS-Master类似，参考上面步骤即可）*

bitcoin/share/rpcauth/rpcauth.py at master · bitcoin/bitcoin
bitcoin/share/rpcauth/rpcauth.py at master · bitcoin/bitcoin
Bitcoin Core integration/staging tree. Contribute to bitcoin/bitcoin development by creating an account on GitHub.
github.com
在Bitcoin的Github页面，下载rpcauth.py代码，可以放在刚才atomicals-electrumx-docker的文件夹里。

下载代码
下载代码
在文件夹内打开cmd，运行：

python rpcauth.py 用户名 密码

其中，用户名和密码位置自行修改，运行把返回参数记录下来，等下会用到。

此步骤需要安装Python，这里不再赘述。

记录返回值
记录返回值
打开Bitcoin Core的conf配置文件，添加如下参数：

server=1

txindex=1

daemon=1

rpcuser=用户名 #设置一个用户名

rpcpassword=密码 #设置一个密码

rpcauth=electrumx:c7ed296134ebe0035d9ff786dfa102b5$9d40e8e36 #这里是上一步生成记录的返回值

**************************2023.11.30调整-修改部分*******************************

rpcbind=127.0.0.1

rpcbind=192.168.31.188 #这里需要自行查看本机的IP地址，可以通过ipconfig命令查看

rpcallowip=127.0.0.1

rpcallowip=192.168.31.188 #这里需要自行查看本机的IP地址，可通过ipconfig命令查看

2023.11.30，调整一下这部分内容，因为发现很多人重启电脑后，本机IP会变化（因为大多数人使用了DHCP自动获取IP，每次会开关机后有可能会自动分配不同IP），所以还要再次修改conf文件，现在改为下面的内容后，可以避免这种情况，如果你已经在成功运行，并且没有报错，电脑也不经常关机，那么可以不做修改。
rpcallowip=127.0.0.1

rpcallowip=172.0.0.0/8

rpcallowip=192.168.0.0/16

rpcbind=0.0.0.0

改完后就涵盖了192.168号段的所有IP。

**************************2023.11.30调整-修改部分*******************************

我的配置如图（2023.11.30修改前内容）
我的配置如图（2023.11.30修改前内容）
2023.11.30修改后内容样例
2023.11.30修改后内容样例
新建一个.env文件，并在文件内写入：

DAEMON_URL=用户名:密码@192.168.31.188:8332

这里的用户名和密码就是上一步Bitcoin Core的conf里设置的用户名和密码


**********************************************************************************

/*第一次看教程的朋友，从这里往下可以不做，直接去看后面更新部分。

在Atomicals-Electrumx-Docker文件夹里打开cmd，并运行：

docker-compose pull && docker-compose up -d

配置中。。。
配置中。。。
开启服务器
开启服务器
运行命令后，可以关闭cmd窗口了。Docker开始同步节点信息，完全同步至最新区块高度，大约需要90G磁盘空间，需要1天半时间。可以打开Docker APP 来查看同步进度。

在Docker查看
在Docker查看
第一次看教程的朋友，从这里往上到 /*可以不做，直接去看后面更新部分。

*/

**********************************************************************************

全部同步完成后， 打开atomicals-js-master文件夹里的.env文件，替换原有节点URL为：

ELECTRUMX_PROXY_BASE_URL=http://localhost:8080/proxy

替换节点URL
替换节点URL
至此，私有节点就已经搭建完成。

Docker其他相关命令，在cmd中运行下列命令实现对应功能：

检查electrumx是否准备就绪

docker-compose ps

使用cmd查看进度

docker-compose logs -f

关闭服务器

docker-compose down

2023.11.15晚9点-更新内容
更新内容是：本地索引服务支持。由于Next-DAO的Github把两个模式的docker-compose配置文件放在不同的页面，所以上一节教程中没有本节内容，需要操作本节内容才能正确开始本地节点服务。不影响之前的同步数据和进度。

1.首先停止已经在运行的Docker，并删除旧的Container。

删除旧的Container
删除旧的Container
2.在Next-DAO的GitHub页面，下载docker-compose.yml文件，并替换文件夹内现有的同名文件：

GitHub - Next-DAO/atomicals-electrumx-proxy-docker
GitHub - Next-DAO/atomicals-electrumx-proxy-docker
Contribute to Next-DAO/atomicals-electrumx-proxy-docker development by creating an account on GitHub.
github.com
下载docker-compose.yml
下载docker-compose.yml
下载新的docker-compose.yml并替换现有同名文件
下载新的docker-compose.yml并替换现有同名文件
docker-compose.yml文件中，需要填写全节点的rpcuser及rpcpassword，${IP:?}部分为本机IP，如果之前的步骤正确创建并配置了.ENV文件，这里可以不改，如果没有配置.ENV，则${IP:?}改为本机IP即可。

3.把 data 文件夹 重命名为： electrumx-data

把 data 文件夹 重命名为：electrumx-data
把 data 文件夹 重命名为：electrumx-data
上面3步操作完成后重启电脑

4.然后在文件夹内打开cmd命令提示符窗口，并运行：

docker-compose pull && docker-compose up -d

如果报错：&&不是有效语句分隔符，就分开运行这两段代码即可。

atomicals-js-master文件夹里的.env文件配置与上一节教程一样，无需改动。

5.1检查docker同步状态：在浏览器地址栏输入 http://192.168.31.188:8080 ，如果返回 “success”：true 则表示节点运行正常，docker会继续同步数据并检索本地atom协议的交易，此过程也要持续数小时。

5.2检查节点状态：在浏览器地址栏输入 http://192.168.31.188:8080/proxy/health ，如果返回 “success”：true ， “health”：true则表示节点已经准备就绪，此时就可以用atomicals-js工具连接私有节点正常使用了。

说明：由于Windows系统上docker的效率本身就低，再加上网络等各种不可控原因，docker下的私有节点并不稳定，及时电脑不关机持续运行，也会导致节点莫名其妙丢失状态（双false状态），此时不用管它，docker会自动继续同步，直到再次准备就绪。（如果期间docker卡死，就需要手动去重启docker）。

（注意这里的IP地址要替换为你得本机IP地址）

返回 “success”：true 则表示docker运行正常
返回 “success”：true 则表示docker运行正常
返回 “success”：true，“health”：true 则表示节准备就绪
返回 “success”：true，“health”：true 则表示节准备就绪
