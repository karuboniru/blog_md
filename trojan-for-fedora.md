---
title: Trojan for Fedora and EPEL
date: 2020-06-21 09:12:50
tags: 打包
categories: 瞎折腾
---
[Trojan][1] 即将在 Fedora 操作系统官方源(含 EPEL 8)可用🎉.

详情可见 [Fedora Bodhi][2], 以及 [Trojan issue #462][3]. 

在 Trojan 进入 stable 仓库之后我会去更新一下 Trojan 那边的安装教程, 顺便在这里也写一下. 等到软件进入 Testing 之后大家也可以帮忙测试. 有这个版本特有的问题（来自于打包等的问题）请汇报[Bugzilla][4]或者是[邮件联系我][5]. 来自于上游的问题可以[直接汇报给上游][6], 但请注明软件包安装来源.

## Update 20/6/22
已经可用
```
Package: trojan-1.16.0-4.fc33
Summary: An unidentifiable mechanism that helps you avoid censorship
RPMs:    trojan
Size:    1.54 MiB
```

# 接下来是 clash

Clash 已经在 fedora 官方源可用!


[1]: https://github.com/trojan-gfw/trojan/
[2]: https://bodhi.fedoraproject.org/updates/?search=trojan
[3]: https://github.com/trojan-gfw/trojan/issues/462
[4]: https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&bug_status=ASSIGNED&classification=Fedora&component=trojan&product=Fedora&product=Fedora%20EPEL
[5]: mailto:yanqiyu@fedoraproject.org
[6]: https://github.com/trojan-gfw/trojan/issues