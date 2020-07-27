---
title: Laradock 部署 Laravel
date: 2017-10-25 00:41:18
categories: Docker
tags:
- Laravel
- Docker
- Laradock
---

最近在部署 `Laravel` 的时候发现有一款Docker很好用，那就是 [Laraldock](http://laradock.io) 。

在使用的过程中有几个坑要注意一下。

初始化自己的.env的时候一定要记得修改APPLICATION，这个目录指向的是/var/www/目录。

如果你需要找到你的 Mysql 信息，在.env里搜索 Mysql 就能找到相关信息。

刚开始用的时候根本不知道怎么进入 Mysql 命令模式，进入的方法是：

<!-- more -->

`docker-compose exec mysql mysql -uroot -p`

并且在初始化 Laravel Composer 时发现直接在本地目录运行PHP是不行地，在修改 APPLICATION 目录后。

执行

`docker-compose exec workspace bash`

进入到 wordspace 的 Bash 模式。

Nginx 方面，如果你需要部署多个实例，你只需要在`../nginx/sites/`目录下创建新的 .conf

root 指向的目录是 /var/www/ 这个 /var/www/ 等于你之前设置的 APPLICATION 目录

如果你的APPLICATION目录下有`/www1/public` 那么在这里设置为 `/var/www/www1/public;`

就能顺利的访问到你的 Laravel 程序。

总体来说，这款 Docker 应用太方便了，避免了在本地和服务器上服务器应用造成的部署困难问题~

推荐大家使用哦~