-  示例
```
franz@ubuntu:~/src$ cat demo.c 
#include <unistd.h>
#include <stdio.h>

int main() {
 char * const argv[] = {
  "/bin/bash","-c","env", NULL, 
 };
 char * const envp[] = {
  "HELLO=WORLD", NULL, 
 };
 execve("/bin/bash", argv, envp);
 print("Hello, World!\n");
}
```
-  构建
  ```
  gcc demo.c -o demo
  ```
-  用strace分析执行哪些系统调用
  ```
  strace ./demo
  execve("./demo", ["./demo"], 0xffffd7f1fcf0 /* 23 vars */) = 0
  brk(NULL)                               = 0xc7fe3e0e9000
  mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xe45ddc4e1000
  faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
  openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=34027, ...}) = 0
  mmap(NULL, 34027, PROT_READ, MAP_PRIVATE, 3, 0) = 0xe45ddc4d8000
  close(3)                                = 0
  openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
  read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\360\206\2\0\0\0\0\0"..., 832) = 832
  fstat(3, {st_mode=S_IFREG|0755, st_size=1722920, ...}) = 0
  mmap(NULL, 1892240, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_DENYWRITE, -1, 0) = 0xe45ddc2da000
  mmap(0xe45ddc2e0000, 1826704, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0) = 0xe45ddc2e0000
  munmap(0xe45ddc2da000, 24576)           = 0
  munmap(0xe45ddc49e000, 40848)           = 0
  mprotect(0xe45ddc479000, 81920, PROT_NONE) = 0
  mmap(0xe45ddc48d000, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19d000) = 0xe45ddc48d000
  mmap(0xe45ddc492000, 49040, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xe45ddc492000
  close(3)                                = 0
  set_tid_address(0xe45ddc4e1fb0)         = 405945
  set_robust_list(0xe45ddc4e1fc0, 24)     = 0
  rseq(0xe45ddc4e2600, 0x20, 0, 0xd428bc00) = 0
  mprotect(0xe45ddc48d000, 12288, PROT_READ) = 0
  mprotect(0xc7fe29e3f000, 4096, PROT_READ) = 0
  mprotect(0xe45ddc4e6000, 8192, PROT_READ) = 0
  prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
  munmap(0xe45ddc4d8000, 34027)           = 0
  execve("/bin/bash", ["/bin/bash", "-c", "env"], 0xffffc85d8118 /* 1 var */) = 0
  brk(NULL)                               = 0xb39daef54000
  mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xe71ed0ae6000
  faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
  openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=34027, ...}) = 0
  mmap(NULL, 34027, PROT_READ, MAP_PRIVATE, 3, 0) = 0xe71ed0add000
  close(3)                                = 0
  openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libtinfo.so.6", O_RDONLY|O_CLOEXEC) = 3
  read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\0\0\0\0\0\0\0\0"..., 832) = 832
  fstat(3, {st_mode=S_IFREG|0644, st_size=265416, ...}) = 0
  mmap(NULL, 329928, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_DENYWRITE, -1, 0) = 0xe71ed0a5c000
  mmap(0xe71ed0a60000, 264392, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0) = 0xe71ed0a60000
  munmap(0xe71ed0a5c000, 16384)           = 0
  munmap(0xe71ed0aa1000, 47304)           = 0
  mprotect(0xe71ed0a8f000, 53248, PROT_NONE) = 0
  mmap(0xe71ed0a9c000, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x3c000) = 0xe71ed0a9c000
  close(3)                                = 0
  openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
  read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\360\206\2\0\0\0\0\0"..., 832) = 832
  fstat(3, {st_mode=S_IFREG|0755, st_size=1722920, ...}) = 0
  mmap(NULL, 1892240, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_DENYWRITE, -1, 0) = 0xe71ed0892000
  mmap(0xe71ed08a0000, 1826704, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0) = 0xe71ed08a0000
  munmap(0xe71ed0892000, 57344)           = 0
  munmap(0xe71ed0a5e000, 8080)            = 0
  mprotect(0xe71ed0a39000, 81920, PROT_NONE) = 0
  mmap(0xe71ed0a4d000, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19d000) = 0xe71ed0a4d000
  mmap(0xe71ed0a52000, 49040, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xe71ed0a52000
  close(3)                                = 0
  mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xe71ed0adb000
  set_tid_address(0xe71ed0adb0f0)         = 405945
  set_robust_list(0xe71ed0adb100, 24)     = 0
  rseq(0xe71ed0adb740, 0x20, 0, 0xd428bc00) = 0
  mprotect(0xe71ed0a4d000, 12288, PROT_READ) = 0
  mprotect(0xe71ed0a9c000, 16384, PROT_READ) = 0
  mprotect(0xb39d909bb000, 20480, PROT_READ) = 0
  mprotect(0xe71ed0aeb000, 8192, PROT_READ) = 0
  prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
  munmap(0xe71ed0add000, 34027)           = 0
  openat(AT_FDCWD, "/dev/tty", O_RDWR|O_NONBLOCK) = 3
  close(3)                                = 0
  getrandom("\xc3\x45\x16\x41\xd5\xf3\x38\xb9", 8, GRND_NONBLOCK) = 8
  brk(NULL)                               = 0xb39daef54000
  brk(0xb39daef75000)                     = 0xb39daef75000
  getuid()                                = 1000
  getgid()                                = 1000
  geteuid()                               = 1000
  getegid()                               = 1000
  rt_sigprocmask(SIG_BLOCK, NULL, [], 8)  = 0
  rt_sigaction(SIGCHLD, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGCHLD, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTART}, 8) = 0
  rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGQUIT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGQUIT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTSTP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTSTP, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTTIN, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTTIN, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTTOU, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTTOU, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigprocmask(SIG_BLOCK, NULL, [], 8)  = 0
  rt_sigaction(SIGQUIT, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  uname({sysname="Linux", nodename="ubuntu", ...}) = 0
  getcwd("/home/franz/src", 4096)         = 16
  getpid()                                = 405945
  getppid()                               = 405942
  socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
  connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
  close(3)                                = 0
  socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
  connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
  close(3)                                = 0
  newfstatat(AT_FDCWD, "/etc/nsswitch.conf", {st_mode=S_IFREG|0644, st_size=526, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  openat(AT_FDCWD, "/etc/nsswitch.conf", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=526, ...}) = 0
  read(3, "# /etc/nsswitch.conf\n#\n# Example"..., 4096) = 526
  read(3, "", 4096)                       = 0
  fstat(3, {st_mode=S_IFREG|0644, st_size=526, ...}) = 0
  close(3)                                = 0
  openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=1757, ...}) = 0
  lseek(3, 0, SEEK_SET)                   = 0
  read(3, "root:x:0:0:root:/root:/bin/bash\n"..., 4096) = 1757
  close(3)                                = 0
  getpid()                                = 405945
  getppid()                               = 405942
  getpid()                                = 405945
  getppid()                               = 405942
  getpgid(0)                              = 405942
  ioctl(2, TIOCGPGRP, [405942])           = 0
  rt_sigaction(SIGCHLD, {sa_handler=0xb39d908a7d20, sa_mask=[], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTART}, 8) = 0
  prlimit64(0, RLIMIT_NPROC, NULL, {rlim_cur=79540, rlim_max=79540}) = 0
  rt_sigprocmask(SIG_BLOCK, NULL, [], 8)  = 0
  getpeername(0, 0xffffd5e56258, [16])    = -1 ENOTSOCK (Socket operation on non-socket)
  rt_sigprocmask(SIG_BLOCK, NULL, [], 8)  = 0
  newfstatat(AT_FDCWD, ".", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/local/sbin/env", 0xffffd5e55a78, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/local/bin/env", 0xffffd5e55a78, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/sbin/env", 0xffffd5e55a78, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/bin/env", {st_mode=S_IFREG|0755, st_size=68368, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/bin/env", {st_mode=S_IFREG|0755, st_size=68368, ...}, 0) = 0
  geteuid()                               = 1000
  getegid()                               = 1000
  getuid()                                = 1000
  getgid()                                = 1000
  faccessat(AT_FDCWD, "/usr/bin/env", X_OK) = 0
  newfstatat(AT_FDCWD, "/usr/bin/env", {st_mode=S_IFREG|0755, st_size=68368, ...}, 0) = 0
  geteuid()                               = 1000
  getegid()                               = 1000
  getuid()                                = 1000
  getgid()                                = 1000
  faccessat(AT_FDCWD, "/usr/bin/env", R_OK) = 0
  rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGQUIT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, {sa_handler=SIG_IGN, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGCHLD, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTART}, {sa_handler=0xb39d908a7d20, sa_mask=[], sa_flags=SA_RESTART}, 8) = 0
  execve("/usr/bin/env", ["env"], 0xb39daef60640 /* 4 vars */) = 0
  brk(NULL)                               = 0xc369e15f8000
  mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xe57df918e000
  faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
  openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=34027, ...}) = 0
  mmap(NULL, 34027, PROT_READ, MAP_PRIVATE, 3, 0) = 0xe57df9185000
  close(3)                                = 0
  openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
  read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\360\206\2\0\0\0\0\0"..., 832) = 832
  fstat(3, {st_mode=S_IFREG|0755, st_size=1722920, ...}) = 0
  mmap(NULL, 1892240, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_DENYWRITE, -1, 0) = 0xe57df8f87000
  mmap(0xe57df8f90000, 1826704, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0) = 0xe57df8f90000
  munmap(0xe57df8f87000, 36864)           = 0
  munmap(0xe57df914e000, 28560)           = 0
  mprotect(0xe57df9129000, 81920, PROT_NONE) = 0
  mmap(0xe57df913d000, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19d000) = 0xe57df913d000
  mmap(0xe57df9142000, 49040, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xe57df9142000
  close(3)                                = 0
  set_tid_address(0xe57df918eff0)         = 405945
  set_robust_list(0xe57df918f000, 24)     = 0
  rseq(0xe57df918f640, 0x20, 0, 0xd428bc00) = 0
  mprotect(0xe57df913d000, 12288, PROT_READ) = 0
  mprotect(0xc369a9faf000, 4096, PROT_READ) = 0
  mprotect(0xe57df9193000, 8192, PROT_READ) = 0
  prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
  munmap(0xe57df9185000, 34027)           = 0
  getrandom("\x42\xf3\x04\xe6\x99\x36\xa1\xd0", 8, GRND_NONBLOCK) = 8
  brk(NULL)                               = 0xc369e15f8000
  brk(0xc369e1619000)                     = 0xc369e1619000
  fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0), ...}) = 0
  write(1, "PWD=/home/franz/src\n", 20PWD=/home/franz/src
  )   = 20
  write(1, "HELLO=WORLD\n", 12HELLO=WORLD
  )           = 12
  write(1, "SHLVL=0\n", 8SHLVL=0
  )                = 8
  write(1, "_=/usr/bin/env\n", 15_=/usr/bin/env
  )        = 15
  close(1)                                = 0
  close(2)                                = 0
  exit_group(0)                           = ?
  +++ exited with 0 +++
  ```
