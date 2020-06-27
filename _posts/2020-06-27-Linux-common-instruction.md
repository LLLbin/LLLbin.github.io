---
layout:     post                    
title:      Linux - 常用指令           
subtitle:   那些Linux上常用的指令笔记
date:       2020-06-27              
author:     Lebin                     
header-img: img/post-bg-Vimrc.jpg
catalog: true                       
tags:                               
    - Linux
---

# 那些常用的Linux指令🍖

## 目录操作
```
mkdir: 创建目录
　　-p : 递归的创建目录 也就是可以创建多层目录
　　一次创建多个目录： mkdir {a,b,c,d,e,f}
　　一次创建 a b c d e f多个目录。

rmdir：删除一个空文件夹

cp：复制文件或者文件夹
　　-a =-pdr
　　-p 同时复制文件属性，比如修改日期
　　-d 复制时保留文件链接
　　-r: 复制文件夹时,递归复制子文件夹
　　-l 不复制，而是创建指向源文件的链接文件，链接文件名由目标文件给出。   
　　note:可以在拷贝的同时重命名

mv：移动文件或者文件夹，可以在移动的时候重命名

rm ：删除文件或者文件夹
　　-r：递归删除
　　-f：强制删除 即没有提醒
```

## 文件处理命令
```
ls :查看文件
　　-l 以列表形式查看
　　-h 以一种人性化的方式查看，也是文件的大小以合适的单位显示
　　-a 查看所有文件，包括隐藏文件
　　-i 显示出文件的i节点号
　　-ltr 这个好用

touch 文件名：创建文件 可以一次创建多个文件，以空格隔开

cat :查看文件内容 
　　-n:带行号

tac:反向显示文件内容

more：分页查看文件内容
　　进入浏览模式后：
　　f或者空格：下一页
　　enter：一行一行往下翻
　　q:退出

less:查看文件内容 
　　空格翻页
　　回车换行
　　pageup：上一页
　　pagedown：下一页
　　上箭头：向上翻
　　下箭头：向下翻
　　/搜索词 n向下找

head -n 文件名 :查看文件前n行。缺省-n显示前10行
tail -n 文件名 ：查看文件的末尾几行
　　-f :动态显示文件末尾内容

ln:链接命令
　　-s创建软连接
　　硬链接和cp -p的区别是硬链接会同步更新
　　源文件如果丢失，硬链接依然存在。
　　硬链接和源文件的i节点相同。
　　硬链接不能夸分区，软连接可以跨分区。
　　硬链接不可以链接目录，链接可以
　　软连接文件具有的权限是ugo都是rwx
```

## 权限管理命令
```
chmod:修改文件或目录的权限，只有root和所有者可以更改
　　[{ugoa}{+-=}{rwx}] [文件或目录] 
　　[mode=421] [文件或目录]
　　-R 递归修改
　　权限的数字表示：rwx：000~111 
　　r->4
　　w->2
　　x->1

　　例：chmod u+x a.txt
　　　　chmod g+w,o-r a.txt //同时做多个权限的修改
　　　　chmod g=rwx a.txt
　　　　chmod 640 a.txt
　　　　chmod -R 777 testdir //把目录和下面所有文件的权限

　　　　　　　　　　　　     针对文件　　 　　　　 针对目录
　　　　r　　 读权限 　　 可以查看文件内容　　  可以列出目录中的内容
　　　　w 　  写权限 　　 可以修改文件内容 　　 可以在目录中创建、删除文件
　　　　x 　  执行权限    可以执行文件 　　　　 可以进入目录

chown:更改文件所有者，只有root可以更改
　　chown root a.txt//把a.txt更改为root所有

chgrp:更改所属组
　　chgrp lambrother fengjie //把fengjie的所属组更改为lambrother

umask -S:查看创建文件的缺省权限，即默认权限
umask 023:修改文件的缺省权限为777-023=754。即rwxr-xr--
```

