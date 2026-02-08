- gdb技巧，看代码不如运行代码
  ```
    git clone git@github.com:mit-pdos/xv6-riscv.git
    cd xv6-riscv/
    wget https://jyywiki.cn/pages/OS/2022/demos/trace.py -O mkfs/trace.py
    gcc ./mkfs/mkfs.c -g -o mkfs/mkfs -I`pwd`
    
    gdb -ex 'source mkfs/trace.py' mkfs/mkfs
  ```
- trace.py
  ```
    TRACED = 'bwrite balloc ialloc iappend rinode winode rsect wsect'.split()
    IGNORE = 'ip xp buf'.split()
    
    class trace(gdb.Breakpoint): # 继承 GDB 的 Breakpoint 类
        def stop(self):
            f, bt = gdb.selected_frame(), []
            while f and f.is_valid():
                if (name := f.name()) in TRACED:
                    lvars = [f'{sym.name}={sym.value(f)}'
                        for sym in f.block()
                        if sym.is_argument and sym.name not in IGNORE]
                    bt.append(f'\033[32m{name}\033[0m({", ".join(lvars)})')
                f = f.older()
            print('    ' * (len(bt) - 1) + bt[0])
            return False # won't stop at this breakpoint
    
    gdb.execute('set prompt off')
    gdb.execute('set pagination off')
    for fn in TRACED:
        trace(fn)
    gdb.execute('run fs.img README user/_ls')
    gdb.execute('quit')
  ```
- 宏观结果分析：写一个数据需要调用这些接口，你想想 数据出问题的概率是不是容易失败，所以需要buffer cache,读写减少io操作开销；还有另外一种方式：write ahead log。如果buf read hit就不会读取硬盘。
  ```
    iappend(inum=1, n=16)
        rinode(inum=1)
            rsect(sec=33)
        rsect(sec=47)
        wsect(sec=47)
        winode(inum=1)
            rsect(sec=33)
            wsect(sec=33)
  ```
- bug定位：cpu飙高->perf top锁定函数->确定是usb函数->物理usb有问题
- 可见gdb有很多层用法，所以gdb更多高阶用法可以继续调研。
- boot block：对于os的引导块；support block：文件系统的元数据；
- transation: all or nothing
- 这个章节有[代码导读](https://www.bilibili.com/video/BV1LT4y1B79b?spm_id_from=333.788.videopod.sections&vd_source=2211521a84d324c18aba00755ad3bcec)
- 磁盘写操作：先写日志，再落盘，最后再去做写磁盘具体数据（比如三个块，将buf搬移到硬盘）；最后标记日志完成清理日志；即使系统奔溃也有对应recover动作；
- 手搓测试框架：故障注入，qemu对指定地址写入数据则触发关机，然后保存快照：
  <img width="2066" height="1664" alt="image" src="https://github.com/user-attachments/assets/8309264d-6dd9-413b-a8fe-a884c5950076" />
