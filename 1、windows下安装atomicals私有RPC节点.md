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
