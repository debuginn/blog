---
title: "操作系统 文件管理 概述"
date: 2018-01-06T19:22:56+08:00
draft: false
author: "Meng小羽"
authorLink: "https://www.debuginn.cn"
authorEmail: "debuginn@icloud.com"
keywords: "os,introduction"
comment: true
weight: 0

tags: ["os", "system"]
categories: ["os"]

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: "https://image.debuginn.cn/202302221853276.jpg"
featuredImagePreview: "https://image.debuginn.cn/202302221853276.jpg"
---

> 计算机的主要功能之一就是对数据进行数值或非数值计算。系统软件必须提供数据存储、数据处理、数据管理的基本功能。数据管理是通过文件管理的方式来完成的，而目录又是建立在分区或卷的基础之上的。操作系统中文件和目录相关的子系统称之为文件系统。

计算机程序都要存储信息、检索信息。

1. 能够存储大量的信息。 
2. 长期保存信息。 
3. 可以共享信息。

**管理的内容**：文件的结构、命名、存取、使用、保护和实现方法。

**透明存取**：指的是不必要了解文件存放的物理机制和查找方法，只需要给定一个代表某段程序或数据的文件名称，文件系统就会自动的完成对与给定文件名称相对应的文件的有关操作。

## 文件和文件系统

### 文件的定义

文件：一组带标识的、逻辑上有完整意义的信息项的序列。

标识：文件名。

信息项：文件内容的基本单位。一组有序序列。

读写指针：用来记录文件当前的读取位置，它向下一个将要读取的信息项。

写指针：用来记录文件当前的写入位置，下一个将要写入的信息项被写到该处。

文件的长度：是单字节或多字节，这些字节可以是字符，也可以组成记录。

UFS：可达255个字符。

FAT12（MS-DOS所使用的文件系统）命名规则规定文件名为8个字符。

NTFS：达到255个字符。

EXT2：chap5, htm, Chap5等文件名称。

### 文件系统

文件系统：操作系统中统一管理信息资源的一种软件。

从用户的角度来看，文件系统负责为用户建立文件、读写文件、修改文件、复制文件和撤销文件。文件系统还负责完成对文件的按名存取和对文件进行存取控制。

## 文件分类

### 按文件的用途分类

#### 系统文件

操作系统和各种系统应用程序和数据所组成的文件。

不允许对该类文件进行读写或修改。

#### 库函数文件

标准子程序及常用应用程序组成的文件。允许用户对其进行读取、执行，但不允许对其进行修改。C语言子程序库。

#### 用户文件

用户文件是用户委托文件系统保护的文件。可以由源程序、目标程序、用户数据文件、用户数据库等组成。

### 按文件的组织形式分类

#### 普通文件

指文件的组织格式为文件系统中所规定的最一般格式的文件。普通文件即包括用户文件、库函数文件和用户实用程序文件等。

#### 目录文件

有文件的目录构成的特殊文件。含有文件目录信息的一种特定文件。主要用来检索文件的目录信息。

#### 特殊文件

把特殊文件的操作转成为对应设备的操作。

### 一些常见的文件分类方式

- 按文件的保护方式可划分为：只读文件、读写文件、可执行文件、无保护文件等 
- 按信息的流向分类可划分为：输入文件、输出文件和输入输出文件
- 按文件的存放时限可划分为：临时文件、永久文件和档案文件
- 按文件所使用的介质类型可划分为：磁盘文件、磁带文件、卡片文件和打印文件
- 按文件的组织结构分类：顺序文件、链接文件和索引文件。

### UNIX类操作系统中文件的分类

- 普通文件 
- 目录文件 
- 特殊文件

### 文件系统的功能

1. 统一管理文件的存储空间，实施存储空间的分配和回收。 
2. 实现文件从名字空间到外存地址空间的映射。 
3. 实现文件信息的共享，并提出文件的保护和保密措施。 
4. 向用户提供一个方便使用的接口。 
5. 系统维护及向用户提供有关信息。 
6. 保持文件系统的执行效率。 
7. 提供与I/O的统一接口。