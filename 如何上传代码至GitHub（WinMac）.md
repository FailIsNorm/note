一:首先是在github官网注册自己的账号 https://github.com/



二:安装Git https://git-scm.com/downloads

1.1根据自己的电脑系统安装


三:(选看)如何判断自己成功安装Git

Win:

1.按住win + R打开命令提示符并输入cmd

2.输入 git --version

3.出现以下内容即是安装成功

![img](https://i0.hdslb.com/bfs/article/a1d4af63a8aa50e5a426503f2c8aa340ebd3b3df.png@843w_90h_progressive.webp)

​									<!--2.1win或mac同样效果-->
Mac:

1.按住command + 空格输入终端(terminal)

2.输入 git --version 出现2.1同样效果图



四:创建SSH

  Win:

    1).在桌面,鼠标右击点击Git Bash Here或者如果你知道要上传哪些内容可直接选中你要上传的文件夹并右击出现如图3.1,打开后效果是3.2

![img](https://i0.hdslb.com/bfs/article/9e060471091821a3b4f71fd4683099c16d2a3065.jpg@942w_599h_progressive.webp)

​				<!--3.1图中选中了我要想上传的文件夹(Scripts)-->

​	![img](https://i0.hdslb.com/bfs/article/b3d38c33e4bf6ecbede9b6d208cb308c95ad1eb2.png@942w_569h_progressive.webp)

​					<!--3.2选择文件夹打开后的效果-->


    2).打开控制器后输入 ssh-keygen -t rsa -C"此处输入你注册github时使用的邮箱"  然后连续敲击三下回车直到出现3.3

​			![img](https://i0.hdslb.com/bfs/article/e64afa857b1c0814d94b26783d676ca154ee9b21.png@942w_569h_progressive.webp)

​					<!--3.3效果图-->
  Mac:

    1).打开终端输入ssh-keygen -t rsa -C"此处输入你注册github时使用的邮箱"  然后连续敲击三下回车直到出现3.3



这里生成了私钥(id_rsa)和公钥(id_rsa.pub)请妥善保护自己的私钥防止代码被攻击外泄


这里补充一点(选看):

如果需要新的SSH公钥,请依次输入以下内容以删除旧文件

mkdir key_backup

cp id_rsa* key_backup

rm id_rsa*



五:在GitHub上设置公钥

1.打开自己的github网站,点击右上角图标,点击settings如图4.1

![img](https://i0.hdslb.com/bfs/article/9280f645b42f3974a27a5dbe69a64c33437ad3a0.png@621w_1569h_progressive.webp)

​							<!--4.1效果图-->
2.选择New SSH Key(4.2)

![img](https://i0.hdslb.com/bfs/article/44695c042841cc327096db110f7e6b008c8cffcd.png@942w_554h_progressive.webp)

​					<!--4.2效果图(有两个是我生成好的)-->
3.点进后效果图4.3

![img](https://i0.hdslb.com/bfs/article/8f3b1e21e9e84db33258b76d2ee912b42e82f907.png@942w_576h_progressive.webp)

​							<!--4.3效果图-->
4.这一步我们复制已经生成的公钥



方法一(win/mac):见图4.4

(每次输完一行敲回车)

先输入 cd ~/.ssh

再输入 ls

最后输入复制语句 cat id_rsa.pub

或者,

从图4.4的ssh-rsa开始一直到最后的邮箱全部选中并ctrl + c或command + c复制

​	![img](https://i0.hdslb.com/bfs/article/907e9415ce70dbcef39094fc95d3b9b3a45598b3.png@942w_279h_progressive.webp)

​					<!--4.4 注意符号 win是在"$"后面跟着打就行-->


注意:如果你使用了本文第四条(创建SSh)/win/1的方法,也就是右击文件夹然后选择的Git Bash Here,再使用了 cd ~/.ssh 会cd到另个目录下,见下面方法二 (后面也可以cd回来,所以方法二选看)



方法二:找到.ssh生成的文件打开复制

    Win:
    
        1).一般默认情况下,在图3.3我们可以看到生成的文件在 /c盘/Users/"你的电脑用户名"/.ssh/,这里会出现一个情况, .ssh是可能默认不显示的我们需要打开隐藏文件夹、拓展名,如4.5

​	![img](https://i0.hdslb.com/bfs/article/ba02364e739114befc2cf254f2b6871eb18386b4.png@942w_158h_progressive.webp)

​						<!--4.5 在“此电脑”上方打开-->
​       2).用记事本打开 id_rsa.pub 并 复制全部内容



    Mac:
    
      1).在终端(terminal)直接输入 open ~/.ssh 回车后会弹出文件夹
    
       2).打开 id_rsa.pub 并 复制全部内容



5:回到github上设置公钥

  1).如图4.3,将我们刚才复制的公钥内容粘贴到"Key"中

  2).最后点击"Add SSH Key"即可生成!(可回看图4.2成功生成的样式)



