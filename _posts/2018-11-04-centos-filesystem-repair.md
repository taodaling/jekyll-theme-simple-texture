---
categories: experiement
layout: post
---

最近使用VirtualBox运行centos7的时候启动报错，查看日志是文件系统与内存数据冲突。

解决方法是:

umount /dev/mapper/centos-root

xfs_repair -L /dev/mapper/centos-root