## 文件搜索命令
```
find:搜索制定范围内的文件
　　find [搜索范围] [匹配条件]
　　-name 按文件名搜索
　　-iname 根据文件名查找，不区分大小写
　　-size +n大于 -n小于 n等于 这个n是数据块，在Linux中一个数据块是512字节大小
　　-user 根据所有者查找
　　-group 根据所属组查找
    -maxdepth 最深路径
　　根据文件属性查找：
　　　　-amin 访问时间 access
　　　　-cmin 根据文件属性被修改的时间 change
　　　　-mmin 根据文件内容被修改的时间 modify
　　例： find /etc -cmin -5 :查找/etc目录下五分钟内被修改过属性的文件和目录

　　-a 两个条件同时满足
　　　　find /etc -size +10 -a -size -50
　　-o 两个条件满足一个即可

　　-type 
　　　　f 文件， d 目录， l 软连接文件， s 套接字文件， b 块设备文件， c 字符设备文件， p 命名管道文件

　　-inum 根据i节点查找

　　对找到的结果进行操作
　　　　-exec或者-ok 命令 {} \;
　　　　例如：
　　　　　　find /etc -name init* -exec ls -l {} \; 对找到的文件名按列表查看
       -|xargs 命令
        例如： 
           find ./ type d|xargs ls -l
       -|grep "查找字符"
        例如： 
           find ./ -type f|grep ".txt"

　　find /etc -name init :搜索目录/etc下面所有的init文件，精确匹配，包括子目录中的init文件
　　find / -size +204800 搜索大于100M的文件

locate:(查找速度非常快，因为它维护了一个文件库。缺点就是新建立的文件没有很快收录到文件库)
　　locate 文件名
　　updatedb 更新locate的文件资料库 文件资料库不收录/tmp下的文件
　　-i 不区分大小写

which :查找命令的目录以及别名
　　which 命令

whereis :搜索命令所在目录及帮助文档路径。

grep:在文件中搜寻字符串匹配的行并输出，多个文件以空格隔开。
　　-i 不区分大小写
　　-v 排除指定字符串
　　-E 以正则表达式的方式搜索
　　-F 以普通文本的方式搜索
　　-n 显示搜索到的内容在文件中的行号。
```

## 帮助命令
```
man：查看命令或者配置文件的帮助信息
　　man 命令/配置文件
　　在手册里面，可以输入/要查找的str
　　man ls
　　man services
　　man fstab //直接输入配置文件的名字，而不需要使用绝对路径 重点查看name选项和配置文件的格式。
　　如果一个命令即使命令又是配置文件，那么可以使用一个序号进行区分，比如：
　　man 1 passwd 查看命令passwd的帮助
　　man 5 passwd 查看配置文件passwd的帮助

whatis 命令：得到命令的简要信息

apropos 配置文件名：查看配置文件的简短信息

命令 --help：查看命令的选项。

help 命令：查看shell内置命令的帮助信息。 shell内置命令是没有命令路径。不能使用man查看帮助。
```

## 用户管理命令
```
useradd: 添加用户
　　useradd 用户名

passwd: 修改用户密码
　　passwd 用户名 不加用户名直接更改自己的密码

who:查看当前的账户 显示的格式为： 登录用户名 登录终端（tty:本地登录 pts:远程终端） 登录时间 ip地址

　　w:查看更详细的用户登录信息。
```

## 压缩解压缩命令
```
.gz格式
　　压缩：gzip 文件名 只能压缩文件不能压缩目录，压缩完源文件也不见了
　　解压缩：gunzip/gzip -d 压缩包名称

tar:
　　-zcvf 压缩后文件名 打包的目录 :生成.tar.gz文件 注：这个命令先用tar归档，然后把归档的包压缩成.gz
　　-zxvf 要解压的文件名 ：解压缩.tar.bz2的文件

　　-jcvf 压缩后的文件名 打包的目录：生成.tar.bz2 注：这个命令先用tar归档，然后把归档的包压缩成.bz2
　　-jxvf 要解压的文件名 :解压.tar.bz2的文件

zip:
　　zip -r 压缩生成的文件名 要压缩的目录
　　zip 压缩生成的文件名 要压缩的文件。

unzip:
　　unzip 要解压缩的文件

bzip2:
　　bzip2 -k 要压缩的文件名 -k选项：保留源文件
　　bunzip2 -k 要解压的文件名 -k选项：保留压缩包
```

## 网络命令
```
write:给在线用户发送信息，用户不在线不行。以Ctrl+D保存
　　write 用户名

wall:给所有用户名发送信息
　　wall 要发送的信息

ping:测试网络连通性

　　ping ip地址 
　　-c 要ping的次数

ifconfig:
　　直接回车查看当前网卡信息
　　ifconfig 网卡名 ip地址 临时修改网络ip
　　　　ifconfig th0:0 192.168.1.100 netmask 255.255.255.0
　　　　　　给th0这个网卡新添加一个ip
　　　　ifconfig eth0:0 down
　　　　ifconfig eth0:0 up
ifdown th0
　　禁用th0这块网卡

ifup th0
　　开启th0这块网卡

mail:邮件命令
　　mail 要发送的用户名
　　mail 直接回车：查看命令
　　　　help :查看支持的命令格式
　　　　输入序列号：查看邮件详细内容
　　　　h: 回到邮件列表
　　　　d 序列号：删除序列号对应的邮件

last:统计计算机所有用户登录的时间信息，以及重启信息
lastlog:所有用户最后一次登录的时间
　　-u 用户的uid 查看指定用户的登录信息。

traceroute:显示数据包到主机间的路径
　　traceroute 要探测的地址.
　　-n 使用ip而不使用域名

nslookup www.baidu.com
　　查看百度的ip地址

netstat:显示网络相关信息
　　-t :tcp协议
　　-u :udp协议
　　-l:监听
　　-r:路由
　　-n:显示ip地址和端口号

　　netstat -tlun:查看本机监听的端口
　　netstat -an:查看所有的监听信息
　　netstat -rn ：查看路由表，即网管

wget 文件地址
下载文件

service network restart:重启网络服务。

telnet 域名或ip
　　远程管理与端口探测
　　如： telnet 192.168.2.3:80
　　　　探测192.168.2.3是否开启了80端口

mount:挂在命令
　　mount -t iso9660 /dev/sr0 /mnt/cdrom :把sr0挂在到cdrom
```

