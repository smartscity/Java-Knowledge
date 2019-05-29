# B&B+ Tree



### 磁盘

* heads/sectors/cylinders，分别就是磁头/扇区/柱面

```shell
# fdisk -l
Disk /dev/cciss/c0d0: 146.7 GB, 146778685440 bytes
255 heads, 63 sectors/track, 17844 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

可以看到几个名词：heads/sectors/cylinders，分别就是磁头/扇区/柱面，每个扇区512byte（现在新的硬盘每个扇区有4K）了
硬盘容量就是heads*sectors*cylinders*512=255*63*17844*512=146771896320b=146.7G

```

* xfs 文件系统  block default 4k



