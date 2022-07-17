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

