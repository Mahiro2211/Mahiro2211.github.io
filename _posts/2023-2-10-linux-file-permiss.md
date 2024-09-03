---
layout: post
title: 【Linux】文件访问控制列表（ACL）
categories: [Linux]
description: 用户，用户组，管理员,文件权限与归属
keywords: Linux
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

# 目录
1. [用户，用户组，管理员](#用户用户组管理员)
   - [添加，删除，修改用户的属性](#添加删除修改用户的属性)
   - [id命令（用来查询一个用户的uid，gid和判断用户是否存在）](#id命令用来查询一个用户的uidgid和判断用户是否存在)
   - [useradd，groupadd（用来创建一个新的用户账户，创建一个新的用户组账户）](#useraddgroupadd用来创建一个新的用户账户创建一个新的用户组账户)
   - [usermod（用来修改一个用户的属性）](#usermodusermod用来修改一个用户的属性)
   - [passwd命令（修改用户密码，过期时间，password的缩写）](#passwd命令修改用户密码过期时间password的缩写)
   - [userdel（删除用户）](#userdel删除用户)
2. [文件权限与归属](#文件权限与归属)
   - [如何修改文件权限和所属](#如何修改文件权限和所属)
   - [chgrp命令（change group改变文件所属的用户组）](#chgrp命令change-group改变文件所属的用户组)
   - [chown（change owner 改变文件的拥有者）](#chownchange-owner改变文件的拥有者)
   - [chmod(change modify 改变文件的权限）](#chmodchange-modify改变文件的权限)
   - [默认权限](#默认权限)
   - [隐藏权限](#隐藏权限)
     - [SUID（让执行者临时拥有所有者的权限）](#suid让执行者临时拥有所有者的权限)
     - [SGID（让执行者临时用所有组的权限）](#sgid让执行者临时用所有组的权限)
     - [SBIT（特殊权限位—保护位）](#sbit特殊权限位保护位)
   - [隐藏权限的总结](#隐藏权限的总结)
   - [隐藏权限的指令](#隐藏权限的指令)
     - [chattr命令（change attributes设置某个文件的隐藏权限）](#chattr命令change-attributes设置某个文件的隐藏权限)
     - [lsattr命令（list attributes查看某个文件的隐藏权限）](#lsattr命令list-attributes查看某个文件的隐藏权限)


#  用户，用户组，管理员
## 添加，删除，修改用户的属性
~~在Linux中每个用户都是有自己的UID(user IDentification)，在这里，管理员（root）的uid为0，系统用户为1-999，普通用户从1000开始~~
~~同样的我们用GID（group IDentification）用来表示用户组，为了方便管理一个组里的所有用户~~
### id命令（用来查询一个用户的uid，gid和判断用户是否存在）
```shell
id user_name
```
### useradd，groupadd（用来创建一个新的用户账户，创建一个新的用户组账户）
```shell
useradd [参数] user_name
groupadd [参数] group_name
```
**常用的参数**
* -d /home/user_name可以用来指定用户的家的目录的路径
* -u number 用来指定创建用户的UID
* -s /bin/bash(也可以识别的路径)用来指定用户的默认shell解释器
**tips：**
```shell
useradd -d /home/linuxprobe -u 8888 -s /sbin/nologin linuxdown
```
一旦我们把默认的shell解释器设置成了以上路径，那么就代表着这个用户不能再登录到系统中
### usermod（用来修改一个用户的属性）
```shell
usermod [参数] user_name
```
**常用的参数**
* -c 填写用户账户的备注信息
* -G 变更拓展用户组
```shell
usermod -G group user
将user 加入到 group
```
* -L 禁止用户登录系统（lock
* -U 解锁用户登陆系统（unlock
* -s 变更默认终端
* -u 修改用户的uid
别看他参数还有很多，其实多用就熟练了

### passwd命令（修改用户密码，过期时间，password的缩写）
如果只是单纯的修改你目前登录用户的密码只需要单纯的输入passwd就可以了
如果要修改其他人的密码就需要添加用户的名字
```shell
passwd username
```
### userdel（删除用户）
```shell
userdel -f username 
#-f参数表示强制删除user
userdel -r username
#-r参数表示删除用户以及家的目录
```
# 文件权限与归属
## 如何修改文件权限和所属
这里有一个很重要的参数就是-R，加上这个参数的意思就是递归这个文件的子目录，让他的子目录都执行这个操作
### chgrp命令（change group改变文件所属的用户组）
```shell
chgrp user filename/dirname
#将文件或者目录的所有人改成user这个用户组
```
### chown（change ownner 改变文件的拥有者）
```shell
chown user filename/dirname
#将文件的所有者改为user
chown user:group filename/dirname
#他也可以单纯的修改用户所属的用户组
```
### chmod(change modify 改变文件的权限）
#### 修改非隐藏权限
```shell
chmod [-R] xyz(数字权限） filename/dirname
```
#### 符号类型修改文件权限
```shell
chmod u=rwx go=rx .bashc（文件名)
#给user用户rwx的权利，其他人只有rx权利，对于.bashc文件来说
chmod a-x .bashc
#给所有人去掉可以执行.bashc的权力
```
a就是所有的身份，u就是user所有者，g是用户组，o是普通用户（others）
## 默认权限

> 在Linux中，每个文件都有他的所有者和所属组，对于不同身份的用户，对文件的权限也通常是不一样的，在Linux中非隐藏权限通常有这三种 r（可读）w（可写）x（可执行），他们也对应着相应的数字表示法 r = 4 ，w = 2 , x = 1
> 现在来说用户种类，用户种类分三种，管理员，系统用户，普通用户
> 如果现在告诉你某一个文件的权限是741，就表示管理员有4+2+1=7有可读可写可执行三种权限，系统用户只有可读的权限，普通用户只有可执行的权限

## 隐藏权限
### SUID（让执行者临时拥有所有者的权限）
以passwd命令为例
```shell
[root@localhost ~]#  ls -l /bin/passwd
-rwsr-xr-x. 1 root root 34512 Aug 12  2018 /bin/passwd
```
我们发现这里root用户的权限由rwx变成了rws（但是仅限于passwd这个命令，因为user给了他一个SUID权限位）
### SGID（让执行者临时用所有组的权限）
SGID的第一个功能是模仿SUID的让临时性的让执行者拥有执行组的权限
第二个功能，创建了一个共享目录，在里面创建的任何文件都会属于这个目录的所属组，而不是创建这个文件的用户的基本用户组，也就是**创建的文件自动继承该目录的用户组**
### SBIT（特殊权限位—保护位）
当一个目录被设置成SBIT的特殊权限位的时候，原本文件的普通用户（others）的可执行权限x会变成t或者T（原本有x'的会被写成小写，原先没有的会被变成大写T）
**当然文件能否被删除不能却决于是否有rwx权限，而是这个文件在那个用户的目录下，在谁的目录下，也就说明哪个用户对这个文件所在的目录下有完整的rwx权限，也就是说我虽然打不开这个文件，但是因为我在这个目录下有完整的权限，我可以把它删了，就像你有个保险箱，里头有钱，你打不开，你也可以把它扔了**

### 隐藏权限的总结
这三种隐藏权限分别针对的是user，group，others设置的具体方法就是他们开哦头的第一个字母加号（+）就是设置权限，减号（-）就是取消权限

### 隐藏权限的指令
#### chattr命令（change attributes设置某个文件的隐藏权限）
```shell
chattr [参数] username
```
参数丰富，用加号或者减号来表示设置权限或者曲线权限
### lsattr命令（list attributes查看某个文件的隐藏权限）
参数跟ls命令有很多相同之处