## 关机重启命令
```
shutdown:这个关机命令更安全一些，不推荐使用其他关机命令。
　　-h：关机
        shutdown -h now shutdown -h 20:30
　　-r: 重启 
        shutdown -r now 
　　-c: 取消上次的关机命令
        shutdown -c 
    -k：不是真关机，只是发出告警信息，表示要关
        shutdown -c 
重启：
　　init 6
　　reboot

关机：
　　init 0
　　poweroff
　　halt

　　系统运行级别：
　　　　0 关机
　　　　1 单用户 类似windows安全模式
　　　　2 不完全多用户，不含nfs服务
　　　　3 完全多用户
　　　　4 未分配
　　　　5 图形界面
　　　　6 重启
　　可以通过查看/etc/inittab来查看系统启动的运行级别
　　runlevel:查看当前的运行级别
　　init n:设置系统运行级别

logout:退出当前用户，返回到登录界面
```

## 其他小技巧
```
\命令名字: 使用原始的命令
　　比如：
　　　　ls 实际上是ls --color auto
　　　　\ls 就是原始的ls

./: 当前文件夹

../：上一个文件夹
```

## 源码包安装过程
```
1. 安装注意事项
　　源代码保存位置：/usr/local/src/
　　软件安装位置： /usr/local/
　　如何确定安装过程报错：
　　安装过程停止并出现error、warning或no的提示  
2. 源码包安装过程
　　1)下载源码包
　　2)解压缩下载的源码包
　　3)cd dir 进入解压缩目录 注：里面有个INSTALL是系统安装的步骤说明
　　4)./configure 软件配置与检查
　　　　定义需要的功能选项
　　　　检测系统环境是否符合安装要求
　　　　把定义好的功能选项和检测系统环境的信息都写入Makefile文件，用于后续的编辑。
　　　　./configure --prefix=/usr/local/apache2 ：定义安装位置 
　　5)make :编译
　　　　　　如果前面有错误，则使用make clean命令清楚编译产生的临时文件
　　6)sudo make install: 编译安装
3. 源码包的卸载
　　(sudo make distclean: 删除卸载软件)
　　直接删除安装目录也ok，不会遗留任何垃圾文件。
```

## 其他命令
```
du -sh 文件名: 查看当前文件夹大小

ps 静态查看系统进程，系统默认安装
　　ps -aux 使用BSD语法查看所有进程
　　ps -ef 标准语法查看所有进程
　　　　UID 程序被该 UID 所拥有
　　　　PID 就是这个程序的 ID 
　　　　PPID 则是其上级父程序的ID
　　　　C CPU 使用的资源百分比
　　　　STIME 系统启动时间
　　　　TTY 登入者的终端机位置
　　　　TIME 使用掉的 CPU 时间。
　　　　CMD 所下达的指令为何
　　ps -aux --sort -pcpu,-pmem
　　　　根据CPU占用情况和内存占用情况来显示进程
　　watch -n 1 'ps -aux --sort -pcpu,-pmem'
　　　　每隔1秒监控一次进程情况

top 动态查看系统的状态

lsof -Pti :8000
　　通过端口号获得进程pid

kill -9 pid
　　杀死指定pid的进程，强行杀死。

history
　　查看历史命令

执行历史命令
　　!! 执行上一条命令
　　!n 执行历史命令的中第n条
　　!-n 执行导数第n条
　　!string 执行以string开头的历史命令行
　　!?string? 执行包含string的历史命令行

alias 
　　给命令起别名

　　alias 命令='别名'
　　alias -p 查看已存在的别名
　　
　　在~/.bashrc中添加
　　alias 命令='别名'
　　执行source .bashrc保存

unlias 
　　取消别名
　　unlias name

cal 
　　查看某一年的日历，可以是1-9999中的任意一年
　　cal 88

zcat
　　查看压缩包中的内容

sed -i 's#old#new#g' 文件名
　　使用new替换文件中的old

ssh root@192.168.8.15 "ifconfig"
　　远程执行命令

bash -x 脚本名
　　调试脚本
```

参考链接：[Linux常用命令笔记大全](https://www.jianshu.com/p/6ea061f74d84)



