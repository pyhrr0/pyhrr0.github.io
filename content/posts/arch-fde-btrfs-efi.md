---
title: "Arch Linux with FDE, btrfs and EFI support"
date: 2022-01-21T13:33:37Z
---

Disk preparations
-----------------
1) Create two partitions (for `/boot` and `/`):
    ```console
    # fdisk /dev/sda
    ```
    - g - create a new empty GPT partition table
    - n - add a new partition
    - t - change a partition type

2) Create / initialize usable LUKS container (and accompanying volumes):
    ```console
    # cryptsetup luksFormat /dev/sda2
    # cryptsetup open /dev/sda2 luks_container
    # pvcreate /dev/mapper/luks_container
    # vgcreate encrypted /dev/mapper/luks_container
    # lvcreate -l 100%FREE encrypted -n rootfs
    ```
3) Create filesystems and mount partitions:
    ```console
    # mkfs.fat -F 32 /dev/sda1                                     # /boot
    # mkfs.btrfs /dev/mapper/encrypted-rootfs                      # /
    # mount /dev/mapper/encrypted-rootfs /mnt
    # btrfs subvolume create /mnt/@
    # btrfs subvolume create /mnt/@home
    # btrfs subvolume create /mnt/@snapshots
    # umount /mnt

    # mount -o subvol=@ /dev/mapper/encrypted-rootfs /mnt
    # mkdir -p /mnt/home /mnt/boot
    # mount -o subvol=@home /dev/mapper/encrypted-rootfs /mnt/home
    # mount /dev/sda1 /mnt/boot
    ```

Install Arch Linux
------------------
1) Ensure there's internet connectivity:
  - Wired:
    ```console
    # dhclient <device_name>
    ```
  - Wireless:
    ```console
    # iwctl
    # [iwd]# device list
    # [iwd]# station <device_name> scan
    # [iwd]# station <device_name> get-networks
    # [iwd]# station <device_name> connect SSID
    ```
2) Install base system + desired packages:
   ```console
   # pacman -Sy reflector
   # reflector --country=<country_name> > /etc/pacman.d/mirrorlist
   # pacstrap /mnt base linux linux-firmware intel-ucode <any_other_desired_package_name>
   # genfstab -U /mnt >> /mnt/etc/fstab
   # arch-chroot /mnt
   # ln -sf /usr/share/zoneinfo/<Region>/<City> /etc/localtime
   # hwclock --systohc
   # locale-gen
   # echo 'LANG=en_US.UTF-8' > /etc/locale.conf
   # echo '<DESIRED_HOSTNAME>' > /etc/hostname
   ```
Configure bootloader
--------------------
1) Install `systemd-boot` to `/boot` partition:
   ```console
   # bootctl --path=/boot install
   ```
2) Configure the bootloader:
   ```console
   # cat <<EOF > /boot/loader/loader.conf
   > default arch.conf
   > timeout 0
   > EOF

   # cat <<EOF > /boot/loader/entries/arch.conf
   > title Arch Linux
   > linux /vmlinuz-linux
   > initrd /intel-ucode.img
   > initrd /initramfs-linux.img
   > options rd.luks.name=<UUID_OF_SDA2_FS>=luks_container root=/dev/mapper/encrypted-rootfs rootflags=subvol=@ rd.luks.options=discard ipv6.disable=1
   > EOF
   ```
3) Generate initramfs:
   ```console
   # sed -i 's/^HOOKS=.*$/HOOKS=(base systemd autodetect modconf keyboard block sd-encrypt lvm2 filesystems fsck)/' /etc/mkinitcpio.conf
   # mkinitcpio -p linux
   ```

Final bits
----------
1) Install desired files in `/etc`:
  - /etc/proxychains.conf
  - /etc/iptables/iptables.rules
  - /etc/systemd/resolved.conf
  - /etc/wpa_supplicant/wpa_supplicant-<device_name>.conf
  - /etc/systemd/network/10-wifi.link
  - /etc/systemd/network/10-wifi.network
  - /etc/privoxy/config
  - /etc/fstab (add tmpfs)

2) Enable desired unit files:
   ```console
   # systemctl enable --now fstrim.timer
   # systemctl enable --now btrfs-scrub@-.timer
   # systemctl enable --now systemd-networkd.service
   # systemctl enable --now systemd-resolved.service
   # systemctl enable --now systemd-timesyncd.service
   # systemctl enable --now wpa_supplicant@<device_name>.service
   # systemctl enable --now iptables.service
   # systemctl enable --now privoxy.service
   # systemctl enable --now tlp.service
   ```
