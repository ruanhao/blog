---
title: 在终端下分析日志文件
comments: true
date: 2017-08-06T19:59:36+08:00
categories: linux
---

## 常用方法

在桌面环境下，分析日志文件一般使用 TextEdit ，Sublime 等文本编辑工具，当需要查找特定的关键词时，Ctrl-F 或者 Command-F 即可。
但还是有很多内容需要用肉眼过滤，很容易看花眼。

在 Linux 上进行开发的话，会使用 grep 过滤出感兴趣的内容。比如：

- 过滤某个关键词:
  ``` sh
  grep -i 'keyword' xxx.log
  ```
  
- 过滤多个关键词:
  ``` sh
  grep 'kw1\|kw2' xxx.log
  ```
  
- 反向过滤:
  ``` sh
  grep -v 'nokw' xxx.log
  ```

- 将空行过滤掉:
  ``` sh
  grep . xxx.log
  ```

grep 功能很强，也是我最喜欢的命令之一。尽可能使用高亮模式 `grep --color=auto`，提升视觉享受。

## 问题所在

很多时候分析日志文件时，需要查找某一行同时出现多个关键词，
比如需要找出等级为 INFO ，且同时出现 "exception" 关键词的日志，
一般情况下会将命令写成这样：

``` sh
grep 'INFO' xxx.log | grep 'exception'
```

但这样关键词多起来，就要写很多管道了，看上去也不直观。

## 解决方法

可以 source [这个脚本](https://gist.github.com/ruanhao/e94d426f715dcc22e1405b8462685354)，将命令写成：

``` sh
m -p 'INFO' -p 'exception' xxx.log
```

这样就能同时搜索多个关键词了，不过如果还能高亮那就再好不过了：

``` sh
m -p 'INFO' -p 'exception' xxx.log | h 'INFO' 'exception'
```

效果如下：

![](/images/tech/browse-log-under-term.png)

其实在终端下分析日志还是很开心的。
