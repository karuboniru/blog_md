---
title: Switch to systemd-boot and Unified Kernel Image on Fedora 
date: 2021-04-30 11:32:57
---
{% note danger %}
I am not responsible for bricked computers, system instabilities, dead cats, thermonuclear war or you getting fired because you lost important work.

Please make a backup of your device or of the data, and make a boot drive in case of necessity. Do some research if you have any concerns about steps documented in this guide.

YOU are choosing to make these modifications, and if you point the finger at me for messing up your device, I will laugh at you.
{% endnote %}


## Why are you doing this kind of wried thing?

Ok... Since I want to sign secure boot on my own, without breaking Fedora's current multi-kernel behavior. Luckily that [kernel-install.d] provides enough power to customize kernel install process.

## Switch to Systemd-Boot (from Grub+Shim)
{% note danger %}
ENSURE YOU ARE ON UEFI BEFORE DOING THIS.
{% endnote %}

It is basically same as [kowalski7cc's article] with minor modifications to make it work with Unified Kernel Image

### Move efi mount point
You may want to check if modifications to `/etc/fstab` is sanity.
```Bash
sudo mkdir /efi
sudo sed -i "s|/boot/efi|/efi|g" /etc/fstab
```
Now remount efi:
```Bash
sudo umount /boot/efi
sudo mount /efi
```

### Install systemd-boot
You may want to backup files at `/boot/efi` and `/boot` before this.
```Bash
sudo mkdir /efi/$(cat /etc/machine-id)
sudo rm /etc/dnf/protected.d/grub* /etc/dnf/protected.d/shim*   # needed in some cases, if next command won't run.
sudo dnf remove grubby grub2\* shim\* memtest86\ && sudo rm -rf /boot/grub2 && sudo rm -rf /boot/loader
cat /proc/cmdline | cut -d ' ' -f 2- | sudo tee /etc/kernel/cmdline
sudo bootctl install
```
And you may umount `/boot` and remove its fstab entry now since it will no longer be used.

{% note danger %}
DO NOT REBOOT UNTIL I TOLD YOU THAT YOU CAN.
{% endnote %}

## Change kernel-install scripts to enable Unified Kernel Image
### Disable old initrd generation
Do following:
```Bash
sudo ln -s /dev/null /etc/kernel/install.d/50-dracut.install
```
to disable default initrd generation and installation, we are going to move this work in other scripts.

### Change installation of kernel image
Create file at `/etc/kernel/install.d/90-loaderentry.install` with contents [here](https://gist.github.com/karuboniru/d47b0a70f53614d90d30946745c33ab9)

The `dracut --kernel-cmdline "${BOOT_OPTIONS[*]}" -f ${noimageifnotneeded:+--noimageifnotneeded} --uefi "$LOADER_ENTRY" "$KERNEL_VERSION"` does the magic to enable Unified Kernel Image.

### Change generation of rescue image
Create file at `/etc/kernel/install.d/51-dracut-rescue.install` with contents [here](https://gist.github.com/karuboniru/2e6fb6dc48094a7bbd9671da42a83960), this is for building rescue entry in unified way.


## Reinstall kernel-core
Reinstall your kernel image to make changes apply:
```Bash
sudo dnf install binutils # needed by dracut to build kernel image
sudo dnf reinstall $(rpm -qa|grep kernel-core)
```

You should be able to see several `.efi` kernel image at `/efi/EFI/Linux`, if it's not there, rollback with your backup. Now you can reboot to see if everything works.

## Update systemd-boot on demand
Install plugins to run scripts for dnf:
```Bash
python3-dnf-plugin-post-transaction-actions
```

Create file `/etc/dnf/plugins/post-transaction-actions.d/systemd-udev.action`
```
systemd-udev:in:bootctl update
```
This will enable systemd-boot in efi to update along with systemd package.


[kernel-install.d]: https://www.freedesktop.org/software/systemd/man/kernel-install.html
[kowalski7cc's article]: https://kowalski7cc.xyz/blog/systemd-boot-fedora-32
