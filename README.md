# archlinux-installer-script
This installer script is meant to be used from a bootable usb when you
install Arch Linux. This script tries to only do what is necessary to get up
and running with an Arch console on your machine. The script is configured for
EFI only, but you can change two lines to turn it into a BIOS install (disk
formatting and grub installation). Refer to the [arch install
guide](https://wiki.archlinux.org/title/installation_guide) so that you know
what you're doing.

The script does the following setup:

- Partition drive in order: 512MiB Boot, 14G Swap, 50G Root, Home (remainder).
- Format partitions.
- Mount partitions to `/mnt`.
- Create directories on `/mnt`.
- Call `pacstrap` on `/mnt` to install the following packages:
    - Core `linux linux-firmware base base-devel efibootmgr grub`
    - System `vim git man networkmanager` 
- Generate fstab in `/mnt/etc/fstab`.
- `arch-chroot` to the installation and do the following:
    - Generate locale for `en_US.UTF-8`.
    - Set timezone to `Europe/Amsterdam` (change later).
    - Synchronize time with `hwclock --systohc`.
    - Enable networkmanager with systemd.
    - Set root password to `123` (change later).
    - Set hostname to `archer`.
    - Create `/boot/grub directory`.
    - Create Grub config with `grub-mkconfig`.
    - Install Grub with `grub-install`.
- Outside of chroot: unmount and notify user of completion.

# Install guide 

To get internet you need to use the `iwctl` wifi manager or plug in ethernet
cable.
```sh 
iwctl
[iwd]# device list
[iwd]# station <DEVICE> scan
[iwd]# station <DEVICE> get-networks
[iwd]# station <DEVICE> connect <SSID>
```

When you have internet you can download the script with `curl`:

```sh
curl -LO "https://raw.githubusercontent.com/parcevval/archlinux-installer-script/master/README.md"
```

Once you have the script you can run it. Arguments for the command are
automatically printed. If arguments are incorrect, the script will tell you.
Be sure that you want to wipe your drive and type `y` to proceed.

```sh 
chmod +x archstrap.sh
./archstrap.sh /dev/sda
Are you certain? (y/n)
y
...
```

At the end you will be greeted with a message giving you the root login 
credentials: username `root` and `123` as password. Change these later.
