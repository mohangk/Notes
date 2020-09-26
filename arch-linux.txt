### installation guides

<https://linoxide.com/distros/beginners-arch-linux-installation-guide/>

<https://gist.github.com/CodingCellist/05556e0cb6cde146fc3f70b578b73da3>

[https://linuxconfig.org/install-arch-linux-on-thinkpad-x1-carbon-gen-7-with-encrypted-filesystem-and-uefi](https://linuxconfig.org/install-arch-linux-on-thinkpad-x1-carbon-gen-7-with-encrypted-filesystem-and-uefi)

### disk encryption guides

- https://github.com/ejmg/an-idiots-guide-to-installing-arch-on-a-lenovo-carbon-x1-gen-6
- https://gmpreussner.com/reference/fully-encrypted-archlinux-with-secure-boot-on-yoga-920
- https://www.howtoforge.com/tutorial/how-to-install-arch-linux-with-full-disk-encryption/ (this was used)
- to encrypt: cryptsetup --verbose --cipher aes-xts-plain64 --key-size 512 --hash sha512 --iter-time 5000 --use-random luksFormat /dev/sda2
- *todo - document the encryption setup process*

### mounting the filesystem and chroot

- when trying to moung from boot disk
  
  - to decrypt: cryptsetup open --type luks /dev/nvme0n1p2 cryptroot
  
  - mount -t auto /dev/mapper/cryptroot  /mnt

- mount boot file system
  
  - mount -t auto /dev/nvme0n1p1 /mnt/boot

- chroot the mthe /mnt 

#### fix small fonts on console

- cat /etc/vconsole.conf
  
  - FONT=ter-132n
