---
title: Madagascar输出程序运行日志文件
date: 2017-01-12
author: Lloyd
categories: ["地震学软件"]
tags: ["Madagascar"]
slug: rsf-print-log
---

## 为什么要输出日志文件？
输出日志文件的目的就是保存整个程序执行过程的所有状态信息，以便未来排错或者对结果进行分析的一种手段。

程序运行时有些信息如程序运行时间，剩余运行时间等一般情况下需要打印在屏幕上，以便使用者能预估程序运行时间。另外一些信息如具体的参数信息，某个变量的输出值等信息可以既打印在屏幕上，同时又输出到日志文件中，方便以后对其进行分析处理。

<!-- more -->

## 命令行下处理
命令行下执行Madagascar命令并将输出信息打印至日志文件的方式比较简单，可采用如下方式
```
sfmycode < in1.rsf out1=out1.rsf 2> log.txt
```
`2> log.txt`　表示将程序里的其他输出语句如`printf(stderr,"t= \n",t)`,`sf_warning()`等输出到日志文件中。

## SCons脚本处理
当采用SCons脚本来运行程序并打印相关的数据至日志文件的时候,需要进行如下处理。

```
Flow('out1 ','in1.rsf',
    '''
    sfmycode
    out=${TARGET[0]}
    2> "log.txt"
    ''',stdout=-1)
```
`stdout=-1`表示标准输出设备未指向任何文件指针。

注意：文件输出的选择，此时不要选择将stdout作为文件的输出指针(即在源代码中不要出现像`Fout = sf_output("out")`这样的语句)，这样的话就无法完成分别输出不同信息至屏幕和日志文件的目的。
