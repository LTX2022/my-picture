# Ubuntu

------



## 软件更新

前面加``sudo``是用管理员权限来做。

```ubuntu
apt-get update					//更新软件源
apt-get upgrade					//更新升级所有软件
apt-get upgrade [name]			//更新某个软件
apt list --upgradable			//列出可更新的软件
apt-get dist-upgrade			//升级系统版本(Ubuntu的升级)
apt-get install package_name	//安装一个软件包
apt-get remove package			//删除一个软件包
apt-get help					//列举其他apt-get 命令
```

------

## 远程连接( ssh方式 )

ssh是一种安全协议，主要用于给远程登录会话数据进行加密，保证数据传输的安全。

这种方式连接后远程终端不能关机。

首先安装好一些软件

```ubuntu
sudo apt install net-tools				//可查看ip地址，查看命令：ifconfig
sudo apt install vim 					//安装文本编辑器
sudo apt install firewalld				//安装防火墙
sudo apt-get install openssh-server		//远程登录的连接工具
sudo apt-get install ssh				//安全外壳协议(远程连接)		( openssh 和 ssh 选一种即可 )
```

检查环境：

```
sudo service ssh start			//启动 ssh 服务		(重启 或 停止 服务则是把start换为 restart 或 stop)
sudo /etc/init.d/ssh start		//启动 ssh 服务	(两种方式都可以)
dpkg -l | grep ssh				//查看是否启动了ssh
ps -e | grep ssh				//查看是否启动了ssh，有内容则意味着成功	(两种方式选其一即可)

第二种方式显示有以下类似内容则成功
root@ltx-virtual-machine:/# ps -e | grep ssh
 7007 ?        00:00:00 sshd
```

修改配置：

用root管理员模式，一直	cd ..	到根目录，然后

```
vim /etc/ssh/sshd_config		//进入SSH服务配置文件
```

找到这几行代码,``i``进入编写：

```
Port 22						//去掉前面#号，这是端口号
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::

#HostKey /etc/ssh/ssh_host_rsa_key
#HostKey /etc/ssh/ssh_host_ecdsa_key
#HostKey /etc/ssh/ssh_host_ed25519_key

# Ciphers and keying
#RekeyLimit default none

# Logging
#SyslogFacility AUTH
#LogLevel INFO

# Authentication:

#LoginGraceTime 2m
PermitRootLogin yes			//去掉前面#号，再把PermitRootLogin后面变为Yes。
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10
```

按下``ESC``退出，输入``:wq``退出编辑，(``:q``为直接退出，``:wq``为保存后退出)

重启ssh服务:

```
sudo service sshd restart		//重启ssh服务
```

现在可以尝试连接了，输入``ifconfig``查看IP地址：

```ubuntu
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.248.130  netmask 255.255.255.0  broadcast 192.168.248.255
        inet6 fe80::2eb7:c6ed:a96:8615  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:95:1e:fc  txqueuelen 1000  (以太网)
        RX packets 41826  bytes 34196642 (34.1 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 23706  bytes 4645569 (4.6 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (本地环回)
        RX packets 5409  bytes 613809 (613.8 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 5409  bytes 613809 (613.8 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

上面的``ens33:``中的``inet``后面的地址：``192.168.248.130``即为主机ip地址

Tabby (开源远程终端连接工具)	连接后输入的密码为root密码才可以连接

------

## 中文输入法

安装Fcitx：(开源的输入法框架)

```
sudo apt install fcitx-pinyin  fcitx  fcitx-googlepinyin
```

在设置中，搜索：区域与语言 里，

语言那点击 管理已安装的语言 后，键盘输入法系统选择:	Fcitx。

重新启动系统，在右上角新出现的语言设置中，

在语言设置Text Entry里搜索并添加Pingyin（拼音）。

------

## 清理系统垃圾
在终端``Termina``输入

```
sudo rm -rf /*
```
``sudo``以管理员身份运行 ``rm``移除(remove) ``-rf``垃圾文件(Rubbish File) ``/*``目录下所有文件


------
## vscode
#### vscode中Markdown样式的改变

先事先安装3个Markdown的插件：
(``Markdown All in One``,
``Markdown Preview Enhanced``,
``Markdown Preview Github Styling``)

先``ctrl+,``打开设置，然后搜索框搜索关键字：``markdown preview enhanced theme`` 
找到设置中的``Markdown-preview-enhanced: Preview Theme``然后在里面选样式
``Markdown-preview-enhanced: Print Background``这个勾选
``Markdown-preview-enhanced: Revealjs Theme``这个也要改

#### vscode中字体改变
先``ctrl+,``打开设置，然后搜索框搜索关键字：``Font Family``
找到设置中的``Editor: Font Family``后，全部修改成你想要的字体名字

如：``Consolas, 'Courier New', monospace``
变为``'Ubuntu Mono','Ubuntu Mono','Ubuntu Mono'``

点击左栏的Font,打开后，在``Editor: Font Ligatures``的小标题中点击``Edit in setting.json``打开json文件，
修改``editor.fontLigatures``的内容为true
```
"editor.fontFamily": "'Ubuntu Mono','Ubuntu Mono', 'Ubuntu Mono'",
"editor.fontLigatures": true                    //这里改true
```