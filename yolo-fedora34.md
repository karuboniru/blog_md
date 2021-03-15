---
title: 莽一把，升级 Fedora 34
date: 2021-03-15 11:16:54
tags: 瞎折腾
categories: 瞎折腾
index_img: https://cdn.jsdelivr.net/gh/karuboniru/blog_imgs@master/20210315111708.png
---

{% note danger %}
Fedora 34 （在 2021 年 3 月 15 日）还处于 Prerelease 状态，虽然我使用过程没遇到严重问题，但是不建议新手盲目上测试版。
{% endnote %}

在邮件列表看了下 Fedora 34 的 [blocker bugs] 状态，感觉严重的问题基本上已经被解决了。加上现在我的机器有牛逼闪闪的 Btrfs 加成，再大的翻车都能回滚快照。

## 总之先搞一个快照
顺便把快照传出去：
```Bash
sudo btrfs sub snap -r / /.snapshot/fedora-33
sudo btrfs send /.snapshot/fedora-33 | pigz --best > /any/path/fedora-33.btrfs.gz
```

## 众所周知的升级过程
```Bash
sudo dnf system-upgrade download --refresh --releasever=34
sudo dnf system-upgrade reboot
```
![超级多的包](https://cdn.jsdelivr.net/gh/karuboniru/blog_imgs@master/20210315113127.png)
平平稳稳的重启，祈祷不要翻车。

## 使用 Gnome 40 是一种怎样的体验
说实话，第一眼看到 Gnome 40 的时候还是很喜欢这个设计的，
![](https://cdn.jsdelivr.net/gh/karuboniru/blog_imgs@master/20210315113321.png)
虽然比较浪费纵向空间，但是登陆就进入上图的截面确实比登陆就面对什么都没有的做面要好。

还有一个没怎么被翻译的 Tours 应用，设置翻译也缺一些火候。
{% gi 2 2 %}
![](https://cdn.jsdelivr.net/gh/karuboniru/blog_imgs@master/20210315113443.png)
![](https://cdn.jsdelivr.net/gh/karuboniru/blog_imgs@master/20210315113609.png)
{% endgi %}

## 蹬蹬咚
一切都是好的...知道我开始准备在 Telegram 吹水...

发现我的输入法 Panel 变回了 Fcitx5 的界面（而不是 kimpanel 的界面）。
![痛苦面具](https://cdn.jsdelivr.net/gh/karuboniru/blog_imgs@master/20210315114401.jpg)

好家伙，我的插件全部炸了。我真傻，真的，我单知道 @Sumomogal 说的 [Gnome 会炸插件]，却不知道会炸掉我的全家身当。救火要紧，赶紧去骚扰老K。

老K[瞬间修好了扩展]，总算是能愉快的输入了。别的插件陆陆续续提交 issue，但愿大家能早日复活。

[blocker bugs]: https://qa.fedoraproject.org/blockerbugs/milestone/34/beta/buglist
[Gnome 会炸插件]: https://twitter.com/Sumomogal/status/1370576849084899331?s=20
[瞬间修好了扩展]: https://github.com/wengxt/gnome-shell-extension-kimpanel/commit/f0afbfe17ab421841a15eb8d0761d9105686b7f1