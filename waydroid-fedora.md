---
title: Waydroid on Fedora
date: 2021-10-25 23:10:07
---

## Waydroid need kernel with ashmem and binder
Ashmem and binder is what makes "Android kernel" different from traditional desktop's. They are not built with Fedora's stock kernel for now ([But they are planning so](https://bugzilla.redhat.com/show_bug.cgi?id=1455411)). So we need to install a kernel with ashmem and binder support.

### Use XanMod Kernel
You can find XanMod Kernel from [Copr](https://copr.fedorainfracloud.org/coprs/rmnscnce/kernel-xanmod/), just follow instructions and you are all set.

### Build Kernel with Ashmem and Binder
Fedora don't ship kernel with Ashmem and Binder support, we need to build on our own. Before starting please confirm your secboot status is **off**, or your have set up 
self signing secboot flow.

Then install required package to build kernel by `sudo dnf install mock fedpkg`. Clone fedora's kernel packaging files:
```
fedpkg clone -a kernel
git checkout f34 # use your fedora release version
```
Add following lines to `kernel-local` file:
```
CONFIG_ASHMEM=y
CONFIG_ANDROID=y
CONFIG_ANDROID_BINDER_IPC=y
CONFIG_ANDROID_BINDER_DEVICES="binder,hwbinder,vndbinder"
CONFIG_STAGING=y
CONFIG_ANDROID_BINDERFS=n
CONFIG_ANDROID_BINDER_IPC_SELFTEST=n
```
Then open kernel.spec, find `./process_configs.sh $OPTS kernel %{rpmversion}` and change it to `./process_configs.sh $OPTS kernel %{rpmversion} ||:`.

Finally, execute `fedpkg mockbuild`, wait for several minutes, you will find built kernel in `results_kernel` folder, just install package `kernel`, `kernel-core` and `kernel-modules` 
should be enough.


## Install Waydroid
I have build waydroid in Copr, you can find them [here](https://copr.fedorainfracloud.org/coprs/yanqiyu/waydroid/). And install them by
```
sudo dnf copr enable yanqiyu/waydroid
sudo dnf install waydroid
```

If you are using kernel from XanMod, you may need to change `/etc/gbinder.d/anbox.conf` to 
```
[Protocol]
/dev/anbox-binder = aidl2
/dev/anbox-vndbinder = aidl2
/dev/anbox-hwbinder = hidl

[ServiceManager]
/dev/anbox-binder = aidl2
/dev/anbox-vndbinder = aidl2
/dev/anbox-hwbinder = hidl
```

Then just follow [Official Waydroid Website](https://waydro.id/)
```
sudo waydroid init
sudo systemctl start waydroid-container
waydroid show-full-ui
```
To begin using Waydroid.

***
![Playing Arknights via Waydroid](https://cdn.jsdelivr.net/gh/karuboniru/blog_imgs@master/20211025232040.png)

***
## Known problem
Multi-window mode is not working for me, you can refer to [this issue](https://github.com/waydroid/waydroid/issues/131)
