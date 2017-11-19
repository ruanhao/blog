---
title: Erlang Netconf Client
date: 2017-11-19T13:13:12+08:00
categories: erlang
---

## 前言

公司的项目通过 [Netconf](https://en.wikipedia.org/wiki/NETCONF) 协议从网络设备获取配置，管理，运行参数等信息。
使用 [Opendaylight](https://www.opendaylight.org/) 这个平台实现基于 Netconf 的交互操作。

在 SDN 领域，ODL 可以算得上是集大成者，也提供了一些很棒的插件，比如实现 [YANG](https://en.wikipedia.org/wiki/YANG) 与 Java 对象之间的映射绑定。
由于当初对 Netconf 协议了解不多，而 ODL 正好在这一领域提供了一揽子解决方案，为了快速开展项目，就选择了它。
但其实在整个项目中 ODL 只是充当了 Netconf client 的角色，很多模块和内置插件虽然启动了，但是完全没有用到，
考虑到 Docker 和 AWS 上的部署与运维，使用 ODL 的成本就显得很高，因为整个 ODL 体积太庞大，而且需要占用大量内存。

我打算寻找一种简单的 Netconf client 实现方案替代 ODL 。

## 替代方案

其实相比实现 Netconf server ，client 端相对简单一些，只需要实现 Netconf 协议的编解码和消息处理逻辑。

在网上搜索一番之后，发现了 [Erlang Common Test](http://erlang.org/doc/man/common_test.html) 这个框架，
其中包含一个测试套件 [ct_netconfc](http://erlang.org/doc/man/ct_netconfc.html) 。


看了一下帮助文档，发现 ct_netconfc 实现了 [RFC 4741](https://tools.ietf.org/html/rfc4741) ，完全可以为我所用。
而且实现了 [RFC 4742](https://tools.ietf.org/html/rfc4742) ，即基于 SSH 传输协议。
我们的项目是基于 TCP 的，只需要将传输协议改造下即可。

## 代码改造

ct_netconfc 代码写得很简练，重构过程还算轻松：

1. 将编解码的逻辑从框架中抽出
2. 使用 TCP 作为底层传输协议
3. 将 Netconf 连接抽象成 Erlang 进程 (gen_server)

[Talk is cheap, show me the code](https://github.com/ruanhao/simple-netconf-client)


## 后记

其实最开始，在参考了 [ConfD](http://www.tail-f.com/confd-netconf-server/) 的源码后，用 Python 写过一个 demo ，
已经实现了最基本的功能。但是心里始终有个想法，就是希望用 Erlang 来实现。

虽然 Python 在越来越多的领域中使用，而且表现出色，但我认为它的长处在于系统管理，快速建模，数据挖掘等场景。
而多并发 (Erlang process) ，协议处理 (bit syntax) ，传输 (gen_tcp) 这些方面都是 Erlang 的强项。

还有一个原因就是我非常喜欢 Erlang 这门语言，语法简单朴实却功能强大，很有北欧人的性格特征。

已经好久没写 Erlang 了，是时候重拾 Erlang 了。
