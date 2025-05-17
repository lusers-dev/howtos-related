# Linux Users - Swapfiles and Swap Partitions

This guide will walk you through the process of setting up a swapfile or swap partition on Red Hat 8 and Red Hat 9 type systems. 
A swapfile can be useful for providing extra memory resources when physical memory (RAM) is running low.

## Sample Setup a New Swap Partition

```bash
## display disks
sudo fdisk -l
```

```bash
## make new partition type swap (option 82)
sudo fdisk /dev/nvme1n1
  - Type 'n' to create new partition
  - Choose partiton number and size
  - Type 't' to change the partition type
  - Enter '82' as the hex code for Linux swap
```

```bash
## format new partition as swap
sudo mkswap /dev/nvme1n1p1
```

```bash
## enable swap partition
sudo swapon /dev/nvme1n1p1
```

```bash
## display of swap is on
swapon -s
```

```bash
## enable swap on boot
## edit /etc/fstab to enable swap on boot
...
/dev/nvme1n1p1 none swap sw 0 0   
```


## Sample Swapfile Size: 2GB (128MB x 16)

```bash
# Create a 2GB swapfile
sudo dd if=/dev/zero of=/swapfile bs=128M count=16
sudo mkswap /swapfile
sudo chmod 0600 /swapfile
```

```bash
# Add swapfile to /etc/fstab
sudo vi /etc/fstab
/swapfile none swap defaults 0 0
```

```bash
# Enable the swapfile
sudo systemctl daemon-reload
sudo swapon /swapfile
```

```bash
# Verify swap is enabled
cat /proc/swaps
sudo swapon -s
```

```bash
# Check system memory
free -h
```

```bash
# Reboot system
sudo systemctl reboot
```


## Removing References to a Swap Partition

If a swap partition was previously used, follow these steps to remove references to it.
( assuming UUID=25e2fbf0-5c9c-4542-b896-154dc66df34b was used as swap partition )

### For Red Hat 8

```bash
# List current kernel options
sudo grub2-editenv - list | grep kernelopts

kernelopts=root=UUID=81960c3e-160b-4323-992f-e2d8af3370ab ro crashkernel=auto resume=UUID=25e2fbf0-5c9c-4542-b896-154dc66df34b rhgb quiet

# Remove swap partition reference
sudo grub2-editenv - set "kernelopts=root=UUID=81960c3e-160b-4323-992f-e2d8af3370ab ro crashkernel=auto rhgb"
```


### For Red Hat 9

#### Option 1

```bash
# Edit GRUB configuration
sudo vi /etc/default/grub

# Remove resume parameter: resume=UUID=25e2fbf0-5c9c-4542-b896-154dc66df34b
 
# save change for BIOS based systems
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

#### Option 2

```bash
# List default kernel information
sudo grubby --info DEFAULT

index=0
kernel="/boot/vmlinuz-5.14.0-362.18.1.el9_3.x86_64"
args="ro crashkernel=1G-4G:192M,4G-64G:256M,64G-:512M resume=UUID=25e2fbf0-5c9c-4542-b896-154dc66df34b"
root="UUID=76c3b531-ffdb-4c62-8117-ba348beaa329"
initrd="/boot/initramfs-5.14.0-362.18.1.el9_3.x86_64.img"
title="Red Hat Enterprise Linux (5.14.0-362.18.1.el9_3.x86_64) 9.3 (Plow)"
id="d30fac14f2344115850f8ee089af7047-5.14.0-362.18.1.el9_3.x86_64"

# Remove resume parameter
sudo grubby --update-kernel=ALL --remove-args="resume=UUID=25e2fbf0-5c9c-4542-b896-154dc66df34b"
```

```bash
# Verify changes
sudo grubby --info DEFAULT

index=0
kernel="/boot/vmlinuz-5.14.0-362.18.1.el9_3.x86_64"
args="ro crashkernel=1G-4G:192M,4G-64G:256M,64G-:512M"
root="UUID=76c3b531-ffdb-4c62-8117-ba348beaa329"
initrd="/boot/initramfs-5.14.0-362.18.1.el9_3.x86_64.img"
title="Red Hat Enterprise Linux (5.14.0-362.18.1.el9_3.x86_64) 9.3 (Plow)"
id="d30fac14f2344115850f8ee089af7047-5.14.0-362.18.1.el9_3.x86_64"
```


```bash
# Reboot system
sudo systemctl reboot
```