六:新建仓库

1.点击右上角自己的图像图标,选择"Your Repositories"进入后再选择右边一个绿色的"New"进入后如图5.1,根据指示完成创建

![img](https://i0.hdslb.com/bfs/article/d5b7ff4a93e14232b331899a334fae3a5441a8ba.png@942w_524h_progressive.webp)

​								<!--5.1效果图-->
2.如5.2创建完成

​	![img](https://i0.hdslb.com/bfs/article/b3fc19d41756f55341f04fddb7afd1edc532ffbc.png@942w_491h_progressive.webp)

​								<!--5.2 用的前天创建好的:-)-->
3.我们测试一下刚刚创建SSH是否与github连接成功

在打开的git bush here或是终端输入 ssh -T git@github.com 如效果图5.3

​	![img](https://i0.hdslb.com/bfs/article/3da3c2ad16f6dea43125fdaaa6294d5d24f7b121.png@942w_117h_progressive.webp)

​								<!--5.3 出现以上内容则成功-->


七:上传代码/项目

1.配置我们的github用户名和邮箱名

分别输入以下内容,输入完成敲回车

git config --global user.name"注册github时使用的用户名"

git config --global user.email "注册github时使用的邮箱名"

效果如6.1

![img](https://i0.hdslb.com/bfs/article/e315090554f0c54106bf975e60a75126366ea27c.png@942w_56h_progressive.webp)

​					<!--6.1 两行输入完就是如此(1.没有什么提示2.如图第二行引号我没加,其实可加可不加)-->
2.下面就添加项目到仓库了

  1)首先如果你在 五(设置GitHub公钥)/4/  输入了cd ~/.ssh 可见如图6.2黄色语句内容,是你定位到.SSH文件夹位置,则不是6.2所显示,我们需要重新定位到你所想要上传项目到文件夹



![img](https://i0.hdslb.com/bfs/article/58c47288f066d0d08e8f848fb292190ab4dcb834.png@942w_131h_progressive.webp)

​					<!--6.2演示(图中的user.name设置错误![6.1是正确的] 大家这一条主要看黄色语句内容)-->
  2)输入语句 cd 目标路径 效果如图6.3、6.4

注意:如果你的文件夹或文件名字带有空格,需要在空格处输入一个反斜杠 \ (不要删除那个空格)

​	![img](https://i0.hdslb.com/bfs/article/906c40f693d5ec115bf9e1a67e70abb13e40fbaa.png@756w_909h_progressive.webp)

​				<!--6.3 选中路径复制即可(win同理,右击文件夹-属性,然后复制路径)-->

![img](https://i0.hdslb.com/bfs/article/e57503355d1215e5dcf0707a2b30cc226a5928f4.png@942w_69h_progressive.webp)							<!--6.4发现定位改成你想改的即可(win同样看见黄色的路径已被改变)-->
  3)输入语句 git init 如图6.5

![img](https://i0.hdslb.com/bfs/article/d6d405b42399e3d860c4f8fbd3d317bac9d6fe36.png@942w_75h_progressive.webp)

​						<!--6.5效果图-->
  4)输入语句

git add . 

git commit -m"你想要注释的内容"

![img](https://i0.hdslb.com/bfs/article/d96f1c30bc69d3a68f02ecf8b86823a906b3c31f.png@942w_471h_progressive.webp)

​					<!--效果如6.6-->

6.6如图
继续输入语句

git remote add origin "项目地址" 

项目地址见6.7,效果如6.8

![img](https://i0.hdslb.com/bfs/article/b7920440b9dd739cf8de3a4cd3f60c7c7cfdb3c3.png@942w_774h_progressive.webp)

​			<!--6.7 最后三所表示的就是项目地址,点击即是复制-->

​	![img](https://i0.hdslb.com/bfs/article/7ad668ea123de895f66d28e49dfa85be488b5382.png@942w_50h_progressive.webp)

​			<!--6.8 输入完成后没有其他提示是正常的-->
注意:如果6.8有报错,则可能是你以前生成过远程仓库,输入语句 git remote rm origin 删除原仓库,再继续上述创建操作



最后输入语句

git push -u origin master 

将项目/代码移至master分支下,效果如6.9

![img](https://i0.hdslb.com/bfs/article/45d7d35cc43fb13f3928277814d335ff4544de02.png@942w_401h_progressive.webp)

​				<!--6.9 完成效果图-->
注意:如果不能push、有报错的话,可尝试输入语句 git pull rebase --"你的项目地址" 以更新项目



八:完成

刷新页面如图7.1所示完成上传(code/branch:master)

![img](https://i0.hdslb.com/bfs/article/7e92c492e3a41584c02b250b6937b8283658fd11.png@942w_492h_progressive.webp)

​			<!--7.1 完成!-->