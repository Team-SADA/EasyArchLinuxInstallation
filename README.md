# Easy ArchLinux Installation

1. Download ISO File on archlinux.org
2. Boot with that ISO
3. Run `ping google.com` for test network connection
> if your computer has WLAN, the internet will not working now
> Run `iwctl` and Run `station wlan0 scan` `station wlan0 connect <your_SSID>` to connect the wifi.
4. Run `timedatectl set-ntp true` for enable system clock
5. Create Partition
> `fdisk -l` for get list of current partition
> 
> `fdisk /dev/sdx` or `fdisk /dev/vdx` or `fdisk /dev/nvmexnx` to edit the drive. This command will be specified by type of storage.
> Press `n` on fdisk shell to create new partition
> Press `w` on fdisk shell to write the partition
> Google about fdisk to get more information

> `mkfs.ext4 /dev/<device>` to make the partition to ext4 format
> 
> `mkswap /dev/<device>` to make the partition to swap

6. Run `swapon /dev/<device>` to enable swap
7. Run `mount /dev/<device> /mnt` to mount the ext4 device which want to install arch-linux
8. Run `pacstrap /mnt base linux linux-firmware vim dhcpcd` to install archlinux
> vim is for text-editor, dhcpcd for ethernet
> 
> if your computer has wifi, replace dhcpcd to iwd to enable WLAN

8. Run `genfstab -U /mnt >> /mnt/etc/fstab`
9. Run `arch-chroot /mnt` to move to your linux
10. Set timezone with command
```bash
ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
hwclock --systohc
```
11. Edit /etc/locale.gen to your own locale and Run `locale-gen`
12. Create /etc/locale.conf and write `LANG=<your_locale>` to the file(this will cause error when launching gnome DE)
13. Create /etc/hostname and write your own hostname, in my case, I wrote `dayo`
14. Create /etc/hosts and write this
```
127.0.0.1 localhost
::1 localhost
```
15. Edit /etc/resolv.conf and apply your DNS Server
16. Run `mkinitcpio -P` to initalize your linux
17. Run `passwd` to set your root password
18. Run `pacman -S grub efibootmgr` to install GRUB, if your computer using MBR, efibootmgr is not required.
19. Setup microcode with this document https://wiki.archlinux.org/title/Microcode
20. Mount your EFI driver to /mnt with mount command
21. Run `grub-install --efi-directory=/mnt` to setup grub
22. Run `grub-mkconfig -o /boot/grub/grub.cfg` to make config file of grub
> See https://wiki.archlinux.org/title/GRUB for more information
23. Reboot your computer, If installation successed, GRUB will display first.
24. Enter `Arch linux` to boot your OS
25. Enter username: `root`, password: `<your_root_password>` to login. Password will not displayed but typing will injected include of backspace
26. Now internet will not working. Type this to enable internet connection
Ethernet: 
```
ip link
systemctl start dhcpcd@<your_lan_device_on_ip_link_output_except_lo>
```
in case of WLAN: 
```
systemctl start iwd
iwctl # set wifi connection via iwctl
```
27. Install sudo
28. Add new user except root
29. Edit sudoers file, this file is default non-editable so you need to edit permission(like chmod +w sudoers). It is important to remove edit permission after working.
30. reboot and login to new user
31. install `gnome`
32. Run `sudo systemctl start gdm` to start XServer
33. Enjoy your new linux!
