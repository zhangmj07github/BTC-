摘自：https://twitter.com/0xFatDog/status/1730189598855438721。 感谢@0xFatDog，感兴趣的朋友请关注其twitter


  
-------------------------------

1. 安装node： 进入https://nodejs.org/en，下载左边带LTS 的版本，然后一路默认安装
2. 安装git：进入https://git-scm.com/download/，下载最新版，一路默认安装
3. 下载源代码：打开https://github.com/atomicals/atomicals-js，点击绿色的code按钮-download zip
3. 解压上述压缩包
4. 进入到解压后的文件夹里，确认该文件夹包含 package.json文件
5. 在文件夹空白处点击鼠标右键，会出来一个“git bash here”，选择它
6. 执行 npm i -g yarn
7. 执行 yarn （这一步需要开梯子，可能会持续十几分钟）
8. 执行 yarn build
9. 执行 yarn cli wallet-init
10. 配置内存选项，防止中途内存溢出：执行 set NODE_OPTIONS=--max-old-space-size=8192
11. 开始挖矿： 执行 yarn cli mint-dft electron --satsbyte=117  (这里的117是以200gas为基准，自己视情况改，预估gas除以1.7可得出该值  200/1.7 = 117)，会出来一个二维码让你往里面转钱，记住转的金额一定要严格相等，让你转多少就转多少，你转过去以后就会开始挖矿
12.  等待挖矿成功后上链（需要好几天，请确保中途你的电脑不会断电断网、睡眠等）

electron常见误区

例举几个其他教程常见的错误：

1. yarn cli mint-dft dmint： 这个是最离谱的，我们挖的是electron，他这里是dmint，代币都错了
2. yarn cli mint-dft electron ——satsbyte=20 这个也离谱，中英文都分不清楚，正确的应该是两个英文的短横杠 --
3. npm run cli mint-dft electron --satsbyte 20  npm这样设置gas是无效的，这里最好使用yarn并且在--satsbyte后面加上等号：yarn cli mint-dft electron --satsbyte 20
4.  不教你配置内存限制 set NODE_OPTIONS=--max-old-space-size=8192： 没有这个的话 容易半路爆内存，也白跑(如果是macos，需要换成 export NODE_OPTIONS=--max_old_space_size=8096)
