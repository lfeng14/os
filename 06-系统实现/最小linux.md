- 程序都是init程序的子孙后代
- readelf -l 与 pmap 【pid】、gdb查看地址空间查看进程地址空间；哪些段只读、读写、可执行；地址空间布局随机化、地址空间隔离
  ```
  readelf -l ./demo
  
  Elf file type is DYN (Position-Independent Executable file)
  Entry point 0x7c0
  There are 9 program headers, starting at offset 64
  
  Program Headers:
    Type           Offset             VirtAddr           PhysAddr
                   FileSiz            MemSiz              Flags  Align
    PHDR           0x0000000000000040 0x0000000000000040 0x0000000000000040
                   0x00000000000001f8 0x00000000000001f8  R      0x8
    INTERP         0x0000000000000238 0x0000000000000238 0x0000000000000238
                   0x000000000000001b 0x000000000000001b  R      0x1
        [Requesting program interpreter: /lib/ld-linux-aarch64.so.1]
    LOAD           0x0000000000000000 0x0000000000000000 0x0000000000000000
                   0x0000000000000acc 0x0000000000000acc  R E    0x10000
    LOAD           0x000000000000fd68 0x000000000001fd68 0x000000000001fd68
                   0x00000000000002c8 0x00000000000002d0  RW     0x10000
    DYNAMIC        0x000000000000fd78 0x000000000001fd78 0x000000000001fd78
                   0x0000000000000200 0x0000000000000200  RW     0x8
    NOTE           0x0000000000000254 0x0000000000000254 0x0000000000000254
                   0x0000000000000044 0x0000000000000044  R      0x4
    GNU_EH_FRAME   0x00000000000009dc 0x00000000000009dc 0x00000000000009dc
                   0x000000000000003c 0x000000000000003c  R      0x4
    GNU_STACK      0x0000000000000000 0x0000000000000000 0x0000000000000000
                   0x0000000000000000 0x0000000000000000  RW     0x10
    GNU_RELRO      0x000000000000fd68 0x000000000001fd68 0x000000000001fd68
                   0x0000000000000298 0x0000000000000298  R      0x1

      (gdb) info proc mappings
    process 406196
    Mapped address spaces:
    
              Start Addr           End Addr       Size     Offset  Perms  objfile
          0xaaaaaaaa0000     0xaaaaaaaa1000     0x1000        0x0  r-xp   /home/franz/src/demo
          0xaaaaaaabf000     0xaaaaaaac0000     0x1000     0xf000  r--p   /home/franz/src/demo
          0xaaaaaaac0000     0xaaaaaaac1000     0x1000    0x10000  rw-p   /home/franz/src/demo
          0xfffff7df0000     0xfffff7f89000   0x199000        0x0  r-xp   /usr/lib/aarch64-linux-gnu/libc.so.6
          0xfffff7f89000     0xfffff7f9d000    0x14000   0x199000  ---p   /usr/lib/aarch64-linux-gnu/libc.so.6
          0xfffff7f9d000     0xfffff7fa0000     0x3000   0x19d000  r--p   /usr/lib/aarch64-linux-gnu/libc.so.6
          0xfffff7fa0000     0xfffff7fa2000     0x2000   0x1a0000  rw-p   /usr/lib/aarch64-linux-gnu/libc.so.6
          0xfffff7fa2000     0xfffff7fae000     0xc000        0x0  rw-p   
          0xfffff7fbe000     0xfffff7fe5000    0x27000        0x0  r-xp   /usr/lib/aarch64-linux-gnu/ld-linux-aarch64.so.1
          0xfffff7ff7000     0xfffff7ff9000     0x2000        0x0  rw-p   
          0xfffff7ff9000     0xfffff7ffb000     0x2000        0x0  r--p   [vvar]
          0xfffff7ffb000     0xfffff7ffc000     0x1000        0x0  r-xp   [vdso]
          0xfffff7ffc000     0xfffff7ffe000     0x2000    0x2e000  r--p   /usr/lib/aarch64-linux-gnu/ld-linux-aarch64.so.1
          0xfffff7ffe000     0xfffff8000000     0x2000    0x30000  rw-p   /usr/lib/aarch64-linux-gnu/ld-linux-aarch64.so.1
          0xfffffffdf000    0x1000000000000    0x21000        0x0  rw-p   [stack]
  ```
  <img width="1056" height="776" alt="image" src="https://github.com/user-attachments/assets/39c0a967-9670-45d2-b95d-769b11b4b245" />

- 进程通过mmap加载glibc，也可以通过mmap开辟一段大的地址空间
  <img width="2070" height="1148" alt="image" src="https://github.com/user-attachments/assets/8af6ffac-182a-437d-9482-dc6231035aa7" />

  <img width="2052" height="970" alt="image" src="https://github.com/user-attachments/assets/93e51d9c-52df-4143-b28b-e6e69eeacddd" />

- 通过挂载宿主机程序到虚拟机，就可以使用多样工具
<img width="2146" height="1194" alt="image" src="https://github.com/user-attachments/assets/af7fb86d-d059-4e25-9981-ffa3a1f3d444" />
<img width="1110" height="746" alt="image" src="https://github.com/user-attachments/assets/76d0a0b7-40bd-40e8-92f3-ba508970e036" />
- 打印main函数第一条指令
<img width="2060" height="1130" alt="image" src="https://github.com/user-attachments/assets/977f735a-cde5-459a-bf83-a8d992637359" />
- 修改进程空间指令，实现替换函数
  <img width="2096" height="1132" alt="image" src="https://github.com/user-attachments/assets/6a75c025-fc1c-4e7c-a0b7-48487b42849e" />
- vdso是内核和用户态的快捷通道，比如gettime、getcpu这种函数直接通过vdso获取省去了从内核获取
