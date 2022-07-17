# Gentoo_install
Gentoo Installation Process

```
rc-service sshd start
```
```
passwd root
```

Now SSH is ready !

``` 
parted -a optimal /dev/sda
```
```
mklabel gpt
```
```
unit MB
```
```
mkpart primary 1 20
```
```
mkpart primary 21 500
```
```
mkpart primary 501 2500
```
```
mkpart primary 2501 -1
```
```
print
```

Now Naming Partitions:

```
name 1 grub
```
```
set 1 bios_grub on
```
```
name 2 boot
```
```
name 3 swap
```
```
name 4 root
```
```
quit
```

```
lsblk
```

Now Setup filesystems :

```
mkfs.ext4 /dev/sda2 && mkfs.ext4 /dev/sda4 && mkswap /dev/sda3 && swapon /dev/sda3
```

Mount neccessary prtitions :

```
mount /dev/sda4 /mnt/gentoo && mkdir /mnt/gentoo/boot && mount /dev/sda2 /mnt/gentoo/boot
```

Getting tarball for Openrc - desktop :

```
cd /mnt/gentoo
```
```
wget https://bouncer.gentoo.org/fetch/root/all/releases/amd64/autobuilds/20220710T170538Z/stage3-amd64-desktop-openrc-20220710T170538Z.tar.xz
```
```
tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner
```

Editing Flags :

```
nano /gentoo/mnt/etc/portage/make.conf
```
Add "-march=native" to below line :

COMMON_FLAGS="-O2 -pipe"

```
-march=native
```
Adding Repos :

```
mkdir --parents /mnt/gentoo/etc/portage/repos.conf
```
```
cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf
```
Copy DNS Info :

```
cp --dereference /etc/resolv.conf /mnt/gentoo/etc/
```

Mounting the necessary filesystems :

```
mount --types proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev
mount --bind /run /mnt/gentoo/run
mount --make-slave /mnt/gentoo/run
```

Entering the new environment :

```
chroot /mnt/gentoo /bin/bash && source /etc/profile && export PS1="(chroot) ${PS1}"
```

### Configuring Portage:

Installing a Gentoo ebuild repository snapshot from the web

```
emerge-webrsync && emerge --sync
```




