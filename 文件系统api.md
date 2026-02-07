- 软连接 可以将目录树变成一个目录图；
- 涉及api：mount point mount umount mmap read write ftruncate lseek; 
- 目录树或者目录图，可以mount虚拟设备；
- 文件系统的两大主要部分
  - 虚拟磁盘 (文件)
  - mmap, read, write, lseek, ftruncate, ...
  - 虚拟磁盘命名管理 (目录树和链接)
  - mount, chdir, mkdir, rmdir, link, unlink, symlink, open, ...
- 文件系统读取 块设备，每次读单元最小是一块；每次写先读后写同样也是以块单位；不再是random access；发现没有自底向上抽象；目录项（目录元数据）。
  ```
  Filesystem fs;
  root = fs.root();
  sd::vector<File> root.files();
  File &f = root.get_fie("home").get_file("jyy").get_file("a.txt");
  f.write();

  // bread bwrite
  ```
- fat: 以cluster为单位，目录表存放特定的位置；file allocation table: 传统，U盘兼容性好；单个大小小；FAT 是比较传统的文件系统，兼容性特别好，很多老设备都支持，但它单个文件大小和分区容量有限。UEFI分区也是fat文件系统；
  ```
  mkfs.fat -C fat512.img 1024
  ```
- ntfs：windows文件系统，能管理大文件和分区。Windows 常用的，能管理很大的文件和分区，还支持文件权限设置这些高级功能。
- apfs：mac系统对固态硬盘性能优化不错，APFS 是 Mac 系统为固态硬盘设计的，性能好，支持加密和空间共享。
- ext4：ext4 呢，是 Linux 常用的，稳定性强，适合各种 Linux 发行版，也能很好地管理磁盘空间。
- 假如你有180kb软盘，那么如何设计文件系统，尽量减少空间浪费 ？
  - 在块尾部存放next指针：块结束标志、下一块block编号（缺点lseek不友好，致命问题）；另外一个链表：空闲链表；
  <img width="1544" height="780" alt="image" src="https://github.com/user-attachments/assets/3b4f1aac-0171-4e64-9002-d40d1b49b17f" />

  - next指针单独存放，如果损坏整个盘挂了；[fat文件系统手册](https://jyywiki.cn/pages/OS/manuals/MSFAT-spec.pdf)
  <img width="2210" height="1462" alt="image" src="https://github.com/user-attachments/assets/65bc2c50-d1b4-42d7-9451-13eab6f591bc" />

-  文件系统在bread bwrite基础上抽象出来的；section < cluster < partial < volume
- 照抄手册，遍历目录树：[fatree.c](https://jyywiki.cn/pages/OS/2022/demos/fatree.c)
