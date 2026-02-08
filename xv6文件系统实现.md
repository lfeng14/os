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
- 结果分析：写一个数据需要调用这些接口，你想想 数据出问题的概率是不是容易失败，所以需要buffer cache,读写减少io操作开销；还有另外一种方式：write ahead log。
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
- 
