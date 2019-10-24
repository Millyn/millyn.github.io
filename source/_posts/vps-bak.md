---
title: CentOS 6.5自动备份网站及数据库并发送到邮箱
slug: centos_auto_bak_data
date: 2015-07-22 23:54:36
categories: Linux
tags: 
- VPS
- 备份
- 主机
---

今天看鸟哥的linux书，提到了备份。我突然一想，我的VPS全没搞备份过程。

后悔莫及，回家后赶紧开始干。

VPS方面我只想公司的网站数据，这Wordpress我想直接用插件解决得了。

本文原始脚本来源于—– [点这里] 我做了少量修改
一个是我按原始脚本执行发送邮件命令时出错。最后补充完整的目录格式即可.
二个是最后删除.tar.gz文件时我在shell里操作会提示要我输入Y or N ，我追加了一个-f的命令直接删除。

具体脚本如下

<!-- more -->

```
#!/bin/bash
# 进入到备份文件夹
cd /home/backup

# 创建存放备份文件和数据库的文件夹，并修改权限为777
mkdir -m 777 -p ./backup$(date +"%Y%m%d")

# 将需要备份的文件复制到备份文件夹内
cp -r ../wwwroot/你的域名.com ./backup$(date +"%Y%m%d")/文件夹名.com

# 导出数据库到备份文件夹内
/usr/local/mysql/bin/mysqldump -u数据库帐号 -p数据库密码 数据库名 > ./backup$(date +"%Y%m%d")/备份文件名.sql

# 压缩存放备份文件和数据库的文件夹
tar zcvf ./backup$(date +"%Y%m%d").tar.gz ./backup$(date +"%Y%m%d")

# 以附件形式发送压缩包到指定邮箱
echo -e "网站数据备份" | mutt -s "文件内容" -a /home/backup/backup$(date +"%Y%m%d").tar.gz -- your@qq.com

# 删除备份文件夹与压缩包
rm -rf ./backup$(date +"%Y%m%d")
rm -f ./backup$(date +"%Y%m%d").tar.gz
```


脚本写好之后原始是说要放入Root文件夹，我觉得没必要，还是放在home里。
最后执行
crontab -e 添加定时任务在里面添加

`0 3 * * * /home/backup.sh`
到这一步自动备份+发邮件的过程已经结束了。感兴趣的同学可以直接sh backup.sh 试试是否成功。

收到邮件之后会很疑惑，发件人哪一块是空白的，有时候会显示你的主机名和主机用户例如向我的会显示root@xxxx这样的发件人。

不好看，也不方便邮箱管理，会被误认为是垃圾邮件。

再来修改一次配置

执行

`vi /etc/Muttrc`
找到任意位置添加

```
set from="admin@admin.com"
set use_from=yes
set envelope_from="yes"
set realname="admin"
```

admin@admin.com 为发件邮箱地址
admin 为发件人名字
修改为自己的吧~使自己好判断~
整个流程完毕!