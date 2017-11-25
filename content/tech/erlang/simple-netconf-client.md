---
title: 使用 Erlang 实现 Netconf Client
date: 2017-11-19T13:13:12+08:00
categories: erlang
---

## 前言

公司的项目通过 [Netconf 协议](https://en.wikipedia.org/wiki/NETCONF) 对网络设备进行配置与管理。
基于该协议的操作是通过 [Opendaylight](https://www.opendaylight.org/) 这个开源平台来实现的。
该平台涉及 SDN 领域的方方面面，也支持多种设备管理协议，Netconf 就是其中的一种。
实际上我们只是使用了处理 Netconf 协议的功能，即仅仅将 ODL 当做了一个 Netconf client 。
考虑到在 Docker 和 AWS 上的部署与运维，这样一来，使用 ODL 的成本就很高了，
因为整个平台不仅体积庞大，而且占用大量内存。

## 替代方案

[Erlang](http://www.erlang.org) 的标准库中包含了
[Erlang Common Test](http://erlang.org/doc/man/common_test.html) 这个框架，
而其中又提供了一个用于测试 Netconf 协议的测试套件 [ct_netconfc](http://erlang.org/doc/man/ct_netconfc.html) 。

从文档上看，ct_netconfc 实现了 [RFC 4741](https://tools.ietf.org/html/rfc4741) 和
[RFC 4742](https://tools.ietf.org/html/rfc4742)，这正是一个具备基本功能的 Netconf client 的标准，
可以直接使用这个库实现 Netconf 协议的编解码，而不用造轮子了。

## Talk is cheap, show me the code

ct_netconfc 是 Common Test 框架中的一个功能子集，无法直接当做库来使用，
需要做一些重构：

- 将协议编解码函数从 ct_netconfc 模块中提出
- 删除 RFC 4742 实现，使用 TCP 传输即可
- 使用 [gen_server](http://erlang.org/doc/man/gen_server.html) 对 Netconf session 进行抽象

#### 模块之间通信

![](/images/tech/snc-sequence.png)

#### 项目代码

[simple netconf client](https://github.com/ruanhao/simple-netconf-client)

## 后记

之前看过基于 Python 或 Java 的开源 Netconf client 实现，使用了面向对象的设计思想，
将 Netconf 协议中的概念映射成各种对象模型。
而 ct_netconfc 的实现则简单明了许多，只是为了尽快得到可以使用的数据结构，
这种端到端的设计思路逻辑清晰，代码量也不多。

`Offer solution in a simple way` ，这就是 Erlang 语言的性格。
