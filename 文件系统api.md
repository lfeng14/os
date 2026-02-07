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
- 碎片整理，先拷贝数据，后修复fat表；为什么碎片多影响性能 ？空间局部性差 ？
- 可靠性->fat副本->性能问题，多次修改
- 磁盘格式化，其实保留数据，只是破坏了fat表这种元数据，所以xx事件才会出现。
- ext2改善了数据链表方式获取数据：通过多级索引快速访问数据：一级索引、二级索引、三级索引；大文件随机读写性能好，小文件性能没有损失；兼顾了两者。
<img width="1466" height="1244" alt="image" src="https://github.com/user-attachments/assets/58b384d3-2aa0-4fc6-9fe1-ec82e07604f6" />
<img width="1812" height="1530" alt="image" src="https://github.com/user-attachments/assets/601aec45-9975-4713-b595-b38f7a6c8400" />
<img width="1858" height="1596" alt="image" src="https://github.com/user-attachments/assets/c0ddfcf6-91c4-4da3-8814-99d76a34fb2e" />

- ext2 [inode信息](https://jyywiki.cn/pages/OS/2022/demos/ext2.h)
  ```
  struct ext2_inode {
  	__le16	i_mode;		/* File mode */
  	__le16	i_uid;		/* Low 16 bits of Owner Uid */
  	__le32	i_size;		/* Size in bytes */
  	__le32	i_atime;	/* Access time */
  	__le32	i_ctime;	/* Creation time */
  	__le32	i_mtime;	/* Modification time */
  	__le32	i_dtime;	/* Deletion Time */
  	__le16	i_gid;		/* Low 16 bits of Group Id */
  	__le16	i_links_count;	/* Links count */
  	__le32	i_blocks;	/* Blocks count */
  	__le32	i_flags;	/* File flags */
  	union {
  		struct {
  			__le32  l_i_reserved1;
  		} linux1;
  		struct {
  			__le32  h_i_translator;
  		} hurd1;
  		struct {
  			__le32  m_i_reserved1;
  		} masix1;
  	} osd1;				/* OS dependent 1 */
  	__le32	i_block[EXT2_N_BLOCKS];/* Pointers to blocks */
  	__le32	i_generation;	/* File version (for NFS) */
  	__le32	i_file_acl;	/* File ACL */
  	__le32	i_dir_acl;	/* Directory ACL */
  	__le32	i_faddr;	/* Fragment address */
  	union {
  		struct {
  			__u8	l_i_frag;	/* Fragment number */
  			__u8	l_i_fsize;	/* Fragment size */
  			__u16	i_pad1;
  			__le16	l_i_uid_high;	/* these 2 fields    */
  			__le16	l_i_gid_high;	/* were reserved2[0] */
  			__u32	l_i_reserved2;
  		} linux2;
  		struct {
  			__u8	h_i_frag;	/* Fragment number */
  			__u8	h_i_fsize;	/* Fragment size */
  			__le16	h_i_mode_high;
  			__le16	h_i_uid_high;
  			__le16	h_i_gid_high;
  			__le32	h_i_author;
  		} hurd2;
  		struct {
  			__u8	m_i_frag;	/* Fragment number */
  			__u8	m_i_fsize;	/* Fragment size */
  			__u16	m_pad1;
  			__u32	m_i_reserved2[2];
  		} masix2;
  	} osd2;				/* OS dependent 2 */
  };
  ```