-  说明linux系统内可以通过fork拷贝状态机也可以通过execve命令reset重置状态机，通过exit、_exit、__exit来销毁线程、进程等
-  同样也可以通过strace命令来分析gcc构建时调用了哪些
  ```
  strace gcc demo.c -o demo
  execve("/usr/bin/gcc", ["gcc", "demo.c", "-o", "demo"], 0xffffda670c58 /* 23 vars */) = 0
  brk(NULL)                               = 0xd74c000
  mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xebb2e4160000
  faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
  openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=34027, ...}) = 0
  mmap(NULL, 34027, PROT_READ, MAP_PRIVATE, 3, 0) = 0xebb2e4157000
  close(3)                                = 0
  openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
  read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\360\206\2\0\0\0\0\0"..., 832) = 832
  fstat(3, {st_mode=S_IFREG|0755, st_size=1722920, ...}) = 0
  mmap(NULL, 1892240, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_DENYWRITE, -1, 0) = 0xebb2e3f59000
  mmap(0xebb2e3f60000, 1826704, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0) = 0xebb2e3f60000
  munmap(0xebb2e3f59000, 28672)           = 0
  munmap(0xebb2e411e000, 36752)           = 0
  mprotect(0xebb2e40f9000, 81920, PROT_NONE) = 0
  mmap(0xebb2e410d000, 20480, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19d000) = 0xebb2e410d000
  mmap(0xebb2e4112000, 49040, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xebb2e4112000
  close(3)                                = 0
  set_tid_address(0xebb2e4161050)         = 405962
  set_robust_list(0xebb2e4161060, 24)     = 0
  rseq(0xebb2e41616a0, 0x20, 0, 0xd428bc00) = 0
  mprotect(0xebb2e410d000, 12288, PROT_READ) = 0
  mprotect(0x4fe000, 8192, PROT_READ)     = 0
  mprotect(0xebb2e4165000, 8192, PROT_READ) = 0
  prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
  munmap(0xebb2e4157000, 34027)           = 0
  getrandom("\xd7\x21\x0b\x71\x6e\x4a\x65\x68", 8, GRND_NONBLOCK) = 8
  brk(NULL)                               = 0xd74c000
  brk(0xd76d000)                          = 0xd76d000
  brk(0xd78f000)                          = 0xd78f000
  openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=3055776, ...}) = 0
  mmap(NULL, 3055776, PROT_READ, MAP_PRIVATE, 3, 0) = 0xebb2e3c00000
  close(3)                                = 0
  openat(AT_FDCWD, "/usr/share/locale/locale.alias", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=2996, ...}) = 0
  read(3, "# Locale name alias data base.\n#"..., 4096) = 2996
  read(3, "", 4096)                       = 0
  close(3)                                = 0
  openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_CTYPE", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
  openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_CTYPE", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=360460, ...}) = 0
  mmap(NULL, 360460, PROT_READ, MAP_PRIVATE, 3, 0) = 0xebb2e3f07000
  close(3)                                = 0
  openat(AT_FDCWD, "/usr/lib/aarch64-linux-gnu/gconv/gconv-modules.cache", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=27028, ...}) = 0
  mmap(NULL, 27028, PROT_READ, MAP_SHARED, 3, 0) = 0xebb2e4159000
  close(3)                                = 0
  futex(0xebb2e411166c, FUTEX_WAKE_PRIVATE, 2147483647) = 0
  openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_MESSAGES", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
  openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_MESSAGES", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
  close(3)                                = 0
  openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_MESSAGES/SYS_LC_MESSAGES", O_RDONLY|O_CLOEXEC) = 3
  fstat(3, {st_mode=S_IFREG|0644, st_size=48, ...}) = 0
  mmap(NULL, 48, PROT_READ, MAP_PRIVATE, 3, 0) = 0xebb2e4158000
  close(3)                                = 0
  ioctl(2, TCGETS, {c_iflag=ICRNL|IXON|IXANY|IMAXBEL|IUTF8, c_oflag=NL0|CR0|TAB0|BS0|VT0|FF0|OPOST|ONLCR, c_cflag=B9600|CS8|CREAD, c_lflag=ISIG|ICANON|ECHO|ECHOE|IEXTEN|ECHOCTL|ECHOKE|PENDIN, ...}) = 0
  ioctl(0, TIOCGWINSZ, {ws_row=62, ws_col=244, ws_xpixel=1708, ws_ypixel=877}) = 0
  ioctl(2, TCGETS, {c_iflag=ICRNL|IXON|IXANY|IMAXBEL|IUTF8, c_oflag=NL0|CR0|TAB0|BS0|VT0|FF0|OPOST|ONLCR, c_cflag=B9600|CS8|CREAD, c_lflag=ISIG|ICANON|ECHO|ECHOE|IEXTEN|ECHOCTL|ECHOKE|PENDIN, ...}) = 0
  ioctl(2, TCGETS, {c_iflag=ICRNL|IXON|IXANY|IMAXBEL|IUTF8, c_oflag=NL0|CR0|TAB0|BS0|VT0|FF0|OPOST|ONLCR, c_cflag=B9600|CS8|CREAD, c_lflag=ISIG|ICANON|ECHO|ECHOE|IEXTEN|ECHOCTL|ECHOKE|PENDIN, ...}) = 0
  rt_sigaction(SIGINT, {sa_handler=SIG_IGN, sa_mask=[INT], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGINT, {sa_handler=0x408e68, sa_mask=[INT], sa_flags=SA_RESTART}, {sa_handler=SIG_IGN, sa_mask=[INT], sa_flags=SA_RESTART}, 8) = 0
  rt_sigaction(SIGHUP, {sa_handler=SIG_IGN, sa_mask=[HUP], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGHUP, {sa_handler=0x408e68, sa_mask=[HUP], sa_flags=SA_RESTART}, {sa_handler=SIG_IGN, sa_mask=[HUP], sa_flags=SA_RESTART}, 8) = 0
  rt_sigaction(SIGTERM, {sa_handler=SIG_IGN, sa_mask=[TERM], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGTERM, {sa_handler=0x408e68, sa_mask=[TERM], sa_flags=SA_RESTART}, {sa_handler=SIG_IGN, sa_mask=[TERM], sa_flags=SA_RESTART}, 8) = 0
  rt_sigaction(SIGPIPE, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  rt_sigaction(SIGPIPE, {sa_handler=0x408e68, sa_mask=[PIPE], sa_flags=SA_RESTART}, {sa_handler=SIG_IGN, sa_mask=[PIPE], sa_flags=SA_RESTART}, 8) = 0
  rt_sigaction(SIGCHLD, {sa_handler=SIG_DFL, sa_mask=[CHLD], sa_flags=SA_RESTART}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
  prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
  prlimit64(0, RLIMIT_STACK, {rlim_cur=65536*1024, rlim_max=RLIM64_INFINITY}, NULL) = 0
  faccessat(AT_FDCWD, "/usr/local/sbin/gcc", X_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/local/bin/gcc", X_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/sbin/gcc", X_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/bin/gcc", X_OK) = 0
  newfstatat(AT_FDCWD, "/usr/bin/gcc", {st_mode=S_IFREG|0755, st_size=990040, ...}, 0) = 0
  readlinkat(AT_FDCWD, "/usr", 0xffffdc007c80, 1023) = -1 EINVAL (Invalid argument)
  readlinkat(AT_FDCWD, "/usr/bin", 0xffffdc007c80, 1023) = -1 EINVAL (Invalid argument)
  readlinkat(AT_FDCWD, "/usr/bin/gcc", "gcc-13", 1023) = 6
  readlinkat(AT_FDCWD, "/usr/bin/gcc-13", "aarch64-linux-gnu-gcc-13", 1023) = 24
  readlinkat(AT_FDCWD, "/usr/bin/aarch64-linux-gnu-gcc-13", 0xffffdc007c80, 1023) = -1 EINVAL (Invalid argument)
  faccessat(AT_FDCWD, "/usr/local/sbin/gcc", X_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/local/bin/gcc", X_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/sbin/gcc", X_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/bin/gcc", X_OK) = 0
  newfstatat(AT_FDCWD, "/usr/bin/gcc", {st_mode=S_IFREG|0755, st_size=990040, ...}, 0) = 0
  readlinkat(AT_FDCWD, "/usr", 0xffffdc007c80, 1023) = -1 EINVAL (Invalid argument)
  readlinkat(AT_FDCWD, "/usr/bin", 0xffffdc007c80, 1023) = -1 EINVAL (Invalid argument)
  readlinkat(AT_FDCWD, "/usr/bin/gcc", "gcc-13", 1023) = 6
  readlinkat(AT_FDCWD, "/usr/bin/gcc-13", "aarch64-linux-gnu-gcc-13", 1023) = 24
  readlinkat(AT_FDCWD, "/usr/bin/aarch64-linux-gnu-gcc-13", 0xffffdc007c80, 1023) = -1 EINVAL (Invalid argument)
  getcwd("/home/franz/src", 1024)         = 16
  readlinkat(AT_FDCWD, "/home/franz/src/demo.c", 0xffffdc007e20, 1023) = -1 EINVAL (Invalid argument)
  getcwd("/home/franz/src", 1024)         = 16
  readlinkat(AT_FDCWD, "/home/franz/src/demo", 0xffffdc007e20, 1023) = -1 EINVAL (Invalid argument)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/", X_OK) = 0
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/", X_OK) = 0
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/specs", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/aarch64-linux-gnu/13/specs", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/specs", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/specs", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/", X_OK) = 0
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/lto-wrapper", {st_mode=S_IFREG|0755, st_size=856192, ...}, 0) = 0
  faccessat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/lto-wrapper", X_OK) = 0
  faccessat(AT_FDCWD, "/tmp", R_OK|W_OK|X_OK) = 0
  newfstatat(AT_FDCWD, "/tmp", {st_mode=S_IFDIR|S_ISVTX|0777, st_size=20480, ...}, 0) = 0
  openat(AT_FDCWD, "/tmp/cchosBzC.s", O_RDWR|O_CREAT|O_EXCL, 0600) = 3
  close(3)                                = 0
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/cc1", {st_mode=S_IFREG|0755, st_size=26761448, ...}, 0) = 0
  faccessat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/cc1", X_OK) = 0
  pipe2([3, 4], O_CLOEXEC)                = 0
  clone(child_stack=0xffffdc0083d0, flags=CLONE_VM|CLONE_VFORK|SIGCHLD) = 405963
  close(4)                                = 0
  read(3, "", 16)                         = 0
  close(3)                                = 0
  wait4(405963, [{WIFEXITED(s) && WEXITSTATUS(s) == 0}], 0, NULL) = 405963
  --- SIGCHLD {si_signo=SIGCHLD, si_code=CLD_EXITED, si_pid=405963, si_uid=1000, si_status=0, si_utime=0, si_stime=0} ---
  openat(AT_FDCWD, "/tmp/ccPcgQpG.o", O_RDWR|O_CREAT|O_EXCL, 0600) = 3
  close(3)                                = 0
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/aarch64-linux-gnu-as", 0xffffdc009100, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/as", 0xffffdc009160, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/aarch64-linux-gnu-as", 0xffffdc009100, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/as", 0xffffdc009160, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/aarch64-linux-gnu-as", 0xffffdc009100, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/as", 0xffffdc009160, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/aarch64-linux-gnu-as", 0xffffdc009100, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/as", 0xffffdc009160, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/aarch64-linux-gnu-as", 0xffffdc009100, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/as", 0xffffdc009160, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/bin/aarch64-linux-gnu/13/aarch64-linux-gnu-as", 0xffffdc009100, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/bin/aarch64-linux-gnu/13/as", 0xffffdc009160, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/bin/aarch64-linux-gnu/aarch64-linux-gnu-as", 0xffffdc009100, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/bin/aarch64-linux-gnu/as", 0xffffdc009160, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/bin/aarch64-linux-gnu-as", 0xffffdc009100, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/bin/as", 0xffffdc009160, 0) = -1 ENOENT (No such file or directory)
  pipe2([3, 4], O_CLOEXEC)                = 0
  clone(child_stack=0xffffdc009200, flags=CLONE_VM|CLONE_VFORK|SIGCHLD) = 405964
  close(4)                                = 0
  read(3, "", 16)                         = 0
  close(3)                                = 0
  wait4(405964, [{WIFEXITED(s) && WEXITSTATUS(s) == 0}], 0, NULL) = 405964
  --- SIGCHLD {si_signo=SIGCHLD, si_code=CLD_EXITED, si_pid=405964, si_uid=1000, si_status=0, si_utime=0, si_stime=0} ---
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/collect2", {st_mode=S_IFREG|0755, st_size=331800, ...}, 0) = 0
  faccessat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/collect2", X_OK) = 0
  faccessat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/liblto_plugin.so", R_OK) = 0
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/bin/aarch64-linux-gnu/13/.", 0xffffdc0090a0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/bin/aarch64-linux-gnu/.", 0xffffdc0090a0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/bin/.", 0xffffdc0090a0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/aarch64-linux-gnu/13/.", 0xffffdc0090a0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/aarch64-linux-gnu/.", 0xffffdc0090a0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/../lib/.", 0xffffdc0090a0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../aarch64-linux-gnu/13/.", 0xffffdc0090a0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../aarch64-linux-gnu/.", {st_mode=S_IFDIR|0755, st_size=36864, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../lib/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/lib/aarch64-linux-gnu/13/.", 0xffffdc0090a0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/lib/aarch64-linux-gnu/.", {st_mode=S_IFDIR|0755, st_size=36864, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/lib/../lib/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/aarch64-linux-gnu/13/.", 0xffffdc0090a0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/aarch64-linux-gnu/.", {st_mode=S_IFDIR|0755, st_size=36864, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/../lib/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/.", 0xffffdc0090a0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/lib/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  openat(AT_FDCWD, "/tmp/ccjk9Pnk.res", O_RDWR|O_CREAT|O_EXCL, 0600) = 3
  close(3)                                = 0
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/Scrt1.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/aarch64-linux-gnu/13/Scrt1.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/aarch64-linux-gnu/Scrt1.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/../lib/Scrt1.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../aarch64-linux-gnu/13/Scrt1.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../aarch64-linux-gnu/Scrt1.o", R_OK) = 0
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/crti.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/aarch64-linux-gnu/13/crti.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/aarch64-linux-gnu/crti.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/../lib/crti.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../aarch64-linux-gnu/13/crti.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../aarch64-linux-gnu/crti.o", R_OK) = 0
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/crtbeginS.o", R_OK) = 0
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/aarch64-linux-gnu/13/.", 0xffffdc0082b0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/aarch64-linux-gnu/.", 0xffffdc0082b0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/../lib/.", 0xffffdc0082b0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../aarch64-linux-gnu/13/.", 0xffffdc0082b0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../aarch64-linux-gnu/.", {st_mode=S_IFDIR|0755, st_size=36864, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../lib/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/lib/aarch64-linux-gnu/13/.", 0xffffdc0082b0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/lib/aarch64-linux-gnu/.", {st_mode=S_IFDIR|0755, st_size=36864, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/lib/../lib/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/aarch64-linux-gnu/13/.", 0xffffdc0082b0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/aarch64-linux-gnu/.", {st_mode=S_IFDIR|0755, st_size=36864, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/../lib/.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/.", 0xffffdc0082b0, 0) = -1 ENOENT (No such file or directory)
  newfstatat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../.", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/crtendS.o", R_OK) = 0
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/crtn.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/aarch64-linux-gnu/13/crtn.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/aarch64-linux-gnu/crtn.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../../aarch64-linux-gnu/lib/../lib/crtn.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../aarch64-linux-gnu/13/crtn.o", R_OK) = -1 ENOENT (No such file or directory)
  faccessat(AT_FDCWD, "/usr/lib/gcc/aarch64-linux-gnu/13/../../../aarch64-linux-gnu/crtn.o", R_OK) = 0
  newfstatat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/collect2", {st_mode=S_IFREG|0755, st_size=331800, ...}, 0) = 0
  faccessat(AT_FDCWD, "/usr/libexec/gcc/aarch64-linux-gnu/13/collect2", X_OK) = 0
  pipe2([3, 4], O_CLOEXEC)                = 0
  clone(child_stack=0xffffdc008340, flags=CLONE_VM|CLONE_VFORK|SIGCHLD) = 405965
  close(4)                                = 0
  read(3, "", 16)                         = 0
  close(3)                                = 0
  wait4(405965, [{WIFEXITED(s) && WEXITSTATUS(s) == 0}], 0, NULL) = 405965
  --- SIGCHLD {si_signo=SIGCHLD, si_code=CLD_EXITED, si_pid=405965, si_uid=1000, si_status=0, si_utime=0, si_stime=0} ---
  newfstatat(AT_FDCWD, "/tmp/ccjk9Pnk.res", {st_mode=S_IFREG|0600, st_size=0, ...}, 0) = 0
  unlinkat(AT_FDCWD, "/tmp/ccjk9Pnk.res", 0) = 0
  newfstatat(AT_FDCWD, "/tmp/ccPcgQpG.o", {st_mode=S_IFREG|0600, st_size=2384, ...}, 0) = 0
  unlinkat(AT_FDCWD, "/tmp/ccPcgQpG.o", 0) = 0
  newfstatat(AT_FDCWD, "/tmp/cchosBzC.s", {st_mode=S_IFREG|0600, st_size=1455, ...}, 0) = 0
  unlinkat(AT_FDCWD, "/tmp/cchosBzC.s", 0) = 0
  exit_group(0)                           = ?
  +++ exited with 0 +++
  ```
