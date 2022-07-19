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
nano /mnt/gentoo/etc/portage/make.conf
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
### Copy DNS Info :

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
chroot /mnt/gentoo /bin/bash
```
```
source /etc/profile && export PS1="(chroot) ${PS1}"
```

### Configuring Portage:

Installing a Gentoo ebuild repository snapshot from the web

```
emerge-webrsync && emerge --sync
```

### Choosing the right profile

```
eselect profile list
```
For (KDE) Plasma Desktop Profile :

```
eselect profile set 8
```

### Updating the @world set

```
emerge --ask --verbose --update --deep --newuse @world
```

### TimeZone :

```
ls /usr/share/zoneinfo
```

```
echo "Asia/Tehran" > /etc/timezone
```

```
emerge --config sys-libs/timezone-data
```
### Location specific character set and Language :

```
nano /etc/locale.gen
```

Uncomment "en_US.UTF-8 UTF-8" from above file !

```
locale-gen
```

```
eselect locale list
```

Select (en_US.utf8) :

```
eselect locale set 4
```

#### Update environment :
```
env-update && source /etc/profile && export PS1="(chroot) ${PS1}"
```

### Configuring Kernel:

Get the kernel source and compile it using genkernel (The Automatic way :) )

Unmasking required packages for the next emerge commands :
```
echo "sys-kernel/linux-firmware @BINARY-REDISTRIBUTABLE" | tee -a /etc/portage/package.license
```

```
emerge gentoo-sources && emerge genkernel
```
```
cd /usr/src/ && ln -s linux-5.15.52-gentoo linux && cd -
```

#### Compile Kernel and Modules :

```
genkernel all
```


