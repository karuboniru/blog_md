---
title: 如何下周就在 Fedora 32 用上 Fcitx 5
date: 2020-08-30 17:44:07
tags: 
categories: 打包
---

一开始想了想要不要在标题写 Fedora，觉得还是必要的。因为目前只有 Arch Linux （和 Debian 和 Ubuntu）出于套近乎的关系有了 Fcitx 5 全家桶。

为什么是下周？——因为 Fedora 的 QA，**包最长会在 [Bodhi] 等一周，除非你们帮忙测试，点个 upvote** (๑•̀ㅂ•́)و✧。

测试大概明天或者后天上线，想要参与就`dnf upgrade --enablerepo=updates-testing`来进行安装。

***

## 建议安装的包
 - fcitx5               
 - fcitx5-gtk
 - fcitx5-qt 
 - fcitx5-configtool    
 - fcitx5-chinese-addons

另外之前用了我的 [copr] 版本的人，请保证把里面的包卸载之后在进行安装，否则可能出现奇妙的冲突。

虽然在别的发行版上面最新的 Fcitx 4/5 不能共存，但是在 Fedora 上能，rpm 能优雅的处理表面上的文件冲突。
对于原因感兴趣的可见[群里面的讨论]的上下文。

## 环境变量和自启动
### 对于 X11 用户
``` Bash
$ sudo alternatives --config xinputrc
```
即可修改全局输入法配置，但是要是你只想要修改自己的配置的话：
```Bash
$ ln -s /etc/X11/xinit/xinput.d/fcitx5.conf ~/.xinputrc
```
理论上**注销之后重新登陆**就会生效。

### 对于 Wayland 用户
参阅 [Arch Wiki]，在 `~/.pam_environment` 添加
```
INPUT_METHOD  DEFAULT=fcitx5
GTK_IM_MODULE DEFAULT=fcitx5
QT_IM_MODULE  DEFAULT=fcitx5
XMODIFIERS    DEFAULT=\@im=fcitx5
```
然后运行（当然 `ln -s` 可以换成 `cp`）
``` Bash
$ ln -s /usr/share/applications/fcitx5.desktop ~/.config/autostart/
```
同样**注销之后重新登陆**就会生效。但是 Wayland 下可能不会很好使，听天由命吧。

## 一些其他的提示
### 对于 Gnome 用户
见 [李先生的博客] 文章，建议安装 **kimpanel** 插件以改善体验。(以下引用 block 是直接厚颜无耻照抄的, 意味着内容可能过时，没准 Gnome 商店的版本也超级好使呢？)

> 众所周知，网络上吹 Fcitx 5 的用户大多数都是 Arch Linux 用户、而且用的都是 KDE，没有人告诉你 Gnome-shell 要怎么办，不过万幸的是伟大的囧脸的 Gnome shell 插件是支持 Fcitx 5 的，因为用的都是 Kimpanel，也就是说，装了这个 Gnome-shell extension，无论你是 Fcitx 4 还是 Fcitx 5，都是可以用的，赞美囧脸！
> 不过这个插件在 Gnome 官方的 Extension 网站上的版本有一些问题：
> 
> - [快速打字的时候会出现部分内容显示不全]
> - 多显示器的时候会跨越显示器出现选字框
> - [锁屏后解锁会出现两个 Indicator]
> 
> 不过这三个问题都已经被囧脸修复了，赞美囧脸！
>
> 安装的话还是推荐安装 git 的版本，因为官方的还没有更新：
> ```
> https://github.com/wengxt/gnome-shell-extension-kimpanel
> ```
> 安装依赖：gettext cmake 后直接在目录下运行 ./install.sh 就可以了，记得把原本的插件删掉再装。

### 图形界面配置工具
`fcitx5-configtool` 含有 `fcitx5-config-qt`，安装之后`fcitx5-config`就会调用之。另外还支持KCM配置，当然是KDE用户专享了。

## Bug Report
遇到问题，建议先在[Bugzilla]反馈，如果是我的锅（打包翻车），我就修。如果是囧脸的锅，那我就找囧脸修。

当然要是你能判断是囧脸的锅，建议直接去上游找囧脸修。

## TODO List
 - [ ] fcitx5-configtool 拆包，把 `kcm-fcitx5` 拆出来
 - [ ] fcitx5-chinese-addons 拆包
 - [ ] Fedora 31 上的编译

_为什么现在不做，这些事情不复杂啊？_ ——懒

## 此处应该感谢[囧脸]
CSSlayer（囧脸）对于打包做出了巨大贡献，包括但不限于：

- 舍去刷蹦蹦蹦的时间深夜来修 aarch64 上的 bug
- 帮我识别出一个 s390x 上的错误的真正原因，还给 KenLM 提了 PR，修复了十有八九不会有活人遇到的 s390x 上的一个可能导致整个 chinese-addons 不好使的 bug

这里吐槽一下 Fedora，真的有人会在 s390x 大机上面**跑桌面**吗？不过修了也好，没准会有什么别的大端序用户。

***
最后高呼三遍：**赞美囧脸！赞美囧脸！赞美囧脸！**

[Bodhi]: https://bodhi.fedoraproject.org/updates/FEDORA-2020-5465c02630
[copr]: https://copr.fedorainfracloud.org/coprs/yanqiyu/fcitx5
[Arch Wiki]: https://wiki.archlinux.org/index.php/Fcitx5_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)
[囧脸]: https://www.csslayer.info/
[Bugzilla]: https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&bug_status=ASSIGNED&classification=Fedora&component=fcitx5&list_id=11319828&product=Fedora&product=Fedora%20EPEL
[李先生的博客]: https://plumz.me/archives/11740/
[囧脸的博客]: https://www.csslayer.info/wordpress/fcitx-dev/%e5%a6%82%e4%bd%95%e7%8e%b0%e5%9c%a8%e5%b0%b1%e5%9c%a8-arch-linux-%e7%94%a8%e4%b8%8a-fcitx-5/
[快速打字的时候会出现部分内容显示不全]: https://github.com/wengxt/gnome-shell-extension-kimpanel/issues/46
[锁屏后解锁会出现两个 Indicator]: https://github.com/wengxt/gnome-shell-extension-kimpanel/issues/47
[群里面的讨论]: https://t.me/fedorazh/63996
