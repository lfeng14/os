- [xv6: a simple, Unix-like teaching operating system](https://pdos.csail.mit.edu/6.828/2023/xv6/book-riscv-rev3.pdf)
- 通过gcc生成.o文件依赖哪些文件，如果修改就需要重新构建；这是随着项目经验逐渐扩大后出现的；所以很多事情是先运转起来然后解决问题；
- <img width="856" height="68" alt="image" src="https://github.com/user-attachments/assets/c454a853-6f68-4c2b-bdc8-994a44e03d35" />
- make -nB qemu干跑输出构建命令
  ```
  Usage: make [options] [target] ...
  Options:
    -b, -m                      Ignored for compatibility.
    -B, --always-make           Unconditionally make all targets.
    -C DIRECTORY, --directory=DIRECTORY
                                Change to DIRECTORY before doing anything.
    -d                          Print lots of debugging information.
    --debug[=FLAGS]             Print various types of debugging information.
    -e, --environment-overrides
                                Environment variables override makefiles.
    -E STRING, --eval=STRING    Evaluate STRING as a makefile statement.
    -f FILE, --file=FILE, --makefile=FILE
                                Read FILE as a makefile.
    -h, --help                  Print this message and exit.
    -i, --ignore-errors         Ignore errors from recipes.
    -I DIRECTORY, --include-dir=DIRECTORY
                                Search DIRECTORY for included makefiles.
    -j [N], --jobs[=N]          Allow N jobs at once; infinite jobs with no arg.
    -k, --keep-going            Keep going when some targets can't be made.
    -l [N], --load-average[=N], --max-load[=N]
                                Don't start multiple jobs unless load is below N.
    -L, --check-symlink-times   Use the latest mtime between symlinks and target.
    -n, --just-print, --dry-run, --recon
                                Don't actually run any recipe; just print them.
  ```
  <img width="1044" height="688" alt="image" src="https://github.com/user-attachments/assets/33956e86-2baf-421b-b296-b627e85fd9b4" />

- x/10i 0  显示10条0地址的指令
  <img width="828" height="1062" alt="image" src="https://github.com/user-attachments/assets/708d46dc-e6d6-45fa-85c3-3e228fe202f4" />

- 打印寄存器内容，需要加美元符号，p/x $sscratch; p/x $a0; p/x *(trapframe*)$a0;
  <img width="930" height="326" alt="image" src="https://github.com/user-attachments/assets/bc2aa9b7-c42a-46b2-b206-dd17d48e99c4" />

- 舒服是很重要的
