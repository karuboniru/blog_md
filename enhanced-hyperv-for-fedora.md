---
title: 在 Hyper-V 会话中对于 Fedora 启用增强会话
date: 2020-08-22 10:31:03
tags: 瞎折腾
categories: 瞎折腾
---
## Hyper-V 的图形界面搞得老百姓怨声载道
 - 卡出翔
 - 分辨率最高 `1080x1920`
 - 剪切板共享没有

## TL;DR, 使用增强模式
``` Bash
$ git clone https://github.com/karuboniru/linux-vm-tools.git
$ cd linux-vm-tools/fedora
$ sudo ./install-config.sh
```
虚拟机关机, 打开 PowerShell （需要管理员权限）
```
PowerShell> Set-VM -VMName <your_vm_name> -EnhancedSessionTransportType HvSocket
```
重新打开虚拟机即可

## 效果
{% gi 3 1-2 %}
    ![结果](https://cdn.yanqiyu.info/20200822104108.png)
    ![登录界面](https://cdn.yanqiyu.info/20200822103957.png)
    ![增强会话选项](https://cdn.yanqiyu.info/20200822103919.png)
{% endgi %}

## Detail: 你多干了啥
主要是为了让 SELinux 高兴增加了模块：
```
module allow-vsock 1.0;
 
require {
        type unconfined_service_t;
        type unlabeled_t;
        class vsock_socket { getattr read write };
}
 
#============= unconfined_service_t ==============
allow unconfined_service_t unlabeled_t:vsock_socket { getattr read write };
```

但是新版本的 xrdp 貌似自带类似的模块，但是为了保证开箱即用就还是加上吧。

## 后文
脚本是照着 [Arch 的版本][Arch] 改的，[PR][pr] 却没人 review，固有此文。不高兴，希望所有 PR 都能被善良对待。

[Arch]: https://github.com/microsoft/linux-vm-tools/tree/master/arch
[pr]: https://github.com/microsoft/linux-vm-tools/pull/124