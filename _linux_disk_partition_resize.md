In some modern Linux operating systems, after conducting a disk resize, a system reboot usually takes care of growing a partition and resizing a file system.
For example, in AWS cloud init modules take care of the task making this change more or less seemless.  

HOWEVER... if you cannot reboot a live prod server, here are some steps to grow a disk partition without rebooting your system. 

`linux_disk_resize.txt`

```shell

## list and identify the filesystem block devices
user@linux:~$ sudo lsblk

NAME          MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
nvme0n1       259:0    0  80G  0 disk
├─nvme0n1p1   259:1    0  40G  0 part /
└─nvme0n1p128 259:2    0   1M  0 part


## grow the target disk partition
user@linux:~$ sudo growpart /dev/nvme0n1 1


## identify the filesystem type 
user@linux:~$ df -Th
Filesystem                              	Type  	Size  Used Avail Use% Mounted on
devtmpfs                                	devtmpfs   16G 	0   16G   0% /dev
tmpfs                                   	tmpfs  	16G 	0   16G   0% /dev/shm
tmpfs                                   	tmpfs  	16G  384K   16G   1% /run
tmpfs                                   	tmpfs  	16G 	0   16G   0% /sys/fs/cgroup
/dev/nvme0n1p1                          	xfs    	80G   35G   46G  44% /


## in this case it is an xfs type filesystem
## for XFS filesystems
user@linux:~$ sudo xfs_growfs -d /


## if it is ext4 type, you can use the following command instead
user@linux:~$ sudo resize2fs /dev/nvme0n1p1

```


