# Mac下安装XX—Net(翻墙)

<!--more-->

> 生活就像万花筒，缤纷精彩，于是便有了那些所谓的快乐、幸福。

xx-net是一个非常好用的翻墙软件，在windows下安装比较方便，但是随着我换Mac后发现没有一个完整的资料介绍安装过程，因此自己在这里介绍下如何安装。我用的Mac Pro17版，Mac安装XX-Net最大的坑就在于开启IPv6比较麻烦，需要关闭Mac权限控制，相当于手机root那种，我建议是安装需要开启IPV6的软件后，将权限又设置回来。Mac对隐私比较注重，因此在安装外部软件的时候，注意要在系统设置里面的隐私选项那里进入后选择一直允许。

------

## 安装chrome

xx-net使用的浏览器必须是Chrome，安装包不太容易找。安装完成时需要设定为默认浏览器。

### 下载xx-net

可直接下载[xx-net](https://github.com/XX-net/XX-Net)解压，在文件里面有一个SwitchyOmega文件夹，将.crx文件放入chrome://extensions扩展程序里进行安装，**注意**需要打开开发者模式，拖拽到扩展程序页面安装即可。然后进入xx-net文件夹点击[start]()文件，默认会终端打开，会自动打开浏览器。

## 更新情景模式

进入启动xx-net自动打开浏览器的页面，点击左侧的Import/Export导入SwitchyOmega文件夹下的[.bak]()备份情景模式文件。导入后，会在左侧多出几个自动切换模式，点击**GAE-Proxy自动切换**，然后在页面中点击立刻下载模式。启动xx-net后会提示<u>"仅 IPv6" 不可用，请检查</u>这个时候就需要开启ipv6

### 关闭Mac权限控制

Mac是限制安装第三方来源的软件，可以在终端中用`csrutil status` 命令来查看权限是否打开，如果是enable则需要关闭。可以按以下步骤进行操作：

- 重启开机的时候不停按住`comman + r`键，会进入修复模式。
- 在上面选择终端，输入`csrutil disable`会提示关闭成功，然后重启电脑。

### 安装开启IPv6软件

- 下载安装[tuntaposx](<https://sourceforge.net/projects/tuntaposx/>)
- 下载安装[mired](<http://www.deepdarc.com/miredo-osx-prerelease2.pkg.zip>),**注意的是**需要将在系统设置中，点击安全与隐私设置，允许一直打开其他来源安装。否则pkg包不会允许安装。

### 启动xx-net

一切准备就绪后就可以运行xx-net了，在XX-Net文件夹下，点击`start`会自动打开Chrome，从页面中的状态我们可以看到一切ok，然后选择右侧switchyomega图标中GAE-Proxy自动切换，这时候我们键入[google](www.google.com)检验安装是否成功。如果不能访问，检查前面5个步骤是否都做到位。
