---
title: RPM 概述
date: 2021-05-18 19:40:52
summary: RPM 概述
categories: Linux
tags:
- rpm   

---
## RPM 概述

### 什么是 RPM

RPM是RedHat Package Manager（RedHat软件包管理工具）类似Windows里面的“添加/删除程序”

### Linux软件安装方式

- 通用二进制格式：直接解压压缩文件，就可以使用。但一定要注意安装平台。
- 软件包管理器：如RPM。
- 软件包管理器的前端工具：如YUM。
- 源代码编译。



### 软件包管理工具的特性

- 文件清单
- 文件放置路径
- 提供的功能说明
- 依赖关系 
- 软件包管理器内部有一个数据库，其中记载着程序的基本信息，校验信息，程序路径信息等。

### RPM

RPM早期被称为RedHat Package Manager，但由于目前RPM非常流行，且已经成为Linux工业标准。所以RPM现在又被称为RPM is Package Manager。

RPM管理支持事务机制。增强了程序安装卸载的管理。

RPM的功能：打包、安装、查询、升级、卸载、校验、数据库管理。


### YUM

YUM被称为 Yellow dog Updater, Modified，是一个在Fedora和RedHat以及SUSE中的Shell前端软件包管理器。YUM使用Python语言写成。YUM客户端基于RPM包进行管理，可以通过HTTP服务器下载、FTP服务器下载、本地软件池的等方式获得软件包，可以从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系。

YUM在安装RPM时，会从服务器下载相应包，且缓存在本地。

使用YUM进行RPM包的管理，非常简单方便。